![[Pasted image 20250116211222.png]]

Git es un sistema de control de versiones[[Sistemas Distribuidos | distribuido]] que permite rastrear nuestros archivos manteniendo un historial detallado de los cambios en archivos y permitiendo la colaboración entre múltiples desarrolladores.
Se basa en un [[Modelo de datos]] que registra instantáneas de los estados de los archivos en lugar de guardar diferencias, garantizando así la integridad de los datos mediante el uso de hashes criptográficos.

## Modelo de datos de GIT
[[GIT.excalidraw]]



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

![[Pasted image 20250116212414.png]]![[Pasted image 20250116215854.png]]![[Pasted image 20250116220114.png]]