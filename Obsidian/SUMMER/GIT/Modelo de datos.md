En términos generales, un **modelo de datos** es una forma de organizar, estructurar y gestionar la información dentro de un sistema. Es una representación abstracta de cómo se almacenan y manipulan los datos, especificando cómo las entidades del mundo real se representan y las relaciones entre ellas.



Git usa un modelo de datos basado en **objetos** y una estructura **de grafo dirigido acíclico (DAG)** para modelar la historia y los cambios en los archivos de un proyecto.

Aquí te explico las partes clave del modelo de datos en Git:

1. **Objetos en Git**: Git organiza los datos en tres tipos de objetos fundamentales:
    
    - **Blob**: Representa un archivo. Es una secuencia de bytes (como una especie de archivo almacenado sin ninguna estructura adicional, solo su contenido).
    - **Tree**: Representa un directorio o carpeta. Un árbol (tree) es una estructura de datos que mapea nombres de archivos y directorios a blobs o subdirectorios.
    - **Commit**: Un commit es un "snapshot" de tu proyecto en un punto en el tiempo. Es una referencia a un árbol (y sus archivos), y contiene metadatos como la fecha, el autor y el mensaje asociado a ese commit.