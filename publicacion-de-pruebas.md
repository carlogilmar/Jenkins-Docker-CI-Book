# Publicación de Pruebas

1. Creación del servidor nginx con Docker
2. Estructura de Directorios
3. Publicación de pruebas a través del Jenkinsfile

### Creación de Servidor Nginx con Docker

Es necesario levantar un contendor de Docker con Nginx a partir de la siguiente imagen:

```
docker run --name CONTAINER-NAME DOCKER-PORT:HOST-PORT PATHS -d nginx
```

Sugerencia:

```
docker run --name nginx -p 8081:80 -v /u01/folder_nginx:/usr/share/nginx/html:ro -d nginx
```

### Estructura de directorios

En el home del servidor donde esta iniciado el contenedor de Docker es necesario tener un directorio llamado **folder\_nginx**

Dentro de este directorio se podrá crear un directorio con el nombre del proyecto, y dentro de él dos directorios más nombrados **devl** y **qa**

Estructura necesaria:

* **folder\_nginx**
  * **nombre\_\_del\_\_proyecto**
    * **devl**
    * **qa**

![](/assets/nginx1.png)

## Publicación de Pruebas a través del Jenkins File

En el **Jenkinsfile** será necesario agregar un stage más para realizar la publicación de pruebas en el servidor nginx previamente desplegado con Docker, dónde se copie los archivos correspondientes a los archivos de pruebas dentro de la carpeta previamente generada en **folder\_nginx**

Ejemplo:

```
stage('Publish result test') {
      steps{
        dir("appServer"){
          echo "Publish result test"
          sh "scp -r -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no core/build/reports/tests/test/* DAWS03LX@172.31.100.25:/u01/folder_nginx/rutas-docentes/devl"
        }
      }
}
```

![](/assets/pruebas1.png)

