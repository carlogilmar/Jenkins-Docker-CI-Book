# Ejemplo de CI

### Proyecto: SUITE EXPEDIENTE DIGITAL \(contiene 3 submódulos\)

## Ejecución de pipelines para submódulos![](/assets/ebc15.png)

#### Cada submódulo tendrá su propio pipeline, para ello es necesario tener un archivo** Jenkisfile en raiz del proyecto y bajo el control de versiones**.

#### Cada proyecto ejecutara sus pruebas, si el branch en cuestión es **master o DEVL** entonces también publicará los resultados de las pruebas.

## ![](/assets/ebc16.png)

## Ejecución de Pipelines para Suite

Al construir el proyecto SUITE será necesario contemplar los tres proyectos, y seguir lo siguiente:

1. Ejecución de Pruebas
2. Construcción de ejecutable .war
3. Despliegue en ambiente con Docker

Estos pasos se ejecutarán por cada submódulo, de forma que el despliegue final en el ambiente correspondiente contendrá las tres aplicaciones.![](/assets/ebc17.png)![](/assets/ebc18.png)

![](/assets/ebc19.png)

