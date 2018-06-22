# Uso Básico de Docker![](/assets/ebc3.png)

1. Ejemplo de Dockerfile
2. Construir una imagen de Docker
3. Iniciar un contenedor de Docker a partir de una imagen
4. Trabajando con imágenes de Docker Hub
5. Descargar una imagen de Docker Hub
6. Iniciar un contendor a partir de una imagen de Docker Hub
7. Comandos básicos
8. Políticas de Reinicio
9. Entrar a la bash del contenedor

### Ejemplo de Dockerfile

```
#This is a sample Image 
FROM ubuntu 
MAINTAINER demousr@gmail.com 

RUN apt-get update 
RUN apt-get install –y nginx 
CMD [“echo”,”Image created”]
```

También puedes consultar images preexistentes en [Docker Hub](https://hub.docker.com/)

### Construir una Imagen de Docker

```
docker build dockerfile
```

### Iniciar un contendor de Docker a partir de una imagen

```
docker run docker-image-built
```

# Trabajando con imágenes de Docker Hub

### Descargar una imagen de Docker Hub

```
docker pull docker-hub-image

Ejemplo:

docker pull jenkins/jenkins
```

### Iniciar un contendor de Docker a partir de una imagen de Docker Hub

Consultar la documentación de la imagen previamente descargada

```
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
```

## Comandos Básicos

### Listar imágenes de Docker

```
docker image list
```

### Eliminar imagen de Docker

```
docker image rm image-name
```

### Listar contenedores de Docker

```
docker ps --all
```

### Detener un contenedor

```
docker stop docker-container-id
```

### Eliminar un contenedor

```
docker rm docker-container-id
```

### Ver el log de un contenedor

```
docker logs docker-container-id
```

## Políticas de Reinicio de contenedores

Docker puede implementar políticas de reinicio.

```
docker run -dit --restart unless-stopped ...
```

La opción  **-dit --restart unless-stopped **reiniciará el contenedor cuando este haya sido irrumpido por fallos, o por reinicio en el server.

### Entrar a la bash del contenedor

```
docker exec -i -t container-name /bin/bash
```





