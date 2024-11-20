# Informe del Proyecto: Implementación y Depuración de un Recolector de Basura en C

## Introducción

En este proyecto, desarrollamos un recolector de basura personalizado en lenguaje C para gestionar la memoria dinámica y prevenir fugas de memoria. Durante el desarrollo, nos enfrentamos a un error persistente que afectaba la funcionalidad del programa. Para abordar este problema, inicialmente creamos un **diagrama lógico de las listas enlazadas** para comprender mejor la estructura y el flujo de datos. Además, utilizamos herramientas como **git** para el control de versiones y agregamos mensajes de depuración enviados al **stderr** para rastrear el comportamiento del programa.

Este informe detallará el proceso seguido para identificar y corregir el error, así como las mejoras realizadas al diseño de la biblioteca, siguiendo los tres puntos propuestos.

---

## 1. Identificación de la Causa del Error

### Análisis Inicial y Diagrama Lógico

Para entender el problema, primero elaboramos un **diagrama lógico de las listas enlazadas** que representaban las estructuras `MemoryEntry` y `PointerNode`. Este diagrama nos permitió visualizar cómo se almacenaban y manejaban las referencias a las memorias asignadas y cómo interactuaban las funciones de la biblioteca.

**[Aquí se insertaría la imagen del diagrama lógico de las listas enlazadas]**

El diagrama nos ayudó a identificar posibles puntos de fallo en la gestión de punteros y referencias.

### Herramientas y Metodologías Utilizadas

- **Mensajes de Depuración**: Añadimos múltiples `fprintf(stderr, ...)` en puntos clave del código para monitorear el flujo de ejecución y el estado de las variables.
- **Experimentos Controlados**: Realizamos pruebas específicas con casos simplificados para aislar y reproducir el error de manera consistente.
- **Uso de Git**: Empleamos git para registrar cambios incrementales y facilitar la comparación entre versiones del código, ayudándonos a rastrear cuándo y dónde surgió el error.

### Proceso de Depuración

1. **Revisión de la Función `reverse`**:
    
    - Observamos que la función `reverse` llamaba a `unlinkMemory(newFig)` inmediatamente después de crear y copiar `newFig`.
    - Esto resultaba en la desvinculación y potencial liberación de la memoria asignada a `newFig` antes de que pudiera ser utilizada, lo que causaba accesos inválidos a memoria liberada en funciones posteriores.
2. **Análisis de Referencias y Memorias**:
    
    - Verificamos el conteo de referencias en `memoryList` antes y después de cada operación.
    - Identificamos que algunas memorias estaban siendo liberadas prematuramente debido a la gestión incorrecta de los punteros.

### Causa Identificada

El error se debía a que se desvinculaban y liberaban las memorias asignadas a `newFig` en la función `reverse` antes de que fueran utilizadas en otras partes del programa. Esto generaba accesos a memoria liberada y causaba comportamiento indefinido.

---

## 2. Corrección del Error y Ajustes al Código

### Acciones Realizadas

1. **Reubicación de la Desvinculación de Memoria**:
    
    - Movimos la llamada a `unlinkMemory(newFig)` de la función `reverse` a la función `display`, después de que `newFig` haya sido utilizada.
        
    - **Código Modificado en `reverse`**:
        
        c
        
        Copiar código
        
        `char** reverse(char** fig){   // ... código existente ...   return newFig; }`
        
    - **Código Modificado en `display`**:
        
        c
        
        Copiar código
        
        `void display(){   char** Pawn = reverse(pawn);   interpreter(Pawn);   unlinkMemory(Pawn); // Desvinculamos después de usar Pawn   garbageCollector(); }`
        
2. **Validación de Punteros y Contadores**:
    
    - Implementamos comprobaciones adicionales en las funciones `unregisterPointer` y `garbageCollector` para asegurarnos de que no se intentara liberar memoria ya liberada o punteros nulos.
    - Mejoramos la gestión de los contadores de referencias para reflejar correctamente el número de punteros activos a cada memoria.

### Resultado de las Correcciones

- **Estabilidad Mejorada**: El programa dejó de experimentar fallos relacionados con accesos a memoria liberada.
- **Gestión Correcta de la Memoria**: Las memorias se liberan únicamente cuando ya no hay punteros que las referencien.

---

## 3. Simplificación del Diseño de la Biblioteca

### Justificación

El profesor consideró que el error era irrecuperable debido a un problema de diseño en la solución. Se nos solicitó simplificar el diseño de la biblioteca sin cambiar su API, pero permitiendo cambios en su implementación para resolver el problema de manera efectiva.

### Cambios Implementados

1. **Reemplazo de Listas de Punteros por Contadores de Referencias**:
    
    - Simplificamos la estructura `MemoryEntry` eliminando la lista enlazada de `PointerNode` y añadiendo un contador de referencias (`refCount`).
        
    - **Nueva Estructura `MemoryEntry`**:
        
        c
        
        Copiar código
        
        `typedef struct MemoryEntry{   void* memory;   int refCount;   struct MemoryEntry* next; } MemoryEntry;`
        
2. **Actualización de las Funciones de Gestión de Memoria**:
    
    - **Asignación de Memoria (`memoryAlloc`)**:
        
        c
        
        Copiar código
        
        `void memoryAlloc(void** pointer, size_t size){   *pointer = malloc(size);   if(!(*pointer)){     fprintf(stderr, "Error al asignar memoria\n");     return;   }   MemoryEntry* entry = createMemoryEntry(*pointer);   entry->refCount = 1; // Inicia con una referencia   entry->next = memoryList;   memoryList = entry; }`
        
    - **Registro de Nuevas Referencias (`registerPointerToMemory`)**:
        
        c
        
        Copiar código
        
        `void registerPointerToMemory(void* existing_memory){   MemoryEntry* current = memoryList;   while(current){     if(current->memory == existing_memory){       current->refCount++;       return;     }     current = current->next;   }   fprintf(stderr, "Error: la memoria especificada no está registrada\n"); }`
        
    - **Desvinculación de Referencias (`unregisterPointer`)**:
        
        c
        
        Copiar código
        
        `void unregisterPointer(void* memory){   MemoryEntry* current = memoryList;   while(current){     if(current->memory == memory){       current->refCount--;       if(current->refCount < 0){         fprintf(stderr, "Error: contador de referencias negativo\n");       }       return;     }     current = current->next;   } }`
        
3. **Simplificación del Recolector de Basura (`garbageCollector`)**:
    
    - Ahora, el recolector de basura libera la memoria cuando el contador de referencias es cero.
        
        c
        
        Copiar código
        
        `void garbageCollector(){   MemoryEntry* prevEntry = NULL;   MemoryEntry* currentEntry = memoryList;   while(currentEntry){     if(currentEntry->refCount > 0){       prevEntry = currentEntry;       currentEntry = currentEntry->next;     }else{       free(currentEntry->memory);       if(prevEntry)         prevEntry->next = currentEntry->next;       else         memoryList = currentEntry->next;       MemoryEntry* toFree = currentEntry;       currentEntry = currentEntry->next;       free(toFree);     }   } }`
        

### Beneficios de la Simplificación

- **Reducción de la Complejidad**: Al eliminar las listas de punteros, simplificamos significativamente el manejo de referencias.
- **Facilidad de Mantenimiento**: El código es más fácil de entender y mantener, reduciendo la probabilidad de errores futuros.
- **API Inalterada**: La interfaz de la biblioteca permanece igual, por lo que no es necesario modificar el código que la utiliza.

### Consideraciones

- **Registro y Desvinculación de Punteros**: Es crucial que cada vez que se cree o elimine una referencia a una memoria, se actualice el contador de referencias correctamente mediante `registerPointerToMemory` y `unregisterPointer`.
- **Validaciones Adicionales**: Implementamos comprobaciones para evitar que el contador de referencias caiga por debajo de cero y para asegurar que las memorias están registradas antes de operar sobre ellas.

---

## Conclusión

A través de un análisis detallado y el uso de diagramas lógicos para entender la estructura de las listas enlazadas, logramos identificar y corregir el error que afectaba al recolector de basura personalizado. La simplificación del diseño de la biblioteca, reemplazando las listas de punteros por contadores de referencias, resultó en una solución más robusta y fácil de mantener.

El uso de herramientas de depuración y control de versiones fue esencial en este proceso, permitiéndonos rastrear cambios y comprender el comportamiento del programa. Este proyecto destaca la importancia de un buen diseño y de estar dispuesto a adaptar la solución cuando se enfrentan problemas complejos.

---

## Recomendaciones

- **Documentación Detallada**: Mantener una documentación clara de las funciones y estructuras para facilitar el entendimiento y mantenimiento futuros.
- **Pruebas Exhaustivas**: Implementar pruebas unitarias y de integración para verificar el correcto funcionamiento de la biblioteca en diversos escenarios.
- **Uso de Bibliotecas Existentes**: Considerar el uso de soluciones ya probadas, como bibliotecas de gestión de memoria y recolectores de basura para C, cuando sea posible.

---

## Anexos

### Diagrama Lógico de las Listas Enlazadas

_Figura 1: Representación gráfica de las estructuras `MemoryEntry` y `PointerNode` en la lista enlazada._

En este diagrama, se muestra cómo las estructuras `MemoryEntry` y `PointerNode` se organizaban inicialmente en listas enlazadas para gestionar las referencias a las memorias asignadas. Cada `MemoryEntry` contenía una lista de `PointerNode` que representaban los punteros que apuntaban a esa memoria.

Tras la simplificación, la estructura `MemoryEntry` ahora incluye un contador de referencias (`refCount`), eliminando la necesidad de manejar listas de punteros complejas y mejorando la eficiencia y legibilidad del código.

---

Espero que este informe completo cumpla con tus expectativas y refleje adecuadamente el proceso seguido en el proyecto, incluyendo la elaboración del diagrama lógico de las listas para entender el problema.