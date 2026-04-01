
### Pasos a ejecutar (en orden)

```
PASO 1: Limpiar mapas de estado en memoria (restart auth-back)
        → El eventProcessor tiene estados en memoria (volqueteStateMap, etc.)
        → Necesitamos que arranque limpio

PASO 2: DELETE todos los eventos lafecha indicada
        → DELETE FROM evento;  

PASO 3: SET processedAt = NULL en transmisiones 
        → UPDATE transmision SET "processedAt" = NULL, estado = NULL, referencia = NULL
          WHERE timestamp >= fecha deseada

PASO 4: Trigger el reprocesamiento
        → Llamar POST /api/reporte-origen (o cualquier endpoint que invoque processTransmisionEvents)

```

Que paso con volquete V-6 desde el 21 de marzo no presenta spikes 

**V-06**: Los spikes se resolvieron después del 21 marzo
**V-03 y V-10  Tienen el mismo problema** :  Revisar la instalación física — verificar que el enchufe de cigarrera haga buen contacto.
V-03 solo tiene datos de **4 días** en los últimos 20 dias . Solo 20,622 transmisiones vs el promedio de ~160,000. Esto confirma:

- El dispositivo se apaga frecuentemente por **problemas de alimentación eléctrica**
- Cuando se enciende, funciona bien por unas horas y luego se desconecta
- La batería LiPo interna (1050 mAh, 3.7V) del Pro5 Lite sirve como **respaldo**, pero:
    - Si la cigarrera se desconecta, la batería tarda **~3-4 horas en cargarse al 100%** desde vacía
    - Si fue el primer encendido del día y la batería estaba descargada → el GPS solo funciona mientras tenga alimentación directa de la cigarrera
    - Una desconexión momentánea (vibración del volquete afloja el enchufe) = GPS se apaga instantáneamente si la batería no tiene carga

**El gap de 31 minutos a las 03:03** se explica así: el volquete encendió a las ~02:50, el GPS se alimentó por cigarrera y empezó a transmitir a las 02:55. A las 03:03 la vibración aflojó  la conexión → sin batería cargada, se apagó de inmediato. A las 03:34 se reconectó.