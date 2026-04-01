### 2. Cada conexion en detalle

#### CONEXION 1: Dispositivos GPS → ruptela (TCP)

|Aspecto|Detalle|
|---|---|
|**Tipo**|TCP socket (binario)|
|**Quien inicia**|Dispositivo GPS Ruptela|
|**Quien responde**|`ruptela` modulo|
|**Puerto**|6000|
|**Pasa por nginx**|**NO** - mapeado directo `ports: "6000:6000"` en docker-compose|
|**Autenticacion**|Ninguna. El TCP server acepta cualquier conexion|
|**Archivo servidor**|[services/tcpServer.js:103](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/ruptela/services/tcpServer.js) → `net.createServer(...)`|
|**Archivo config**|[config/constants.js](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/ruptela/config/constants.js) → `TCP_PORT: 6000`|
|**Datos**|Paquetes binarios hex del protocolo Ruptela (IMEI, coords, velocidad, IO)|
|**Respuesta**|ACK hex `0002640113BC` enviado de vuelta al dispositivo|

---

#### CONEXION 2: auth-front → ruptela (WebSocket)

|Aspecto|Detalle|
|---|---|
|**Tipo**|WebSocket (WS / WSS)|
|**Quien inicia**|`auth-front` (el navegador del usuario)|
|**Quien responde**|`ruptela` modulo|
|**URL desarrollo**|`ws://localhost:5000`|
|**URL produccion**|Configurada via `VITE_WS_URL` en .env|
|**Pasa por nginx**|**SI** en produccion (upgrade WS via `ws.ruptela.santiago.maxtelperu.com:443`)|
|**Autenticacion**|Token cifrado AES-256-CBC enviado en el primer mensaje|
|**Archivo cliente**|[context/GpsWebSocketInit.tsx:27](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-front/src/context/GpsWebSocketInit.tsx) → `new WebSocket(wsUrl)`|
|**Archivo servidor**|[services/websocketService.js:14](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/ruptela/services/websocketService.js) → `new WebSocketServer({ server: httpServer })`|
|**Mensajes cliente→servidor**|`{ type: 'authenticate', token }` y `{ type: 'ping' }` cada 15s|
|**Mensajes servidor→cliente**|`{ type: 'gps-data', data: {...} }` y `{ type: 'state-change', data: {...} }`|
|**Reconexion**|Automatica cada 5 segundos si se desconecta|

**Segunda conexion WS** (analytics):

|Aspecto|Detalle|
|---|---|
|**Archivo**|[hooks/useAnalytics.ts:189-190](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-front/src/hooks/useAnalytics.ts)|
|**URL produccion**|`wss://api.santiago.maxtelperu.com` (hardcodeado, `USE_AWS = true`)|
|**Nota**|Apunta a `api.santiago.maxtelperu.com` que es el VIRTUAL_HOST de **auth-back**, no de ruptela. Pero auth-back no tiene servidor WebSocket. Esto sugiere que nginx rutea WS de ese dominio a ruptela, o que esta URL esta mal configurada|

---

#### CONEXION 3: auth-front → auth-back (HTTP/HTTPS REST)

|Aspecto|Detalle|
|---|---|
|**Tipo**|HTTP REST (fetch API)|
|**Quien inicia**|`auth-front` (navegador)|
|**Quien responde**|`auth-back` (Express)|
|**URL desarrollo**|`http://localhost:3000`|
|**URL produccion**|`https://api.santiago.maxtelperu.com`|
|**Pasa por nginx**|**SI** en produccion (TLS termination + proxy a :3000)|
|**Autenticacion**|JWT Bearer token en header `Authorization`|
|**Archivo configuracion URL**|[auth/authConstants.ts:1-5](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-front/src/auth/authConstants.ts) → decision por `window.location.hostname`|
|**Archivo servidor**|[index.js:221](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-back/index.js) → `app.listen(port)`|
|**CORS**|Configurado en [index.js:29-48](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-back/index.js) con `FRONTEND_ORIGIN` y `FRONTEND_ORIGIN_2`|

**Endpoints principales** (archivo → ruta):

|Archivo frontend|Metodo|Endpoint|Archivo backend|
|---|---|---|---|
|`Login.tsx:29`|POST|`/api/login`|`routes/login.js`|
|`Signup.tsx:22`|POST|`/api/signup`|`routes/signup.js`|
|`requestNewAccessToken.ts:5`|POST|`/api/refresh-token`|`routes/refreshToken.js`|
|`PortalLayout.tsx:25`|DELETE|`/api/signout`|`routes/logout.js`|
|`AuthProvider.tsx:146`|GET|`/api/user`|`routes/user.js`|
|`GoogleMapStatic.tsx:74,102,129`|GET|`/api/maquinarias?tipo=`|`routes/maquinaria.js`|
|`Recursos.tsx:202,213`|CRUD|`/api/personal`, `/api/maquinarias`|`routes/personal.js`, `routes/maquinaria.js`|
|`Reports.tsx:412,437,522`|POST|`/api/reporte-origen`, `/api/reporte-chute`, `/api/reporte-cargador`|`routes/reporte-origen.js`, `routes/reporte-chute.js`|
|`useAnalytics.ts:153`|GET|`/api/analytics/analytics`|`routes/analytics.js`|
|`Recursos.tsx:261,272`|CRUD|`/api/personal-turno`, `/api/horario`|`routes/personalTurno.js`, `routes/horario.js`|

---

#### CONEXION 4: ruptela → PostgreSQL (TCP/Sequelize)

|Aspecto|Detalle|
|---|---|
|**Tipo**|TCP (protocolo PostgreSQL via Sequelize ORM)|
|**Quien inicia**|`ruptela`|
|**Quien responde**|PostgreSQL|
|**Host**|`process.env.DB_HOST` o `"postgres"` (nombre del container Docker)|
|**Puerto**|5432 (default PostgreSQL)|
|**DB**|`santiago`|
|**Archivo**|[config/database.js:7-22](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/ruptela/config/database.js) → `new Sequelize(...)`|
|**Tablas que ESCRIBE**|`transmision` (registros GPS), `maquinaria` (ultima posicion)|
|**Pool**|max 10 conexiones, 30s acquire, 10s idle|

---

#### CONEXION 5: auth-back → PostgreSQL (TCP/Sequelize)

|Aspecto|Detalle|
|---|---|
|**Tipo**|TCP (protocolo PostgreSQL via Sequelize ORM)|
|**Quien inicia**|`auth-back`|
|**Quien responde**|PostgreSQL|
|**Host**|`process.env.DB_HOST` o `"postgres"`|
|**DB**|`santiago` (la MISMA que ruptela)|
|**Archivo**|[config/database.js:6-21](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-back/config/database.js)|
|**Tablas que LEE**|`transmision`, `maquinaria`, `evento` (escritas por ruptela)|
|**Tablas que ESCRIBE**|`personal`, `horario`, `personal_turno`, `maquinaria`, `evento`|

---

#### CONEXION 6: auth-back → MongoDB (TCP/Mongoose)

|Aspecto|Detalle|
|---|---|
|**Tipo**|TCP (protocolo MongoDB via Mongoose)|
|**Quien inicia**|`auth-back`|
|**Quien responde**|MongoDB|
|**Connection string**|`process.env.DB_CONNECTION_STRING`|
|**Archivo**|[index.js:60](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-back/index.js) → `mongoose.connect(...)`|
|**Proposito**|Autenticacion de usuarios y sesiones (login, signup, tokens)|

---

#### CONEXION 7: auth-back → Gmail SMTP

| Aspecto            | Detalle                                                                                                                                                                 |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Tipo**           | SMTP (TCP puerto 465/587)                                                                                                                                               |
| **Quien inicia**   | `auth-back`                                                                                                                                                             |
| **Quien responde** | Gmail servers                                                                                                                                                           |
| **Archivo**        | [utils/sendEmail.js:6-12](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-back/utils/sendEmail.js) → `nodemailer.createTransport(...)` |
| **Credenciales**   | `SMTP_USER`, `SMTP_PASS` en .env                                                                                                                                        |

---

### 3. Donde esta nginx

Nginx **no tiene archivos de configuracion propios** en el proyecto. Usa la imagen `jwilder/nginx-proxy` que genera configuracion dinamica leyendo variables de entorno de los otros containers Docker en la red `ruptela_net`.

|Archivo|Que define|
|---|---|
|[login/docker-compose.yml:3-16](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/docker-compose.yml)|El servicio nginx-proxy, puertos 80/443, volumenes de certificados|
|[login/docker-compose.yml:17-30](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/docker-compose.yml)|Let's Encrypt companion para certificados SSL automaticos|
|[login/ruptela/docker-compose.yml:12](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/ruptela/docker-compose.yml)|`VIRTUAL_HOST=ws.ruptela.santiago.maxtelperu.com`|
|[login/auth-back/docker-compose.yml:10](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-back/docker-compose.yml)|`VIRTUAL_HOST=api.santiago.maxtelperu.com`|
|[login/auth-front/docker-compose.yml:10](vscode-webview://0lo6u4ecq8v5h4l1mho1okio4sgcvaolgkdkhv5e8uidd00nvo5d/login/auth-front/docker-compose.yml)|`VIRTUAL_HOST=www.santiago.maxtelperu.com,santiago.maxtelperu.com`|

**Mecanismo**: `jwilder/nginx-proxy` detecta automaticamente cada container con `VIRTUAL_HOST` en la red Docker y genera un bloque `server {}` que hace proxy_pass al `VIRTUAL_PORT` del container.