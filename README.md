# Despliege de proyecto con Docker

Instrucciones para despliegue de proyecto con docker, usando el programa docker-hub para almacenar las imágenes compiladas.

## Genera imagen principal
```bash
cd docker/v2024/
docker login -u USUARIO --password CONTRASEÑA
docker rmi -f nytio/circinus:v2024
docker build --no-cache -t nytio/circinus:v2024 .
docker push nytio/circinus:v2024
```

* Nota: las imágenes principales ya están compiladas y publicadas.

## Comandos de limpieza [CUIDADO! SOLO PARA DESARROLLO]
```bash
docker rm -f $(docker ps -aq)
docker rmi -f $(docker images -aq)
docker volume rm $(docker volume ls -q)
docker network prune
docker builder prune
```

## Levanta los servicios
```bash
docker compose up -d
```

## Entra a un servicio
```bash
docker ps
docker exec -it xcaa02cfec04 bash
docker exec -it -u shiny xcaa02cfec04 bash
docker run -it --entrypoint /bin/bash nytio/circinus:v2024
docker exec -it circinus-shiny-1 bash
```

## Cierra los servicios
```bash
docker compose down
```

## Proceso de actualización de servicio
```bash
docker compose down
docker rmi -f $(docker images -aq)
docker compose up -d
```

> Realiza respaldo de base de datos y modifica yml para apuntar a la nueva versión.

## Otros comandos
Revisa puertos abiertos:
```bash
sudo lsof -i -P -n | grep LISTEN
```

Administrar redes:
```bash
docker network ls
docker network inspect circinus_default
docker network create red_local
docker network rm red_local
```
