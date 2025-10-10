
-- ACTUALIZAR UBICACIÓN DE FRENTE1
UPDATE maquinaria 
SET 
  latitud = -14.6830648475000000,
  longitud = -69.5000068475000000,
  ultima_pos = ST_Point(-69.5000068475000000, -14.6830648475000000)
WHERE id = 35;





-- Obtener el punto promedio de la excavadora durante los momentos de carga
WITH momentos_carga AS (
  -- Obtener todos los timestamps donde HAY volquetes en CARGANDO INICIO
  SELECT DISTINCT
    DATE_TRUNC('minute', timestamp) as minuto_carga,
    MIN(timestamp) as inicio_minuto,
    MAX(timestamp) as fin_minuto
  FROM transmision
  WHERE estado = 'CARGANDO INICIO'
    AND "processedAt" IS NOT NULL
    AND timestamp >= NOW() - INTERVAL '24 hours'
    AND imei != '868018071302858'  -- Excluir la excavadora, solo volquetes
  GROUP BY DATE_TRUNC('minute', timestamp)
),
posiciones_excavadora AS (
  -- Obtener las posiciones de la excavadora durante esos momentos
  SELECT 
    t.id,
    t.timestamp,
    t.latitude,
    t.longitude,
    t.imei
  FROM transmision t
  INNER JOIN momentos_carga mc 
    ON t.timestamp >= mc.inicio_minuto 
    AND t.timestamp <= mc.fin_minuto
  WHERE t.imei = '868018071302858'
    AND t.latitude IS NOT NULL
    AND t.longitude IS NOT NULL
    AND t.latitude BETWEEN -90 AND 90
    AND t.longitude BETWEEN -180 AND 180
)
SELECT 
  '🎯 PUNTO PROMEDIO DE LA EXCAVADORA DURANTE CARGAS' as info,
  COUNT(*) as total_registros_excavadora,
  AVG(latitude) as latitud_promedio,
  AVG(longitude) as longitud_promedio,
  STDDEV(latitude) as desviacion_lat,
  STDDEV(longitude) as desviacion_lng,
  MIN(timestamp) as primer_registro,
  MAX(timestamp) as ultimo_registro,
  -- Validación de calidad
  CASE 
    WHEN COUNT(*) < 5 THEN '⚠️ Pocos registros (<5)'
    WHEN STDDEV(latitude) > 0.001 OR STDDEV(longitude) > 0.001 THEN '⚠️ Alta dispersión'
    ELSE '✅ Datos buenos'
  END as calidad_datos
FROM posiciones_excavadora;

-----------------------------------------

-- Ver el cambio que se va a realizar
WITH 
punto_nuevo AS (
  SELECT 
    -14.6830648475000000 as latitud_promedio,
    -69.5000068475000000 as longitud_promedio
),
estado_actual AS (
  SELECT 
    nombre_maquinaria,
    tipo_maquinaria,
    imei,
    latitud as lat_actual,
    longitud as lng_actual,
    fecha as fecha_actual
  FROM maquinaria
  WHERE imei = '868018071302858'
)
SELECT 
  '📍 ESTADO ACTUAL' as seccion,
  ea.nombre_maquinaria,
  ea.tipo_maquinaria,
  ea.imei,
  ea.lat_actual,
  ea.lng_actual,
  ea.fecha_actual,
  '' as separador,
  '🎯 NUEVO PUNTO (PROMEDIO)' as seccion2,
  pn.latitud_promedio as lat_nueva,
  pn.longitud_promedio as lng_nueva,
  '' as separador2,
  '📏 CAMBIO' as seccion3,
  ROUND(
    6371000 * ACOS(
      LEAST(1.0, GREATEST(-1.0,
        COS(RADIANS(pn.latitud_promedio)) * COS(RADIANS(ea.lat_actual)) *
        COS(RADIANS(ea.lng_actual) - RADIANS(pn.longitud_promedio)) +
        SIN(RADIANS(pn.latitud_promedio)) * SIN(RADIANS(ea.lat_actual))
      ))
    )
  ) as distancia_cambio_metros,
  CASE 
    WHEN ROUND(
      6371000 * ACOS(
        LEAST(1.0, GREATEST(-1.0,
          COS(RADIANS(pn.latitud_promedio)) * COS(RADIANS(ea.lat_actual)) *
          COS(RADIANS(ea.lng_actual) - RADIANS(pn.longitud_promedio)) +
          SIN(RADIANS(pn.latitud_promedio)) * SIN(RADIANS(ea.lat_actual))
        ))
      )
    ) > 500 THEN '⚠️ CAMBIO GRANDE (>500m) - Revisar'
    WHEN ROUND(
      6371000 * ACOS(
        LEAST(1.0, GREATEST(-1.0,
          COS(RADIANS(pn.latitud_promedio)) * COS(RADIANS(ea.lat_actual)) *
          COS(RADIANS(ea.lng_actual) - RADIANS(pn.longitud_promedio)) +
          SIN(RADIANS(pn.latitud_promedio)) * SIN(RADIANS(ea.lat_actual))
        ))
      )
    ) > 100 THEN '⚠️ Cambio moderado (>100m) - Verificar'
    ELSE '✅ Cambio aceptable (<100m) - Proceder'
  END as validacion
FROM estado_actual ea
CROSS JOIN punto_nuevo pn;
