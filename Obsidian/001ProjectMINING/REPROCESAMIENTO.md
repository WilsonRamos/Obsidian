
### Pasos a ejecutar (en orden)

```
PASO 1: Limpiar mapas de estado en memoria (restart auth-back)
        → El eventProcessor tiene estados en memoria (volqueteStateMap, etc.)
        → Necesitamos que arranque limpio

PASO 2: DELETE todos los eventos
        → DELETE FROM evento;  (3,244 registros, no hay datos anteriores al 18 Feb)

PASO 3: SET processedAt = NULL en transmisiones desde Feb 18
        → UPDATE transmision SET "processedAt" = NULL, estado = NULL, referencia = NULL
          WHERE timestamp >= '2026-02-18 00:00:00';
        → 585,191 registros quedarán como "sin procesar"

PASO 4: Trigger el reprocesamiento
        → Llamar POST /api/reporte-origen (o cualquier endpoint que invoque processTransmisionEvents)
        → El procesador tomará los 585K registros en chunks de 5K
        → Al final, processEventsFromTimestamp generará eventos nuevos con la lógica corregida

PASO 5: Verificar resultados
        → Contar eventos generados
        → Verificar coherencia origen vs chute
```