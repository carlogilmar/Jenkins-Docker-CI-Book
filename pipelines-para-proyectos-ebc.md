# Pipelines para Proyectos EBC

Los proyectos de desarrollo EBC serán considerados en SUITES y SUBMÓDULOS.

### Pipeline para submódulos

Los proyectos considerados submódulos únicamente ejecutarán las pruebas y publicarán los resultados.![](/assets/ebc10.png)Pipeline para Suites

Una SUITE es considerada como un conjunto de submódulos de git. ![](/assets/ebc11.png)El pipeline deberá realizar lo siguiente para cada submódulo al momento de ejecutarse:

* Ejecución de pruebas
* Construcción de ejecutable .war
* Despliegue al ambiente requerido: DEVL o QA![](/assets/ebc12.png)



