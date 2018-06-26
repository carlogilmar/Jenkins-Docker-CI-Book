# Instalación de Jenkins Slave

1. Creación de Slaves en el Jenkins Master
2. Creación del Jenkins Slave
3. Creación de la imagen de Docker
4. Creación del contenedor del Jenkins Slave
5. Instalación de Maven
6. Políticas de Reinicio de contenedores

![](/assets/ebc6.png)

## Creación de Slaves en el Jenkins Master

Una vez creado el Jenkins Master se deberá crear un Slave a través de la **Interfaz Gráfica**

**Administrar Jenkins -&gt; Nodos -&gt; Nuevo Nodo**

* Agregar un nombre
* Método de Ejecución: **Launch agent vía Java Web Start** ![](/assets/jenkins1.png)

Al registrar el Slave, veremos la siguiente imagen, donde es necesario identificar el token.![](/assets/jenkins2.png)

## Creación del Jenkins Slave

Será necesario **habilitar la comunicación entre el servidor que contiene los Jenkins Slave y el servidor que pertenece al ambiente dónde se ejecutarán las aplicaciones **\(desarrollo, QA, o producción\). Para esto debemos generar una conexión vía SSH desde la imagen de Docker que generará el Jenkins Slave.

### Construcción de la imagen de Docker

A  continuación generaremos la imagen para poder crear el contenedor del Jenkins Slave y que a su vez tenga habilitada la conexión ssh al servidor donde se desplegará:

**Pasos a seguir**:

1.- En un directorio vacío será necesario copiar los archivos **id\_rsa** y **id\_rsa.pub **que están en el directorio **.ssh/. **

2.- Dentro de este directorio crear un archivo llamado **Dockerfile **con la sigiuente estructura:

```
FROM jenkins/jnlp-slave

RUN rm -rf /home/jenkins/.ssh
RUN mkdir .ssh

COPY id_rsa.pub /home/jenkins/.ssh/id_rsa.pub.bk
COPY id_rsa /home/jenkins/.ssh/id_rsa.bk

RUN cp -p /home/jenkins/.ssh/id_rsa.bk /home/jenkins/.ssh/id_rsa
RUN cp -p /home/jenkins/.ssh/id_rsa.pub.bk /home/jenkins/.ssh/id_rsa.pub

RUN chmod 600 /home/jenkins/.ssh/id_rsa
RUN chmod 600 /home/jenkins/.ssh/id_rsa.pub

RUN rm /home/jenkins/.ssh/id_rsa.bk
RUN rm /home/jenkins/.ssh/id_rsa.pub.bk
```

3.- Generar la imagen a partir del **Dockerfile** con el siguiente comando

```
docker build . -t DOCKER_IMAGE_NAME
```

_Sugerencia:_

```
docker build . -t ebc/jenkins/jnlp-slave
```

Para comprobar que se generó la imagen correctamente, con el siguiente comando se podrán enlistar las imágenes de Docker disponibles donde se podrá ver el nombre de la imagen anteriormente creada:

```
docker images
```

### Creación del contenedor del Jenkins Slave

Para crear el contenedor a partir de esta imagen podemos usar el siguiente comando:

> docker run -d --name **CONTAINER-NAME** **DOCKER-IMAGE-NAME** -url **URL-JENKINS-MASTER**  -workDir=**DIRECTORY-IN-DOCKER SLAVE-TOKEN** **SLAVE-NAME**

Donde especificaremos:

* Container name
* Docker image name
* URL Jenkins Master
* Working Directory in Docker
* Slave Token
* Slave Name

_Sugerencia:_

> docker run -d --name **slave-1** **ebc/jenkins/jnlp-slave** -url [http://172.31.100.23:8080](http://172.31.100.23:8080)  -workDir=**/home/jenkins/agent** **1bcf169330dbcc01bcfd55953dad7646e67c0fabf106302667b310dc90c6d357** **slave-1**

##### Con este paso realizado deberá cambiar el estado del Slave creado en el Jenkins Master.

### **Para la creación de más Jenkins Slaves no es necesario volver a generar la imagen de Docker, sólo será necesario crear más contenedores con diferentes nombres. **

### Instalación de Maven en Jenkins Slaves

En ocasiones puede ser requerida la instalación de** maven \(.m2\)** en los Jenkins Slave para poder ejecutar las pruebas y construcciones.

1.- Copiar la carpeta comprimida de **.m2 **en el Jenkins Slave

```
docker cp repository.zip slave-4:/home/jenkins/
```

2.- Entrar a la shell del Jenkins Slave y descomprimir la carpeta **.m2**

```
docker exec -i -t slave-4 /bin/bash

mkdir .m2

unzip repository.zip

mv repository .m2/
```

## Políticas de Reinicio de contenedores

Docker puede implementar políticas de reinicio.

```
docker run -dit --restart unless-stopped ...
```

La opción  **-dit --restart unless-stopped **reiniciará el contenedor cuando este haya sido irrumpido por fallos, o por reinicio en el server.

Ejemplo:

```
docker run -dit --restart unless-stopped -d
--name slave-1 ebc/jenkins/jnlp-slave -url http://172.31.100.23:8080
-workDir=/home/jenkins/agent
1bcf169330dbcc01bcfd55953dad7646e67c0fabf106302667b310dc90c6d357 slave-1
```

Con el comando anterior inciamos un contenedor de Jenkins Slave con políticas de reinicio.

---

# IMPORTANTE: Para el correcto funcionamiento de los slaves es necesario entrar al Docker Container y clonar cualquier repositorio de BitBucket Desarrollo EBC.

##### Ver los contenedores activos

```
docker ps
```

##### Entrar a la bash del contenedor

```
docker exec -i -t container-name /bin/bash
```


