# Deploy a GlassFish

1. Despligue de aplicaciones en GlassFish
2. Detalle de los archivos necesarios
   1. DockerFile para la imagen 
   2. Script de despligue 

### Despliegue de aplicaciones en GlassFish

Una vez que el Jenkins haya construido el pipeline indicado, será necesario desplegar la aplicación. Para ello generamos anteriormente una conexión ssh entre el servidor que contiene al Jenkins, y el servidor de despliegue. \(Ver Instalación de Jenkins Slaves\)

La forma automatizada de realizar esto es creando lo siguiente en el home del usuario del servidor de despligue:

1. Script de despliegue
2. Dockerfile para la imagen del glassfish
3. [Jar OJDBC](http://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html)

**\*\*\* El directorio **_ebcHub_** contiene archivos que usa el aplicativo que esta desplegándose **

**\*\*\* El directorio **_rutas-docentes_** contiene los archivos war y .properties de la aplicación para desplegar, este directorio es poblado automáticamente por el proceso de integración continua**

![](/assets/gf2.png)

# Detalle de los archivos

### **Dockerfile** para la imagen de Glassfish

El siguiente Dockerfile se construirá sobre una imagen preexistente, configurando los parámetros necesarios para desplegar la aplicación.

**Dockerfile**

```
FROM oracle/glassfish:5.0

ENV GLASSFISH_LIB=$GLASSFISH_HOME/glassfish/domains/domain1/lib/ext/

COPY ojdbc8.jar $GLASSFISH_LIB

ARG PATH_FOLDER
RUN echo $PATH_FOLDER
COPY $PATH_FOLDER/*.war $GLASSFISH_HOME/glassfish/domains/domain1/autodeploy/

RUN mkdir /root/.project_config

ARG FILE_NAME_CONF
COPY $PATH_FOLDER/*.properties /root/.project_config/

RUN mkdir /root/files

COPY $PATH_FOLDER/domain.xml $GLASSFISH_HOME/glassfish/domains/domain1/config
```

## Scripts de despligue

Los siguiente script tiene por finalidad construir la imagen de Docker con un servidor GlassFish junto con la configuración necesaria para desplegar la aplicación.

* Script para generar el contenedor con Glassfish 5
* Script para generar el contenedor con Glassfish 4
* Script para generar el contenedor para aplicación Grails

#### Script para generar el contenedor con Glassfish 5

**deployApp.sh**

```
#!/bin/sh

PATH_FOLDER=$1
FILE_NAME_CONF=$2
PROJECT_NAME=$3
VOLUME_NAME=$4

docker build --build-arg PATH_FOLDER=$PATH_FOLDER --build-arg FILE_NAME_CONF=$FILE_NAME_CONF . -t ebc/glassfish/$PROJECT_NAME

docker stop $PROJECT_NAME

docker rm $PROJECT_NAME

docker run --name $PROJECT_NAME -v $VOLUME_NAME -d -p 4848:4848 -p 8080:8080  ebc/glassfish/$PROJECT_NAME
```

### Script para generar el contenedor con Glassfish 4

deployAppGlassfish4.sh

```
#!/bin/sh

PATH_FOLDER=$1
PROJECT_NAME=$2
VOLUME_APP=$3
VOLUME_LOG=$4
PORT=$5
PORTADMIN=$6

docker build --build-arg PATH_FOLDER=$PATH_FOLDER --file=dockerFiles/DockerfileGlassfish4 . -t ebc/glassfish/$PROJECT_NAME

docker stop $PROJECT_NAME

docker rm $PROJECT_NAME

docker run -dit --restart unless-stopped --name $PROJECT_NAME -v $VOLUME_APP -v $VOLUME_LOG -d -p $PORTADMIN:4848 -p $PORT:8080  ebc/glassfish/$PROJECT_NAME
```

### Script para generar el contenedor para aplicación Grails

deployAppGrails.sh

```
#!/bin/sh

PATH_FOLDER=$1
PROJECT_NAME=$2
VOLUME_APP=$3
VOLUME_LOG=$4
PORT=$5
PORTADMIN=$6

docker build --build-arg PATH_FOLDER=$PATH_FOLDER --file=dockerFiles/DockerfileGrails . -t ebc/glassfish/$PROJECT_NAME

docker stop $PROJECT_NAME

docker rm $PROJECT_NAME

docker run -dit --restart unless-stopped --name $PROJECT_NAME -v $VOLUME_APP -v $VOLUME_LOG -d -p $PORTADMIN:4848 -p $PORT:8080  ebc/glassfish/$PROJECT_NAME
```



