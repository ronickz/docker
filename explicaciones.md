# Algunos conceptos


## Canillas
Un programa puede tener varias salidas
0. `stdin` entrada normal al programa. 
1. `stdout` salida normal del programa, ej, `echo "hola"`
2. `stderr` salida erronea de un programa, ej. `ls directorio que no existe`


Al correr por canales sepados, `canal 1: stdout` y `canal 2: stderr` la pipeline `|` **solo toma el stdout**.

Para tomar tanto el stdout como el stderr se realiza
```bash
docker logs mongo 2>&1 | lnav
```