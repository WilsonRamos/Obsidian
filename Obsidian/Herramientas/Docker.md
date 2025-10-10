
![[Pasted image 20251004205904.png]]

---
# 🐳 Comandos Docker Esenciales 

| Comando                                                                | Concepto Técnico Esencial                                                                                                                                                                                                                                                     |
| ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `docker build -t mi-app:v1 .`                                          | **Tag (`-t`) nombra la imagen** - Docker construye en capas usando UnionFS. Cada instrucción del Dockerfile crea una capa inmutable cacheada. Las capas se reutilizan entre builds (copy-on-write)                                                                            |
| `docker images`                                                        | **Lista imágenes locales** - Imágenes = capas apiladas read-only. Son plantillas inmutables sin estado. Múltiples imágenes comparten capas base (ahorro de espacio)                                                                                                           |
| `docker volume create datos-app`                                       | **Crea volumen named** - Volumen = almacenamiento gestionado por Docker fuera del UnionFS. Sobrevive al ciclo de vida del contenedor. Bypasea el copy-on-write                                                                                                                |
| `docker run -d -p 8080:80 -v datos-app:/app/data --name app mi-app:v1` | **Crea contenedor desde imagen** - Agrega capa read-write sobre capas read-only. `-d`=daemon mode, `-p`=NAT port mapping (iptables), `-v`=monta volumen bypaseando UnionFS. Contenedor = proceso aislado con namespaces (PID, NET, MNT, IPC, UTS) + cgroups (límites CPU/RAM) |
| `docker ps`                                                            | **Lista contenedores activos** - Muestra procesos en ejecución con namespaces aislados. Estado="Up" significa proceso principal (PID 1 del namespace) corriendo                                                                                                               |
| `docker ps -a`                                                         | **Todos los contenedores** - Incluye estados: Created, Restarting, Running, Paused, Exited. La capa read-write persiste hasta eliminar el contenedor                                                                                                                          |
| `docker logs app`                                                      | **Streams stdout/stderr** - Docker captura todo lo escrito a file descriptors 1 y 2 del proceso PID 1. Los logs NO están dentro del contenedor, están en `/var/lib/docker/containers/`                                                                                        |
| `docker exec -it app bash`                                             | **Ejecuta proceso en namespace existente** - Crea nuevo proceso dentro de los namespaces del contenedor. `-i`=mantiene STDIN abierto, `-t`=asigna pseudo-TTY                                                                                                                  |
| `docker stop app`                                                      | **Envía SIGTERM a PID 1** - Detención graceful (espera 10s), luego SIGKILL. El contenedor pasa a estado "Exited". La capa read-write se preserva                                                                                                                              |
| `docker start app`                                                     | **Reinicia proceso detenido** - Reutiliza la MISMA capa read-write. No recrea el contenedor, solo reinicia el proceso con los mismos namespaces y filesystem                                                                                                                  |
| `docker rm app`                                                        | **Elimina contenedor** - Destruye la capa read-write del UnionFS y libera namespaces/cgroups. Los volúmenes montados NO se eliminan (persisten independientemente)                                                                                                            |
| `docker run -d -p 8080:80 -v datos-app:/app/data --name app mi-app:v1` | **Nuevo contenedor, MISMOS datos** - Nueva capa read-write (filesystem limpio) pero volumen contiene estado anterior. Volúmenes existen fuera del ciclo de vida del contenedor                                                                                                |
| `docker volume ls`                                                     | **Lista volúmenes** - Volúmenes = directorios en `/var/lib/docker/volumes/` gestionados por Docker. Persisten independiente de contenedores. Permiten compartir datos entre contenedores                                                                                      |
| `docker inspect app`                                                   | **Detalles completos en JSON** - Metadatos: namespaces IDs, cgroups, network config, mounts, layers, environment. Muestra la verdadera arquitectura interna del contenedor                                                                                                    |
| `docker system prune -a --volumes`                                     | **Limpieza total del sistema** - Elimina: contenedores detenidos, imágenes sin tags, capas huérfanas, build cache, volúmenes no montados. Libera espacio del graph driver                                                                                                     |

---

## 🧠 Arquitectura Fundamental de Docker
---

## 🎯 Diferencias Fundamentales


![[Pasted image 20251004210426.png]]

