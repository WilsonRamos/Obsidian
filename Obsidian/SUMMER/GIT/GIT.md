![[Pasted image 20250116211222.png]]

Git es un sistema de control de versiones[[Sistemas Distribuidos | distribuido]] que permite rastrear nuestros archivos manteniendo un historial detallado de los cambios en archivos y permitiendo la colaboración entre múltiples desarrolladores.

Se basa en un [[Modelo de datos]]  que registra instantáneas de los estados de los archivos en lugar de guardar diferencias, garantizando así la integridad de los datos mediante el uso de hashes criptográficos.

## Modelo de datos de GIT

## **2. Estructura de Datos Interna de Git**

Git organiza y almacena la información mediante un sistema eficiente de **objetos**. Estos objetos son los bloques fundamentales que representan archivos, directorios, y commits en el historial del repositorio.

### **2.1 Tipos de Objetos en Git**

Existen tres tipos principales de objetos en Git:

1. **Blob**:
    
    - Representa el contenido de un archivo.
    - Es un simple arreglo de bytes sin ninguna información adicional como el nombre del archivo o la estructura de carpetas.
    
    plaintext
    
    CopyEdit
    
    `type blob = array<byte>`
    
2. **Tree**:
    
    - Representa la estructura de directorios del proyecto.
    - Un **tree** contiene referencias a blobs (archivos) y otros trees (subdirectorios), formando una jerarquía.
    
    plaintext
    
    CopyEdit
    
    `type tree = map<string, tree | blob>`
    
    - **Claves (`string`)**: Nombres de archivos o directorios.
    - **Valores (`tree | blob`)**: Referencias a otros trees o blobs.
3. **Commit**:
    
    - Representa un punto en el historial del repositorio.
    - Incluye:
        - **Parents**: Una lista de commits previos (los padres).
        - **Author**: El autor del commit.
        - **Message**: Un mensaje descriptivo sobre los cambios realizados.
        - **Snapshot**: Un **tree** que captura el estado del proyecto en ese momento.
    
    plaintext
    
    CopyEdit
    
    `type commit = struct {     parents: array<commit>     author: string     message: string     snapshot: tree }`
    

### **2.2 Base de Datos de Objetos**

Git utiliza una **base de datos clave-valor** para almacenar todos los objetos (blobs, trees y commits). Los identificadores únicos (claves) son hashes SHA-1 (o SHA-256 en versiones más recientes).

- **Almacenar un objeto**:
    
    plaintext
    
    CopyEdit
    
    `def store(o):     id = sha1(o)     objects[id] = o`
    
    - Se genera un hash a partir del contenido del objeto.
    - El objeto se guarda en la base de datos con el hash como clave.
- **Cargar un objeto**:
    
    plaintext
    
    CopyEdit
    
    `def load(id):     return objects[id]`
    
    - Dado un hash, Git recupera el objeto asociado.

### **2.3 Referencias**

Las referencias son punteros simbólicos que vinculan nombres legibles (como `main`, `feature`, o `HEAD`) con objetos específicos (generalmente commits).

- Representación:
    
    plaintext
    
    CopyEdit
    
    `references = map<string, string>`
    
    - **Claves (`string`)**: Nombres como `main`, `feature`, `HEAD`.
    - **Valores (`string`)**: Hashes de commits.

---

## **3. Relaciones Internas en el Modelo de Git**

Git organiza toda la información como un **grafo dirigido acíclico (DAG)**. Esto significa que:

1. **Commits como nodos**:
    
    - Cada commit es un nodo que referencia a uno o más commits anteriores.
    - Esto permite reconstruir el historial del proyecto de manera eficiente.
2. **Snapshot en cada commit**:
    
    - Cada commit guarda un **snapshot** del estado del proyecto.
    - El snapshot es un tree que contiene blobs (archivos) y referencias a otros trees (subdirectorios).
3. **Ramas como punteros**:
    
    - Una rama es simplemente un puntero simbólico que apunta a un commit.
    - Al realizar un nuevo commit, el puntero de la rama activa avanza al nuevo commit.
4. **Merge y commits con múltiples padres**:
    
    - Durante un merge, se crea un commit que tiene múltiples padres, representando la combinación de dos o más ramas.

---

## **4. Funcionamiento Interno de Git**

### **4.1 Creación de un Commit**

Cuando realizas un commit, Git realiza los siguientes pasos internamente:

1. Crea blobs para todos los archivos del proyecto.
2. Organiza los blobs en un tree que representa la estructura del directorio.
3. Crea un commit que referencia al tree y a los padres (commits anteriores).
4. Calcula el hash del commit y lo almacena en la base de datos.
5. Actualiza la referencia de la rama activa para que apunte al nuevo commit.

### **4.2 Navegación en el Historial**

Cuando ejecutas un comando como `git checkout` o `git log`, Git utiliza las referencias (`HEAD`, ramas, tags) para:

1. Identificar el hash del commit al que quieres acceder.
2. Recuperar el commit desde la base de datos de objetos.
3. Reconstruir el snapshot del proyecto navegando por los trees y blobs.

### **4.3 Fusión de Ramas (Merge)**

Durante un merge:

1. Git encuentra el **ancestro común** más reciente de las dos ramas.
2. Combina los cambios desde el ancestro común hasta el punto actual en ambas ramas.
3. Crea un nuevo commit con múltiples padres, representando la combinación de las ramas.

---

## **5. Ventajas del Modelo Interno de Git**

1. **Eficiencia**:
    
    - El uso de hashes garantiza que los datos sean únicos y fácilmente verificables.
    - El modelo basado en snapshots reduce la duplicación, ya que los objetos no cambian si su contenido no cambia.
2. **Flexibilidad**:
    
    - Las referencias simbólicas permiten trabajar con nombres amigables en lugar de hashes largos.
    - El DAG facilita la representación de un historial no lineal con ramas y merges.
3. **Integridad**:
    
    - Cada objeto está identificado de forma única por su hash, lo que asegura la consistencia y evita corrupción de datos.

---

## **Resumen Final**

Git organiza su información internamente mediante un sistema eficiente basado en objetos (`blob`, `tree`, `commit`) almacenados en una base de datos clave-valor identificada por hashes. Estas estructuras forman un **grafo dirigido acíclico (DAG)** que captura el historial del proyecto. Las **referencias** proporcionan punteros simbólicos a objetos dentro del DAG, facilitando la navegación y manipulación del repositorio.

Esta arquitectura hace que Git sea:

- Eficiente: Almacena snapshots compactos en lugar de diferencias.
- Flexible: Permite trabajar con ramas, merges y cambios no lineales.
- Confiable: Usa hashes para asegurar la integridad de los datos.

Este modelo avanzado es lo que permite a Git ser una herramienta poderosa y versátil en el desarrollo moderno de software.

## Flujo de Trabajo en GIT
![[GIT.excalidraw| 600]]
**_Notas importantes:_**

Configuracion de las variables globales :
    
```git
    $ git config --global user.name "Tu nombre de Usuario"
    $ git config --global user.email "tu@email.com"

```
    
- _Previamente, debes conocer la ruta o la ruta donde se encuentra almacenado tu proyecto **(carpeta o directorio)**, para evitar frustraciones al usar la terminal._

[[Python | Aprendiendo python]]

**Ver toda la configuración actual de Git**:

bash

CopyEdit

`git config --list`

Esto mostrará tanto la configuración global como la local, incluidas las credenciales, nombre de usuario, correo y más.

![[Pasted image 20250116212414.png]]![[Pasted image 20250116220114.png]]


Revisar el Video Del MIT
<iframe width="560" height="315" src="https://www.youtube.com/embed/2sjqTHE0zok?si=KUk6I7UXhmGofddG" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

You can find the lecture note ans exercises for this lecture at 
https://missing.csail.mit.edu/2020/version-control/