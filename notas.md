# Docker

## Instalacion

```bash
# sudo apt install docker.io
```

LInux tiene un comando `systemctl` que lo que hace es habilitar,deshabilitar,prender o apagar un servicio en segundo plano.

```bash
# sudo systemctl enable --now docker
```

Forma de probar docker

```bash
# sudo docker run hello-world
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
# srw-rw---- 1 root docker ... /var/run/docker.sock
```

#### De donde sale el grupo docker?

Al instalar docker, **`se crea un grupo de usuarios`**

```bash
cat /etc/groups | grep "docker"
```

Entonces, para no hacer un sudo en cada consulta. Se agrega el usuario actual a ese grupo docker.

```bash
sudo usermod -aG docker $USER
```

## Imagenes

Son como una **`plantilla`**, solo esta guardada, no es algo en ejecucion.

```bash
# docker images
```

```bash
# docker pull imagen:tag/version
```

```bash
# docker image imagen:tag/version
```

## Contenedores

Un contenedor es una **`imagen ejecutandose`**.
Se puede descargar una imagen y ejecutar un contenedor con `run`.

>Crear un contenedor nuevo y arrancarlo.

```bash
docker run nombre-imagen
```

Para ver todos los **`contenedores en ejecucion`**

```bash
docker ps
```

Para ver **`TODOS los contenedores`**(en ejecucion y apagados)

```bash
docker ps -a
```

## Volumenes

Para persistir los datos, se deben crear volumenes fisicos en el sistema.

```bash
#docker volume create mongo_data
```

## Ejemplo final con mongo

```bash

docker run -d \
  --name mongo-dev-noauth \
  -p 127.0.0.1:27017:27017 \
  -v mongo_data_noauth:/data/db \
  mongo:7

```