# Contenedores de Docker

- Creación de volúmenes
- Creación de logs
- Instalación de Nginx
- Instalación de Nexus

## Creación de Volúmenes

Agregar la siguiente bandera al comando para crear un contenedor:

```

-v path-a-mapear:path-del-contenedor

Ejemplo:

docker run -dit --name nginx -p 8081:80 -v /u01/folder_nginx:/usr/share/nginx/html -d nginx

```

## Creación de logs

Será necesario mapear el directorio de logs del contenedor como volumen.

## Instalación de nginx con Docker

```
docker run -dit --restart unless-stopped
--name nginx -p 8081:80 -v /u01/folder_nginx:/usr/share/nginx/html:ro -d nginx

```

## Instalación de Nexus con Docker

```
docker run -dit --restart unless-stopped
-d -p 8081:8081 -v /u01/sonatype-work/nexus:/sonatype-work --name nexus sonatype/nexus:oss
```
