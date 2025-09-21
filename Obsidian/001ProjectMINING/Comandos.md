docker-compose down
docker-compose build --no-cache auth-back
docker-compose up -d



Verificar si responde:
curl -I http://localhost:9000

SONARQube
Metanoia1992#

token:
squ_ff56b0cb83c05eb3bccda83890af4cad2482bb41