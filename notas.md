# Docker


## Instalacion

```bash
sudo apt install docker.io
```


LInux tiene un comando `systemctl` que lo que hace es habilitar,deshabilitar,prender o apagar un servicio en segundo plano.

```bash
sudo systemctl enable --now docker
```

```bash
sudo docker run hello-world
```

## Imagenes

Son como una **`plantilla`**, solo esta guardada, no es algo en ejecucion.

```bash
docker images
```

## Contenedores

Un contenedor es una **`imagen ejecutandose`**.
Se puede descargar y ejecutar un contenedor con ``run``

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