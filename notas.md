# Docker

## Instalacion

```bash
sudo apt install docker.io
```

LInux tiene un comando `systemctl` que lo que hace es habilitar,deshabilitar,prender o apagar un servicio en segundo plano.

```bash
sudo systemctl enable --now docker
```

Forma de probar docker

```bash
sudo docker run hello-world
```

## Funcionamiento interno

Cuando se instala docker, pasan algunas cosas:

Se instala un cliente `docker` y el servicio daemon `dockerd`, donde docker es el que consulta y dockerd el que gestiona los contenedores.

Como el daemon, realiza cosas pesadas sobre el sistema, necesita `permisos root`.

### Como es la comucacion entre cliente y servidor?

Se comunican a traves de un socket, `que es intermediario`, ubicado en

```bash
ls -l /var/run/docker.sock
```

En este archivo, solo tiene permisos root y el grupo `docker`

```bash
srw-rw---- 1 root docker ... /var/run/docker.sock
```

#### De donde sale el grupo docker?

Al instalar docker, **`se crea un grupo de usuarios`**

```bash
cat /etc/group | grep "docker"
```

Entonces, para no hacer un sudo en cada consulta. Se agrega el usuario actual a ese grupo docker.

```bash
sudo usermod -aG docker $USER
```

## Imagenes

Son como una **`plantilla`** que contiene todas las dependencias para ejecutarse, **NO** es algo en ejecucion.

```bash
docker images
```

```bash
docker pull imagen:tag/version
```

```bash
docker image imagen:tag/version
```

## Contenedores

Un contenedor es una **`imagen ejecutandose`**.

>Crear un contenedor nuevo y arrancarlo.

```bash
docker container run nombre-imagen
```

Para ver todos los **`contenedores en ejecucion`**

```bash
docker ps
```

Para ver **`TODOS los contenedores`**(en ejecucion y apagados)

```bash
docker ps -a
```
> Manejo de `instancias`

```bash
docker start
docker stop
docker restart
docker rm
```


Para ingresar a un contenedor y ejecutar comandos.

```bash
docker exec -it mongo-eze bash
```

## Variables de entorno

>-e siginifica enviroments docker run -d -e mysql_password=secret

## Logs

Para ver `que sucede` dentro de un contenedor

```bash
docker logs mongo 2>&1 | lnav
```

```bash
docker logs -f idcontainer
```

## Volumenes

Para persistir datos aunque el contenedor se borre, Docker usa volúmenes administrados por el motor Docker.


```bash
docker volume create mongo_data
```

```bash
docker volume ls
```


***Named Volume***: Docker administra donde guardarlo

```bash
-v mongo_data:/data/db
```

***Bind Mount***: Yo elijo el origen, esto permite un intercambio bidireccional

```bash
-v /home/eze/proyecto:/app
```
```bash
docker container run --name nest-app -w /app -dp 80:3000 -v "$(pwd)":/app node:18.20.8-alpine3.21 sh -c "yarn install && yarn start:dev"
```

## Redes
Dos contenedores tienen comunicacion entre si, si estan en la misma red

```bash
docker network create
```

```bash
docker network ls
```
```bash
docker network connect world-app 4051
```

## Como ingresar a un contenedor

```bash
docker exec -it container /bin/sh
```

## A INVESTIGAR
Layers y reutilizacion




docker container run \
--name world-db \
-dp 3306:3306 \
-e MARIADB_USER=example-user \
-e MARIADB_PASSWORD=user-password \
-e MARIADB_ROOT_PASSWORD=root-secret-password \
-e MARIADB_DATABASE=world-db \
--volume world-db:/var/lib/mysql \
--network world-app \
mariadb:jammy

docker container run \
--name phpmyadmin \
-d \
-e PMA_ARBITRARY=1 \
-p 8080:80 \
--network world-app \
phpmyadmin:5.2.0-apache

phpmyadmin:5.2.0-apache