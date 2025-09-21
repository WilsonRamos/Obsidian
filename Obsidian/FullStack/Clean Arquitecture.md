Bajo acoplamiento Alta Cohesion


soporte
casos de uso : Reglas de negocio especificas de la aplicacion

circulos externos deven comunicarse con los circulos internos(no con los externos nunca)
nnuestras entidades

### comandos para instalacion

npm i -D typescript @types/node ts-node-dev rimraf

npx tsc --init --outDir dist/ --rootDir src

# Clean Arquitecture
Principio Fundamental -> las dependencias del codigo fuente soolo pueden apunntar hacia adentro. las capas internas no deben conocer nada sobre las capas externas.

Presentation : es lo mas extreno de nuestro  circulo y los cerca  a los usuarios que van a consumir nuestro API aqui esta exprees etc

 Domain : aqui esta la informacion las reglas que gobiernan la aplicacion  casos de uso, adaptadores, data sources, repositorios
 esto no tiene que tener dependencias externans
 los adaptadores(entes externos) no deben tener injerencia sobre las reglas de negocio
infrastructure : punto inteermedio crear las implementacion de los adaptadores,repositorios, mappers
