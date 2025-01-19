


![[DiagramaHash.excalidraw]]



Una **función hash** es un algoritmo determinista que toma una entrada de longitud variable, que puede ser cualquier tipo de datos (como texto, imágenes, archivos binarios, etc.), y la transforma en una salida de longitud fija, llamada valor de hash o _digest_. La salida tiene una representación compacta y única de los datos originales. Las funciones hash están diseñadas para cumplir varias propiedades fundamentales:


- **Determinismo**: Para una misma entrada, siempre se genera la misma salida.
- **Resistencia a preimagen**: Es computacionalmente inviable recuperar la entrada original a partir del valor de hash.
- **Resistencia a colisiones**: Es extremadamente difícil encontrar dos entradas diferentes que generen el mismo valor de hash.
- **Disposición uniforme**: Cambios pequeños en la entrada producen un cambio significativo y aparentemente aleatorio en la salida.

En el ámbito de la **criptografía**, las funciones hash se utilizan para garantizar la **integridad de los datos** y prevenir manipulaciones. Son esenciales para la generación de **firmas digitales**, el almacenamiento seguro de **contraseñas**, y la validación de la **integridad de archivos**. Además, juegan un papel crucial en la construcción de estructuras de datos como **tablas hash**, que permiten búsquedas eficientes, y **árboles de Merkle**, que permiten la verificación de grandes volúmenes de datos en sistemas distribuidos.

En aplicaciones modernas como **blockchains**, las funciones hash aseguran la inmutabilidad de los registros al vincular bloques de datos de manera irreversible. Además, la seguridad de estas funciones ha sido un tema central en el desarrollo de nuevas versiones, como **SHA-2** o **BLAKE2**, que mejoran la resistencia contra ataques como **colisiones** o **fuerza bruta**, problemas presentes en algoritmos más antiguos como **MD5** o **SHA-1**.


En términos simples, una función hash convierte datos complejos en una "huella digital" única, que puede usarse para verificar la integridad o realizar comparaciones rápidas.

### 2. **Tipos de Funciones Hash**
#### a. **Funciones Hash Criptográficas**
Estas funciones están diseñadas para garantizar la seguridad de los datos y son ampliamente utilizadas en la criptografía moderna. Ejemplos incluyen:

- **SHA-2 (Secure Hash Algorithm 2)**: Un conjunto de funciones hash que incluyen SHA-224, SHA-256, SHA-384 y SHA-512. Son bastante seguras y ampliamente adoptadas en protocolos como TLS/SSL, Bitcoin, y otros sistemas criptográficos.
    
- **SHA-3**: El estándar más reciente de la familia SHA, basado en el algoritmo Keccak. A pesar de ser considerado más seguro que SHA-2, no ha tenido la misma adopción en la práctica.
    
- **BLAKE2**: Un hash criptográfico rápido y seguro diseñado como una mejora sobre SHA-2 y MD5, que es resistente a ataques de colisión y preimagen.
    
- **MD5 y SHA-1**: Aunque en su momento fueron populares, estos algoritmos ahora se consideran inseguros debido a su vulnerabilidad a ataques de colisión. A pesar de esto, todavía se usan en contextos no críticos o donde el rendimiento es más importante que la seguridad.
    

#### b. **Funciones Hash No Criptográficas**

![[Recording 20250118113851.m4a]]

Estas funciones son utilizadas principalmente en estructuras de datos, como las tablas hash, donde la seguridad no es una preocupación principal. Los ejemplos incluyen:

- **MurmurHash**: Una función hash no criptográfica rápida y eficiente, utilizada comúnmente en bases de datos y sistemas de almacenamiento distribuido.
    
- **CityHash**: Desarrollada por Google, es una función hash diseñada para distribuir las claves de manera uniforme en tablas hash. Es rápida y eficiente, pero no está diseñada para ser segura.
    
- **FNV-1 (Fowler–Noll–Vo)**: Una familia de funciones hash rápidas y eficientes, utilizadas en aplicaciones donde la velocidad y la simplicidad son prioritarias.


Revisar este video del MIT :  

<iframe width="560" height="315" src="https://www.youtube.com/embed/Nu8YGneFCWE?si=fDpamhVb6VZCiARl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

