  

### Modelo de objetos Git:
- **Commits**: Snapshots con metadata y punteros a parents
- **Trees**: Estructura de directorios
- **Blobs**: Contenido de archivos
- **Tags**: Referencias a commits específicos
## Operacion de Rollback Destructivo con Reescriltura de Historial Remoto
Esta secuencia implementa un rollback destructivo coordinado que reescribe el historial del repositorio remoto y sincroniza todos los clientes locales al nuevo estado.

Los commits posteriores al target(commit donde redirigiremos HEAD pointer) quedan unreachable en el DAG (Directed Acyclic Graph) pero permanecen en el object store hasta la próxima garbage collection.


```bash
1. Avisa al equipo:"Voy a hacer reset a [COMMIT] en 5 minutos"

2. Reset local
git reset --hard [COMIT]

3. Push directo
git push --force origin main

4. Avisa que terminaste(en cualquier otra maquina despues)
git fetch origin origin &&
git reset --hard origin/main

git log --oneline -1

```


**Mecanismos de recuperación:**
- Reflog mantiene referencias a commits "perdidos" durante ~30 días