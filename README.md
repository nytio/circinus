# Despliege de proyecto con Docker

Instrucciones para despliegue de proyecto con docker, usando el programa docker-hub.

# Genera imagen principal
cd docker/v2023/
docker login -u USUARIO --password CONTRASEÑA
docker build --no-cache -t USUARIO/circinus:v2023 .
docker push USUARIO/circinus:v2023

* Nota: las imágenes principales ya están compiladas y publicadas.

# Comandos de limpieza [CUIDADO! SOLO PARA DESARROLLO]
docker rm -f $(docker ps -aq)
docker rmi -f $(docker images -aq)
docker volume rm $(docker volume ls -q)
docker network prune
docker builder prune

# Levanta los servicios
docker-compose up -d --build

# Entra a un servicio
docker ps
docker exec -it ccaa02cfec04 bash

# Cierra los servicios
docker-compose down

# Proceso de actualización de servicio
* Realiza respaldo de base de datos @todo
docker-compose down
docker rmi -f $(docker images -aq)
* Modifica yml para apuntar a la nueva versión
docker-compose up -d --build

