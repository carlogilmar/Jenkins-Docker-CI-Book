# Plugins

1. Java Melody
2. Blue Ocean
3. Slack Notification Plugin

## Java Melody

Buscar en la sección de plugins como: **Monitoring**

![](/assets/p2.png)

#### **Entrar a IP/monitoring para ver el monitor de Java Melody**![](/assets/p3.png)

### Blue Ocean

Buscar en el apartado de plugins como **Blue Ocean**![](/assets/p4.png)

Una vez instalado podremos ver el ícono del lado izquierdo:

![](/assets/p5.png)

Al entrar podremos ver la interfaz de Blue Ocean:![](/assets/p6.png)![](/assets/p8.png)

### Slack Notification Plugin

#### Instalación de Jenkins CI en el workspace de Slack

Instalación de plugin Jenkins CI en el slack e identificar el **baseURL** y el **Integration Token**

![](/assets/s1.png)

![](/assets/s2.png)

### Jenkins CI

Instalar el plugin **Slack Notification Plugin** en el Jenkins CI![](/assets/s3.png)En la configuración global aparecerá un apartado para configurar las notificaciones por Slack dónde deberán incluir el **BaseURL**, el **team subdomain** \(url del slack\) y el** integration token**.

### ![](/assets/s4.png)Enviando notificaciones desde el pipeline

Una vez instalado lo anterior se podrán enviar notificaciones a través del Jenkinsfile:

```
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
```

#### Webhooks

Para habilitar un webhook es necesario entrar a la opción de Settings / Webhooks / Add Webhook dentro del repositorio en Bit Bucket del proyecto.

Al añadir el nuevo webhook bastará con sólo agregar un nombre y una URL.

- Title: New Jenkins
- URL: http://34.225.189.39:8080/bitbucket-scmsource-hook/notify/
