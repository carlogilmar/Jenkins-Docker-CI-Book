Creación del Contenedor de Jenkins Master![](/assets/ebc7.png)Para instalar un Jenkins con Docker es necesario usar la siguiente imagen:

[https://hub.docker.com/r/jenkins/jenkins/](https://hub.docker.com/r/jenkins/jenkins/)

Para esta creación será necesario habilitar ciertas opciones:

* Container Name 
* Habilitar el JNLP Protocol 3
* Mapear los puertos del contenedor
* Específicar el jenkins home 

Desde línea de comando es necesario crear el contenedor con esta imagen con el siguiente comando:

> docker run -d --name **CONTAINER-NAME** -e **PROTOCOL-ENABLE=TRUE-OR-FALSE **-p **HOST-PORT:DOCKER-PORT** -v **VOLUME:VOLUME-DIR-IN-DOCKER** **DOCKER-IMAGE**

Ejemplo:

> docker run -d - - name **jenkins-master** -e **JNLP\_PROTOCOL\_OPTS=-Dorg.jenkinsci.remoting.engine.JnlpProtocol3.disabled=false** -p **8080:8080** -p **50000:50000** -v **jenkins\_home:/var/jenkins\_home** jenkins/jenkins

Una vez realizado esto se podrá contar con el container de Jenkins.

## Comprobación

Para comprobar que el container esté corriendo se pueden listar los contenedores de docker con la siguiente instrucción:

> docker ps

```
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                                              NAMES
1b22ad819d99        jenkins/jenkins      "/sbin/tini -- /usr/…"   About an hour ago   Up About an hour    0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   agitated_keldysh
```

## Instalación de Jenkins

Una vez creado el contenedor será necesario ingresar al navegador para operar la interfaz gráfica de Jenkins:

> IP-DEL-SERVIDOR:8080

La instalación de Jenkins por vez primera solicita un Token de acceso, para obtenerlo será necesario revisar el log de Docker por medio de la línea de comando:

> docker logs CONTAINER-IR

El container ID puede ser identificado a través del comando **docker ps**

Una vez ingresado el token de inicio, se podrá dar de alta el usuario administrador.

