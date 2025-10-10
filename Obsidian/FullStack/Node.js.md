
Lenguajes del lado del cliente 
lenaguajes del lado  del servidor

Chrome uitliza V8 dentro de navegador

![[Pasted image 20251003220732.png]]

Eslint - linter(analizador estatico de codigo)
ESLint es esencial para escribir JavaScript de alta calidad, detectar errores temprano y mantener un código consistente en proyectos de desarrollo.


REPL(Read,Eval,Print,Loop)->Teminal de Node  ejecutar node
![[Pasted image 20251003104758.png]]undefines : En JavaScript, las declaraciones de variables (const, let, var) no retornan ningún valor. El REPL siempre muestra el valor de retorno de cada expresión, y cuando no hay valor de retorno, muestra undefined.

 -> node tiene watch puedes usarlo en lugar de terceros como nodemon :

![[Pasted image 20251003112153.png]]

Administar de paquete de node NPM


pick chunk de lodash

Se puede modificar el tipado en javascript 
tipado dinamico string despus int 

otra forma de manejar valores compuestos es con objetos sont objeto = {tema : "node"}

como se ejecuta node dentro del servidor(En este caso mi servidor es mi local)
consolse.losg(process)
Guardar valoers sensibleds en una varible de entorno en el .env podemos guardar valores sensibles

### Importar paqueter externos usando el servicio en la nube de NPM
dos forma de inportar librerias que estan en la nube de npm

sintanis clasica de node :
	const {unique }= require("lodash")

javascript moderno : emmascript
type : Mod

import uniq from "losha/uniq.js"

--------------

[...] (rep,res)={} Conceptualmetne entiende estas dos variasle 
se hace una peticion a nuestro servidor

peticion del navegador al servidor
-------

Express gestionar peticiones a un servidor de node

---
![[Pasted image 20251003232624.png]]

Eejcutar una carga sincronica se detiene serviciso de node
import {readFileSync} from "node:fs"


HSABLame de los callbacken Node
este callback tiene dos parametros? por que 
![[Pasted image 20251004102555.png]]

El callback es solo el "buzón" donde readFile deposita los resultados.


### Debugger
se creo un wesocker ?
![[Pasted image 20251004160239.png]]

![[Pasted image 20251004160307.png]]

Tambien podemos debugear en el navegador

pero debemos crear el proceso en node
node --inpect server.js  ese proceso se reconoce en la iamgen
![[Pasted image 20251004161023.png]]


### Para hacer  pruebas unitarias 
*![[Pasted image 20251004163242.png]]*

Falta completar :
![[Pasted image 20251004171551.png]]