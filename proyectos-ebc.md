# Proyectos EBC

* Servers Disponibles
* Server CI
* Server Desarrollo
* Server QA
* Publicación de Pruebas en Submódulos
* Proyectos Migrados

### Servers Disponibles

* Jenkins: [http://172.31.100.23:8080/](http://172.31.100.23:8080/) 
* Desarrollo: [http://172.31.100.25/](http://172.31.100.25/)
* QA: [http://172.31.100.24/](http://172.31.100.24/)![](/assets/ebc2.png)

### Server CI \([http://172.31.100.23](https://legacy.gitbook.com/book/carlogilmar/docker-jenkins/edit#)\)

Este server contendrá lo siguiente:

* Directorio **images\_**_**jenkins**_**\_slave** \(Dockerfile para desplegar Jenkins Slaves, id\_rsa y id\_rsa.pub del server\)
* Directorio **sonatype-work **\(Workspace de Nexus\)
* Archivo **repository.zip** \(Carpeta .m2 de Maven\) 

![](/assets/ci1.png)

Aplicaciones disponibles:

* Jenkins CI en [http://172.31.100.23:8080/](http://172.31.100.23:8080/)
* Sonatype Nexus en [http://172.31.100.23:8081/nexus/](http://172.31.100.23:8081/nexus/)

![](/assets/ci3.png)

### Server Desarrollo

Contendrá lo siguiente:

* Directorio **deploys** para guardar los archivos .war y .properties generados 
* Directorio **dockerFiles** para guardar los dockerfile de Glassfish 4 y grails
* Directorio **ebcHub** para contener archivos externos de las aplicaciones a desplegar
* Directorio **folder\_nginx** para guardar el contenido estático de los reportes de pruebas 
  * Cada proyecto debera tener una carpeta en este directorio, con los subdirectorios **master y QA**
* Directorio **logs**
* **ojdbc8.jar** jar de conexión a base de datos
* **Dockerfile** de Glassfish 5
* Scripts de despliegue **deployApp.sh, deployAppGlassfish4.sh, deployAppGrails.sh**

![](/assets/devl1.png)

Aplicaciones:

* Nginx 
* Aplicativos desplegados en desarrollo

![](/assets/devl3.png)

### Server QA

* Directorio **deploys** para guardar los archivos .war y .properties generados 
* Directorio **dockerFiles** para guardar los dockerfile de Glassfish 4 y grails
* Directorio **ebcHub** para contener archivos externos de las aplicaciones a desplegar
* Directorio **logs**
* **ojdbc8.jar** jar de conexión a base de datos
* **Dockerfile** de Glassfish 5
* Scripts de despliegue **deployApp.sh, deployAppGlassfish4.sh, deployAppGrails.sh**

![](/assets/qa1.png)

### Publicación de pruebas de submódulos

Los proyectos individualmente están preparados para ejecutar las pruebas. Si el branch correspondiente es **master o QA** entonces además publicará los resultados.

Publicación de pruebas: [http://172.31.100.25:8081/NOMBRE-DEL-PROYECTO/BRANCH](http://172.31.100.25:8081/NOMBRE-DEL-PROYECTO/BRANCH)/

## Proyectos Migrados

Los siguientes proyectos han sido migrados al nuevo esquema de CI

| **SUITE** | **Puerto de Aplicaciones** | **Puerto Glassfish** |
| :--- | :--- | :--- |
| Suite rutas docentes | 8080 | 4848 |
| Banner-moodle-middleware | 8082 | 4849 |
| Suite seguimiento estudiantil | 8083 | 4850 |
| Suite inscripción en línea | 8084 | 4851 |
| Suite comisiones | 8085 | 4852 |
| Recibos de nomina cfdi | 8086 | 4853 |
| Suite información docente | 8087 | 4854 |
| Suite mis cursos estudiante | 8088 | 4855 |
| Suite expediente digital | 8089 | 4856 |
| Suite genexa | 8090 | 4857 |
| Suite webex | 8091 | 4858 |
| Suite mis cursos docente | 8092 | 4859 |
| Suite selección de población | 8093 | 4860 |

## **Proyectos API:**

| Submódulo | Puerto de aplicaciones | Puerto Glassfish |
| :--- | :--- | :--- |
| Api ebc | 8094 | 4861 |
| Ws ebc | 8095 | 4862 |
| Api mobile | 8096 | 4863 |
| Api banner | 8097 | 4864 |
| Biblioteca bluebottlebiz | 8098 | 4865 |
| Biblioteca gale | 8099 | 4866 |
| Biblioteca springer | 8100 | 4867 |



