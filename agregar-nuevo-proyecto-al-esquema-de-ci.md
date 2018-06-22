## Agregar nuevo proyecto al esquema de CI

* Agregar proyecto individual
* Agregar proyecto suite
* Despliegue de un proyecto individual

### Agregar proyecto individual

Para este caso será necesario agregar un archivo llamado Jenkinsfile y versionarlo. Para este tipo de proyectos solo se agregan los pasos necesarios para ejecutar las pruebas, y en caso de los branch master y QA publicar los resultados. 

Además el proyecto deberá contener los archivos de properties de los ambientes TEST, DEVEL y QA.![](/assets/ebc20.png)

Ejemplo:

```groovy
pipeline{

  agent any

  tools {
    gradle "Gradle 3.5"
  }

  stages {
		stage('Preparing File Configuration') {
			steps{
				echo "Create folder project_config and move file conf"
					sh 'mkdir -p $HOME/.project_config/'
					sh 'cp api-seguridad-DEVL.properties $HOME/.project_config/api-seguridad.properties'
			}
		}

    stage('Test App'){
      steps {
        echo 'Testing app'
        sh 'gradle clean test'
      }
    }

    stage('Publishing Tests Reports'){
      when {
        expression {
          env.BRANCH_NAME in ["master", "QA"]
        }
      }
      steps{
        echo "Running Test in **${env.BRANCH_NAME}**"
        sh "scp -r -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no core/build/reports/tests/test/* DAWS03LX@172.31.100.25:/u01/folder_nginx/api-seguridad/${env.BRANCH_NAME}"
      }
    }

  }

  post {
		success{
			slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"
		}
		failure{
			slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was failed"
		}
    always {
			cleanWs()
    }
  }

}
```

### Agregar proyecto suite

El proyecto suite es un repositorio de git con las referencias a los submódulos. Será necesario agregar un archivo llamado Jenkinsfile y versionarlo. 

![](/assets/ebc21.png)

Este archivo Jenkinsfile deberá:

* Ejecutar las pruebas de cada submódulo en paralelo 
* Construir el ejecutable .war en cada submódulo
* Dependiendo del branch deberá desplegar en algún ambiente \(master o QA\)
  * Tener en cuenta que la ejecución del script de despliegue debe contener el nombre del proyecto y una asignación de puertos no ocupados 

Ejemplo: 

```
pipeline{

  agent any

  tools {
    gradle "Gradle 3.5"
  }

  stages {

		stage('Checkout Submodules') {
			steps{
        echo "Checkout and init submodules"
        checkout scm: [
                $class: 'GitSCM',
                branches: scm.branches,
                doGenerateSubmoduleConfigurations: false,
                extensions: [[$class: 'SubmoduleOption',
                              disableSubmodules: false,
                              parentCredentials: false,
                              recursiveSubmodules: true,
                              reference: '',
                              trackingSubmodules: false]],
                submoduleCfg: [],
                userRemoteConfigs: scm.userRemoteConfigs
        ]
			}
		}

    stage('Test submodules: Copy test properties and test'){
      parallel{
        stage('Test api-ebc-adea app'){
          steps{
            dir('api-ebc-adea'){
              sh 'cp api-ebc-adea-TEST.properties $HOME/.project_config/api-ebc-adea.properties'
              sh 'gradle clean test'
            }
          }
        }
        stage('Test api-seguridad app'){
          steps{
            dir('api-seguridad'){
              sh 'cp api-seguridad-TEST.properties $HOME/.project_config/api-seguridad.properties'
              sh 'gradle clean test'
            }
          }
        }
        stage('Test api-ebc app'){
          steps{
            dir('expedientes-digitales'){
              sh 'cp expedientes-digitales-TEST.properties $HOME/.project_config/expedientes-digitales.properties'
              sh 'gradle clean test'
            }
          }
        }
      }
    }

    stage('Building applications'){
      parallel{
        stage('Building api-ebc-adea app'){
          steps{
            dir('api-ebc-adea'){
              sh 'gradle build -x test'
            }
          }
        }
        stage('Building api-seguridad app'){
          steps{
            dir('api-seguridad'){
              sh 'gradle build -x test'
            }
          }
        }
        stage('Building expedientes-digitales app'){
          steps{
            dir('expedientes-digitales'){
              nodejs(nodeJSInstallationName: 'Node 10.3.0') {
                sh 'npm install'
                sh 'node_modules/bower/bin/bower update'
                sh 'node_modules/coffee-script/bin/coffee --output web/src/main/webapp/libs/compiled_coffeescript/  --compile web/src/main/webapp/libs/coffeescript/'
              }
              sh 'gradle build -x test'
            }
          }
        }
      }
    }

    /*
     *  DEVL Environment
     */

		stage('DEVL ENVIRONMENT: Transfering Wars'){
      when {
        expression {
          env.BRANCH_NAME in ["master"]
        }
      }
      parallel{

        stage('Transfering api-ebc-adea war'){
          steps{
            dir('api-ebc-adea'){
							sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-ebc-adea.war DAWS03LX@172.31.100.25:~/deploys/suite-expediente-digital/"
            }
          }
        }
        stage('Transfering api-seguridad war'){
          steps{
            dir('api-seguridad'){
							sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-seguridad.war DAWS03LX@172.31.100.25:~/deploys/suite-expediente-digital/"
            }
          }
        }
        stage('Transfering expedientes-digitales war'){
          steps{
            dir('expedientes-digitales'){
							sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no expedientes-digitales.war DAWS03LX@172.31.100.25:~/deploys/suite-expediente-digital/"
            }
          }
       }
			}
		}

    stage('DEVL ENVIRONMENT: Preparing DEVL Configuration'){
      when {
        expression {
          env.BRANCH_NAME in ["master"]
        }
      }
      steps{
        dir('api-ebc-adea'){
					sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-ebc-adea-DEVL.properties DAWS03LX@172.31.100.25:~/deploys/suite-expediente-digital/api-ebc-adea.properties"
        }
        dir('api-seguridad'){
					sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-seguridad-DEVL.properties DAWS03LX@172.31.100.25:~/deploys/suite-expediente-digital/api-seguridad.properties"
        }
        dir('expedientes-digitales'){
					sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no expedientes-digitales-DEVL.properties DAWS03LX@172.31.100.25:~/deploys/suite-expediente-digital/expedientes-digitales.properties"
        }
      }
    }

    stage('DEVL ENVIRONMENT:Deploying app in Glassfish Docker'){
      when {
        expression {
          env.BRANCH_NAME in ["master"]
        }
      }
      steps{
        sh "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no DAWS03LX@172.31.100.25 'sh deployAppGlassfish4.sh deploys/suite-expediente-digital suite-expediente-digital no_content:/root/no_content /u01/logs/suite-expediente-digital:/root/.project_config/logs/logs 8089 4856'"
      }
    }

    /*
     *  QA Environment
     */

		stage('QA ENVIRONMENT: Transfering Wars'){
      when {
        expression {
          env.BRANCH_NAME in ["QA"]
        }
      }
      parallel{

        stage('Transfering api-ebc-adea war'){
          steps{
            dir('api-ebc-adea'){
							sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-ebc-adea.war TAWS02LX@172.31.100.24:~/deploys/suite-expediente-digital/"
            }
          }
        }
        stage('Transfering api-seguridad war'){
          steps{
            dir('api-seguridad'){
							sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-seguridad.war TAWS02LX@172.31.100.24:~/deploys/suite-expediente-digital/"
            }
          }
        }
        stage('Transfering expedientes-digitales war'){
          steps{
            dir('expedientes-digitales'){
							sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no expedientes-digitales.war TAWS02LX@172.31.100.24:~/deploys/suite-expediente-digital/"
            }
          }
       }
			}
		}

    stage('QA ENVIRONMENT: Preparing QA Configuration'){
      when {
        expression {
          env.BRANCH_NAME in ["QA"]
        }
      }
      steps{
        dir('api-ebc-adea'){
					sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-ebc-adea-QA.properties TAWS02LX@172.31.100.24:~/deploys/suite-expediente-digital/api-ebc-adea.properties"
        }
        dir('api-seguridad'){
					sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-seguridad-QA.properties TAWS02LX@172.31.100.24:~/deploys/suite-expediente-digital/api-seguridad.properties"
        }
        dir('expedientes-digitales'){
					sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no expedientes-digitales-QA.properties TAWS02LX@172.31.100.24:~/deploys/suite-expediente-digital/expedientes-digitales.properties"
        }
      }
    }

    stage('QA ENVIRONMENT:Deploying app in Glassfish Docker'){
      when {
        expression {
          env.BRANCH_NAME in ["QA"]
        }
      }
      steps{
        sh "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no TAWS02LX@172.31.100.24 'sh deployAppGlassfish4.sh deploys/suite-expediente-digital suite-expediente-digital no_content:/root/no_content /u01/logs/suite-expediente-digital:/root/.project_config/logs/logs 8089 4856'"
      }
    }

  }

  post{
    success{
      emailext body: '''${JELLY_SCRIPT,template="html_gmail"} AGREGAR AQUI EL BODY DEL CORREO!!''',
        mimeType: 'text/html',
        subject: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - SUCCESS!",
        to: "jorge@makingdevs.com,carlo@makingdevs.com,r.martinez026@ebc.edu.mx ",
        from: "ci@ebc.edu.mx",
        replyTo: "ci@ebc.edu.mx",
        recipientProviders: [[$class: 'CulpritsRecipientProvider']]
      slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"
    }
    failure{
      emailext body: '''${JELLY_SCRIPT,template="html_gmail"} AGREGAR AQUI TEMPLATE''',
        mimeType: 'text/html',
        subject: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - FAILURE!",
        to: "jorge@makingdevs.com,carlo@makingdevs.com,r.martinez026@ebc.edu.mx ",
        from: "ci@ebc.edu.mx",
        replyTo: "ci@ebc.edu.mx",
        recipientProviders: [[$class: 'CulpritsRecipientProvider']]
      slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was failed"
    }
    always {
      cleanWs()
    }
  }

}
```

### Despliegue de un proyecto individual

En ocasiones será necesario desplegar un solo proyecto, para ello agregaremos el Jenkinsfile bajo el control de versiones y seguiremos los mismos pasos para desplegar un proyecto SUITE.

Ejemplo

```
pipeline{

  agent any

  tools {
    gradle "Gradle 3.5"
  }

  stages {

    stage('Preparing File Configuration') {
      steps{
        echo "Create folder project_config and move file conf"
          sh 'mkdir -p $HOME/.project_config/'
          sh 'cp apiBanner-TEST.properties $HOME/.project_config/apiBanner.properties'
      }
    }

    stage('Test App'){
      steps {
        echo 'Testing app'
        sh 'gradle clean test'
      }
    }

    stage('Publishing Tests Reports'){
      when {
        expression {
          env.BRANCH_NAME in ["master", "QA"]
        }
      }
      steps{
        echo "Running Test in **${env.BRANCH_NAME}**"
        sh "scp -r -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no core/build/reports/tests/test/* DAWS03LX@172.31.100.25:/u01/folder_nginx/api-banner/${env.BRANCH_NAME}"
      }
    }

	/*
     *  DEVL Environment
     */

    stage('DEVL ENVIRONMENT: Deploy DEVL'){
      when {
        expression {
          env.BRANCH_NAME in ["master"]
        }
      }
      steps{
        sh 'gradle build -x test'
        sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-banner.war DAWS03LX@172.31.100.25:~/deploys/api-banner/"
        sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no apiBanner-DEVL.properties DAWS03LX@172.31.100.25:~/deploys/api-banner/apiBanner.properties"
        sh "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no DAWS03LX@172.31.100.25 'sh deployAppGlassfish4.sh deploys/api-banner api-banner no_content:/root/no_content /u01/logs/api-banner:/root/.project_config/logs/logs 8097 4864'"
      }
    }

    /*
     *  QA Environment
     */

    stage('QA ENVIRONMENT: Deploy QA'){
      when {
        expression {
          env.BRANCH_NAME in ["QA"]
        }
      }
      steps{
        sh 'gradle build -x test'
        sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no api-banner.war TAWS02LX@172.31.100.24:~/deploys/api-banner/"
        sh "scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no apiBanner-QA.properties TAWS02LX@172.31.100.24:~/deploys/api-banner/apiBanner.properties"
        sh "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no TAWS02LX@172.31.100.24 'sh deployAppGlassfish4.sh deploys/api-banner api-banner no_content:/root/no_content /u01/logs/api-banner:/root/.project_config/logs/logs 8097 4864'"
      }
    }

  }

  post {
    success{
      slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"
    }
    failure{
      slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was failed"
    }
    always {
      cleanWs()
    }
  }

}

```



