
Un **sistema distribuido** es un conjunto de computadoras independientes que trabajan juntas para parecer un único sistema coherente a los usuarios. Estas computadoras, conocidas como **nodos**, están interconectadas mediante una red y colaboran para realizar tareas comunes, como almacenamiento, procesamiento o comunicación.

## Ejemplos de sistemas distribuidos:

1. **Sistemas de archivos distribuidos**: HDFS, Google File System.
2. **Bases de datos distribuidas**: Cassandra, MongoDB, y otros.
3. **Servicios de computación en la nube**: Amazon Web Services (AWS), Google Cloud Platform (GCP).
4. **Aplicaciones peer-to-peer**: BitTorrent.
5. **Sistemas de mensajería**: Kafka, RabbitMQ.


### Sistema de control de versiones distribuido
La Arquitectura de Git nos permite de cada usuario tenga una copiua completa del repositoria en su propia maquina

Cuando clonas un repositorio, obtienes no solo los archivos del proyecto, sino también todo el historial de cambios. Esto lo hace distribuido, ya que cada usuario tiene una réplica independiente del repositorio completo.

cada copia en autonoma y funcional

Puedes trabajar en tu copia local del repositorio de manera independiente sin necesidad de estar conectado a un servidor central para realizar la mayoria de las operaciones de Git : 
	editar mofidicar archivos localmente
	realizar commit
	crear nuevas ramas
	Resivar el historial compelto
puedes compartir estos cambios con otros(distribuir el flujo de trabajo) o integrar el trabajo realizado por otros desarrolladores
para ello git utiliza las siguientes operaciones
	* git push : Enviar commits
	* git pull:traer los cambios
	* git fetch