
antijammer
lectotes de tarjeta

# CONECTIVIDAD

## TECNOLOGIAS DE COMUNICACION (EL MODEN WAN)
- **LTE Cat 1:** Es una versión de 4G optimizada para IoT pero con mayor ancho de banda y menor latencia. Es ideal para vehículos que requieren transmisión de datos constante y en tiempo real (ej. streaming de datos de tacógrafo).
    
- **LTE Cat M1 / NB-IoT (Narrowband IoT):** Son tecnologías "Low Power Wide Area" (LPWA). Tienen una excelente penetración en interiores (sótanos/garajes) y consumen muy poca energía, pero su tasa de transferencia es baja.
    
- **2G Fallback:** Capacidad crítica. Si no hay cobertura 4G/LTE, el equipo "cae" a la red 2G (GSM/GPRS) para no perder conexión

## TECNOLOGIAS DE POSICIONAMIENTO(EL RECEPTOR GNSS)
Aqui el equipo escucha satelites no transmite nada solo recibe senales para triangular su posicion
- **GNSS (Global Navigation Satellite System):** Ruptela usa módulos **u-blox** que leen múltiples constelaciones simultáneamente: **GPS** (USA), **GLONASS** (Rusia), **Galileo** (Europa) y **BeiDou** (China).

## TECNOLOGIAS DE CONECTIVIDAD LOCAL(WPAN)
- **BLE (Bluetooth Low Energy):** Protocolo de radio de corto alcance.
        - **BLE 5.0 / 5.1:** Más alcance (hasta 4x más que el 4.2) y velocidad. Presente en **HCV5**, **Pro5**, **Plug5** .
        - **BLE 4.2:** Versión anterior, suficiente para sensores básicos. Presente en **Trace5 IP67**


# INTERFACES Y CONECTORES
## BUS CAN(Controller Area Network)
#### **FMS/J1939/J1708**: Protocolos específicos de CAN usados en camiones
Sistema de comunicacion interno del vehiculo permite leer datos directamente del computador del vehiculo

## K-Line 
Protocolo de diagnostivo mas antiguo 

## OBD / OBD-II
Puerto de diagnostico standar en vehiculos OBD II desde 1996
-> Punto de conexion plug and play
## RS232 / RS485 (Puertos Serie)
- **RS232:** Comunicación punto a punto. Se usa para conectar **un** periférico complejo, como una sonda de combustible digital de alta precisión, un sensor de fatiga o un lector RFID.
    
- **RS485:** Comunicación multipunto. Permite conectar **múltiples** sensores en cadena (daisy-chain), como varios sensores de temperatura y humedad en un remolque refrigerado.
- 
## 1 WIRE
Protocolos de comunicacion simple con un solo cable 

## DIN/DOUT/AIN (Entradas y Salidas)
- **DIN (Digital Input)**: Entrada digital para detectar estados ON/OFF
    - Ejemplo: Puerta abierta/cerrada, motor encendido/apagado
- **DOUT (Digital Output)**: Salida digital para controlar dispositivos
    - Ejemplo: Bloquear motor, activar alarma
- **AIN (Analog Input)**: Entrada análoga para medir valores variables
    - Ejemplo: Nivel de combustible (0-100%)


# Tarjeta de SIM Recomendadas

SIM Multicarrier -> cambia automaticamente entre carriers(Movistar, Claro)
Garantiza mejor cobertura nacional



------
para remolque y caja seca
GPS con bateria de larga duracion ponerlo en el chasis del vehiculo por el iman se queda pegado un par por cada trailer

------
rango de voltaje muy anplio 6 - 32
volquetes trabajan a 24 voltios

un gps funcionando y otro que sea el senuelo 
que es el señuelo 
le retiras el gps el equipo se apaga automaticamente



Factura
Esquema de subcripscion

Licencia de uso de la plataforma



-----
Empresa que se dedica a telemetria 

https://www.syscom.mx/categories/66523?sortBy=mas_vendidos&m=audio-y-video



