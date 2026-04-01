docker-compose down
docker-compose build --no-cache auth-back
docker-compose up -d



Verificar si responde:
curl -I http://localhost:9000

### LINUX
 ¿Hay un archivo Dockerfile?
  ls -la | grep -i dockerfile

--------
Que hace este comando
instal bun :    curl -fsSL https://bun.sh/install | bash
