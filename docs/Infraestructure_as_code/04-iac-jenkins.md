# Terraform Pipelines in Jenkins

![1_62WJpBzEdlsjlc2TtjFf3g](https://user-images.githubusercontent.com/18565089/124832596-dfc90800-df4a-11eb-8e0f-6dbe321a2648.jpeg)

## Pipelines
¿Qué es un pipeline exactamente? La interpretación tradicional o literal es que es una forma de canalizar algo de un lugar a otro. En informática, doblamos eso un poco para hacer referencia a una forma en que movemos algo (tal vez un cambio de código) hacia otra cosa (digamos, nuestra infraestructura). En este contexto, casi siempre comprende un conjunto de pasos lineales o tareas para ir de A a B.
Por lo general, vemos canalizaciones a las que se hace referencia como parte de la integración continua, que en su forma más básica es simplemente la práctica de fusionar frecuentemente los cambios del desarrollador en una línea principal de código. Una canalización a menudo se desencadena por un cambio de código (como un enlace posterior a la confirmación en git) y puede ayudar a fusionar ese cambio al proporcionar etapas de prueba, aprobación e implementación.
Recuerde que al comienzo de esta serie defendimos el beneficio de definir la infraestructura como código. Ahora que estamos todos al día, configuremos una canalización en Jenkins para implementar cambios en ese código.


## Git
Necesitamos una forma de compartir nuestro código con Jenkins, por lo que es hora de enviar su directorio de trabajo a un repositorio de git alojado. Puede ser Github, BitBucket, Gitlab o cualquier otro servicio de git alojado, siempre que Jenkins pueda verlo a través de Internet. He usado Github:

## Estado remoto
También necesitamos hacer una transición rápida de regreso a la escuela Terraform para aprender sobre el estado remoto. Anteriormente, cuando ejecutamos Terraform, notará que algunos archivos de estado se escriben en nuestro directorio local. Terraform los usa para hacer su gráfico de cambios cada vez que se ejecuta. Entonces, si ejecutamos Terraform en un contenedor elegante en una tubería, ¿dónde escribe su archivo de estado?
La respuesta es almacenar el estado en un contenedor en el propio proyecto. Entonces cualquiera puede ejecutar Terraform en la tubería y el estado remoto se almacena y comparte de forma centralizada. Terraform también gestionará el bloqueo del estado para asegurarse de que los trabajos en conflicto no se ejecuten al mismo tiempo.
Entonces, creemos un depósito de Google Cloud Storage (cambie su-project-id en consecuencia):

Ahora necesitamos definir un backend remoto en nuestro código Terraform. Llamémoslo ```backend.tf```:

```
 terraform {
  backend "consul" {
    address="<http://IP>:8500"
    scheme="http"
    path="onpremise/Nutanix/Infraestructure/VMs_SUSE/output/Bloque_0001_POS/terraform-state"
    datacenter="dc1"
    lock="false"
  }
}
```
Ejecute terraform init Terraform le ofrecerá amablemente copiar su estado local en su nuevo backend remoto. Responda con sí y luego agregue backend.tf a su repositorio.

## Jenkinsfile
¡Lo hicimos! Es hora de crear una canalización. En Jenkins, los pipelines están escritos en groovy (una especie de lenguaje de scripting derivado de Java). Se pueden escribir mediante secuencias de comandos , utilizando la mayor parte de la funcionalidad del lenguaje maravilloso, o declarativas , que es mucho más simple. Copie el ```Jenkinsfile``` del repositorio anterior y agréguelo al suyo. Describiré las partes a continuación:
```    
pipeline {
  agent any
  environment {
    SVC_ACCOUNT_KEY = credentials('terraform-auth')
  }
  stages {
``` 

Al principio del archivo, declaramos que esto es un ```pipeline``` y no nos importa en qué ```agent``` se ejecute (porque el complemento de Kubernetes lo administrará por nosotros). Podemos definir variables de entorno para nuestros agentes en la ```environment``` sección, y aquí llamamos a la ```credentials``` función groovy para obtener el valor del ```terraform-auth``` secret que establecimos anteriormente. Entonces estamos listos para definir el ```stages``` de nuestro pipeline:
```    stage('Checkout') {
      steps {
        checkout scm
        sh 'mkdir -p creds'
        sh 'echo $SVC_ACCOUNT_KEY | base64 -d > ./creds/serviceaccount.json'
      }
    } 
``` 
En esta etapa, los agentes de Jenkins verifican nuestro repositorio de git en su espacio de trabajo. Luego crea el ```creds``` directorio y base64 decodifica el secreto que establecimos como variable de entorno. El espacio de trabajo ahora tiene un ```creds``` directorio y un ```serviceaccount.json``` archivo al igual que nuestro directorio local.
```
    stage('TF Apply') {
      steps {
        container('terraform') {
          sh terraform apply -input=false myplan'
        }
      }
    }
  }
}
``` 
Finalmente en esta etapa se aplica el plano de terraform que se creó anteriormente, nuevamente utilizando la ```terraform``` plantilla de contenedor.
Una vez que haya creado este archivo y lo haya enviado a su repositorio, estamos listos para agregar la canalización a Jenkins.

## Agregar la canalización
De vuelta en la interfaz de usuario de Jenkins, seleccione Nuevo elemento en la página de inicio. Especifique que este elemento es una canalización y asígnele un nombre, luego haga clic en Aceptar.

![image](https://user-images.githubusercontent.com/18565089/124942780-ee123500-dfd9-11eb-8876-f4e87109ad60.png)
