# Docker file imagen php

## Descripción
Este es un ejemplo de un archivo Dockerfile para crear una imagen de PHP.

### Pasos
Referencia: [FastCode](https://www.youtube.com/watch?v=-XnfBItOBHE)

1.- Creamos una carpeta src y dentro de ella un archivo index.php con el siguiente contenido:
```php
<?php  
    echo "Hola Mundo!"; 
?>
```

2.- Creamos un archivo Dockerfile con el siguiente contenido:
```Dockerfile
FROM php:7.2-apache
COPY /src /var/www/html
EXPOSE 80
```

3.- Construimos la imagen:
```bash
docker build -t mi_imagen .
```
Donde `mi_imagen` es el nombre que le daremos a nuestra imagen.

4.- Creamos un contenedor con la imagen creada:
```bash
docker run -d -p 8080:80 --name mi_contenedor mi_imagen
```
Donde `-d` hace que se ejecute  en segundo plano, `-p` mapea el puerto 8080 del host al puerto 80 del contenedor y `--name` le da un nombre al contenedor.

5.- Accedemos a `http://localhost:8080` y veremos el mensaje `Hola Mundo!`.

6.- Para parar y eliminar el contenedor:
```bash
docker stop mi_contenedor && docker rm mi_contenedor
```
Nota: Para hacer referencia al contenedor, se puede usar el nombre o el id del contenedor. De la id se puede usar solo los primeros 3 caracteres.

7.- Montar el directorio actual en el contenedor:
```bash
docker run -p 5000:80 -d -v $(pwd)/src:/var/www/html hello-php
```
Este comando se encarga de correr un contenedor con la imagen hello-php, mapeando el puerto 5000 del host al puerto 80 del contenedor, ejecutando el contenedor en segundo plano y montando el directorio actual en la carpeta /var/www/html del contenedor.

Es decir, copia el contenido de la carpeta actual (de nuestro escritorio local) en el contenedor, por lo que cualquier cambio que hagamos en la carpeta src se verá reflejado en el contenedor.

8.- Conectarme al contenedor:
```bash
docker exec -it mi_contenedor bash
```
Estando en esta ubicación puedo ingresar directamente al contenedor, de esta forma puedo crear contenido en el contenedor. Pero como ya estoy usando volumes, veré los cambios reflejados en mi directorio local.


9.- Ver los procesos del contenedor:
```bash
ps aux
```


### Comandos útiles:
```bash
# Ver todos los contenedores
docker ps -a
# Inspeccionar un contenedor
docker inspect mi_contenedor
# Ver las imagenes
docker images
# Detener un contenedor
docker stop mi_contenedor
# Eliminar una imagen
docker rmi mi_imagen
# Eliminar un contenedor
docker rm mi_contenedor
# Eliminar todas las imagenes
docker rmi $(docker images -q)
# Eliminar todos los contenedores
docker rm $(docker ps -aq)
```

