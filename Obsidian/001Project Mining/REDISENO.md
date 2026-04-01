
**PROBLEMA 3: Sin detección de duplicados** No existe constraint UNIQUE en `(imei, timestamp, recordIndex)`. Si un paquete se recibe dos veces (retransmisión TCP), se insertan filas duplicadas que luego generan eventos duplicados.
**PROBLEMA 4: Posiciones de excavadoras desactualizadas** Las posiciones de excavadoras se guardan en un `Map` en memoria del servicio Ruptela (`excavadoraPosMap`). Pero el **eventProcessor.js** en auth-back carga posiciones de la tabla `maquinaria` al inicio del chunk, no del Map en tiempo real. Si la excavadora se movió durante el procesamiento del chunk, se usa posición obsoleta.
**PROBLEMA CRITICO 1: ACK antes de confirmar escritura en DB**

```
socket.write(ACK)     ← ACK enviado INMEDIATAMENTE
await saveRecord()    ← INSERT es async, puede fallar
```

El dispositivo cree que el paquete fue guardado, pero si el INSERT falla, **el dato se pierde sin reintento**. El dispositivo no reenviará el paquete porque ya recibió ACK.

**PROBLEMA 2: Sin límite de buffer** En `tcpServer.js`, el buffer crece indefinidamente si llega un header corrupto con `packetLength` muy grande. No hay timeout para paquetes incompletos — memory leak potencial.

## PARA EVENTOS 

**PROBLEMA CRITICO 5: Ciclos incompletos se pierden silenciosamente**

Los eventos solo se crean cuando un ciclo **se completa** (transición DESCARGANDO_FINAL → DESCARGADO, [línea 814-824](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/service/auth-back/routes/eventProcessor.js#L814-L824)). Si un volquete:

- Carga pero nunca llega al chute → **0 eventos**
- Llega al chute pero el tiempo de gracia no se cumple → **0 eventos**
- El proceso se reinicia mid-ciclo → el `cicloEnProgresoMap` se pierde → **0 eventos**

```javascript
// línea 818-822: Solo crea eventos si TODOS los 4 timestamps existen
if (cicloCompleto && cicloCompleto.cargaInicio && cicloCompleto.cargaFinal &&
    cicloCompleto.descargaInicio && cicloCompleto.descargaFinal) {
  eventsInChunk += await crearEventosCicloInline(imei, cicloCompleto);
}
```

**PROBLEMA 6: Estado en memoria volátil**

Todos los estados viven en Maps de Node.js (`volqueteStateMap`, `cicloEnProgresoMap`, etc.). Si el contenedor Docker se reinicia, **todo el contexto de ciclos en progreso se pierde**. La función `inicializarEstados` ([línea 190](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/service/auth-back/routes/eventProcessor.js#L190)) recupera el último estado de la DB, pero NO recupera el `cicloEnProgresoMap` — los timestamps intermedios del ciclo se pierden.

**PROBLEMA 7: TIMEOUT silencioso de CARGADO**

Si un volquete permanece en CARGADO más de 90 minutos ([línea 659](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/service/auth-back/routes/eventProcessor.js#L659)), se resetea a DESCARGADO sin crear eventos. Esto puede ocurrir legítimamente si el volquete espera turno en el chute durante cambio de turno.

**PROBLEMA 8: Concurrencia en procesamiento batch**

El procesamiento en chunks re-consulta `WHERE processedAt IS NULL` para cada chunk ([línea 1080](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/service/auth-back/routes/eventProcessor.js#L1080)). Si el servicio Ruptela inserta nuevos registros mientras se procesa, pueden mezclarse registros de timestamps diferentes, rompiendo la secuencia temporal por IMEI.


## Esto es de copiloto
CRÍTICO 1: Lock de concurrencia en memoria (no distribuido)

// Línea ~64-66let isGlobalProcessing = false;let processingStartTime = null;let currentProcessingSource = null;
Problema: El lock es una variable en memoria del proceso Node.js. Si hay múltiples instancias (PM2 cluster, Docker replicas), cada instancia tiene su propio lock y pueden procesar los mismos registros simultáneamente.

Impacto:

Eventos duplicados
Condiciones de carrera en la máquina de estados
Corrupción de la secuencia de estados

C

**P2: Emparejamiento frágil INICIO-FINAL**

- 
- 
- 
- 

- Busca el **primer** FINAL posterior al INICIO. Si hay eventos desordenados (latencia de procesamiento, GPS con timestamp retrasado), puede emparejar incorrectamente
- No valida que FINAL sea del **mismo ciclo** — si un ciclo pierde su FINAL y el siguiente tiene FINAL, se empareja cruzado

### ALTO-5: El reporte-chute pierde descargas si la descarga ocurre >2h después de la carga

**Archivo**: [reporte-chute.js:193](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/service/auth-back/routes/reporte-chute.js#L193)

```javascript
const ventanaExtendida = new Date(fechaFin.getTime() + 2 * 60 * 60 * 1000);
```

Si un volquete carga al final del turno T4 (17:50) y descarga a las 20:30 (turno nocturno, mantenimiento, cola larga), la ventana de 2h no alcanza. Recomendación: vincular por `ciclo` en vez de por ventana temporal, ya que el eventProcessor ya genera los 4 eventos de ciclo juntos con el mismo `tiempo_ciclo_seg`.

## 3. ANÁLISIS DE IDEMPOTENCIA Y TOLERANCIA A FALLOS

### Es idempotente?

**Parcialmente**. La tabla `transmision` tiene UNIQUE `(imei, timestamp, recordIndex)` → retransmisiones del dispositivo se ignoran con `ignoreDuplicates`. Pero:

- Si `processTransmisionEvents` falla a mitad de un chunk, los records de ese chunk NO tienen `processedAt` seteado aún (el bulk UPDATE es al final del chunk). **El siguiente ciclo los reprocesa**, pero el cicloEnProgresoMap en memoria ya avanzó → puede generar estados inconsistentes.
- La verificación de duplicado en `crearEventosCicloInline` (findOne por CARGANDO INICIO + timestamp) protege contra doble creación de eventos.
![[Pasted image 20260318224811.png]]'


![[Pasted image 20260320101539.png]]


KPI:

![[Pasted image 20260323212500.png]]