# Despliege de proyecto con Docker

Instrucciones para despliegue de proyecto con docker, usando el programa docker-hub para almacenar las imágenes compiladas.

## Genera imagen principal
```bash
cd docker/v2023/
docker login -u USUARIO --password CONTRASEÑA
docker build --no-cache -t USUARIO/circinus:v2023 .
docker push USUARIO/circinus:v2023
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
```
