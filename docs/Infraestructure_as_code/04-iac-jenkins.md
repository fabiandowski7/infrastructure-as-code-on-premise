# Terraform Pipelines en Jenkins

![1_62WJpBzEdlsjlc2TtjFf3g](https://user-images.githubusercontent.com/18565089/124832596-dfc90800-df4a-11eb-8e0f-6dbe321a2648.jpeg)

## Pipelines
¿Qué es un pipeline exactamente? La interpretación tradicional o literal es que es una forma de canalizar algo de un lugar a otro. En informática, doblamos eso un poco para hacer referencia a una forma en que movemos algo (tal vez un cambio de código) hacia otra cosa (digamos, nuestra infraestructura). En este contexto, casi siempre comprende un conjunto de pasos lineales o tareas para ir de A a B.
Por lo general, vemos canalizaciones a las que se hace referencia como parte de la integración continua, que en su forma más básica es simplemente la práctica de fusionar frecuentemente los cambios del desarrollador en una línea principal de código. Una canalización a menudo se desencadena por un cambio de código (como un enlace posterior a la confirmación en git) y puede ayudar a fusionar ese cambio al proporcionar etapas de prueba, aprobación e implementación.
Recuerde que al comienzo de esta serie defendimos el beneficio de definir la infraestructura como código. Ahora que estamos todos al día, configuremos una canalización en Jenkins para implementar cambios en ese código.


## Git
Necesitamos una forma de compartir nuestro código con Jenkins, por lo que es hora de enviar su directorio de trabajo a un repositorio de git alojado. Puede ser Github, BitBucket, Gitlab o cualquier otro servicio de git alojado, siempre que Jenkins pueda verlo a través de Internet o en forma privada. He usado Bitbucket Server:

![image](https://user-images.githubusercontent.com/18565089/124945770-7560a800-dfdc-11eb-9281-86afcf1642ef.png)


## Estado remoto
Cuando ejecutamos Terraform, notará que algunos archivos de estado se escriben en nuestro directorio local. Terraform los usa para hacer su gráfico de cambios cada vez que se ejecuta. Entonces, si ejecutamos Terraform en un contenedor en un pipeline, ¿dónde escribe su archivo de estado?
La respuesta es: Se almacena el estado en un contenedor propio del proyecto. Entonces cualquiera puede ejecutar Terraform en el pipeline y el estado remoto se almacena y comparte de forma centralizada. Terraform también gestionará el bloqueo del estado para asegurarse de que los trabajos en conflicto no se ejecuten al mismo tiempo.
Entonces, creemos un backend en un contenedro con Consul:

![image](https://user-images.githubusercontent.com/18565089/124945420-1d29a600-dfdc-11eb-995e-88917b2e1e90.png)


Ahora necesitamos definir un backend remoto en nuestro código Terraform. Llamémoslo ```backend.tf```:

```
 terraform {
  backend "consul" {
    address="<http://IP>:8500"
    scheme="http"
    path="onpremise/Nutanix/Infraestructure/VMs_SUSE/output/Bloque_0003_SERVER/terraform-state"
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
```  
    stage('Checkout') {
      steps {
        checkout scm
        sh 'mkdir -p creds'
        sh 'echo $SVC_ACCOUNT_KEY | base64 -d > ./creds/serviceaccount.json'
      }
    }
``` 
En esta etapa, los agentes de Jenkins verifican nuestro repositorio de git en su espacio de trabajo. Luego crea el ```creds``` directorio y base64 decodifica el secreto que establecimos como variable de entorno. El espacio de trabajo ahora tiene un ```creds``` directorio y un ```serviceaccount.json``` archivo al igual que nuestro directorio local.
```
    stage('TF Plan') {
       steps {
         container('terraform') {
           sh 'terraform init'
           sh 'terraform plan -out myplan'
         }
       }
     }
``` 

En esta siguiente etapa realizamos el Apply de nuestro Terraform Plan, exactamente de la misma manera que lo hemos hecho anteriormente en nuestro entorno local. Estamos especificando la plantilla del terraform plan que agregamos anteriormente, por lo que en esta etapa preguntará sí ejecutar el plan de la infraestrcuctura y escribirá en el mismo espacio de trabajo.

```
   stage name: 'TF Apply', concurrency: 1
        def deploy_validation = input(
            id: 'Deploy',
            message: 'Let\'s continue the deploy plan',
            type: "boolean")

        sh "terraform apply plan"
```

Finalmente en esta etapa se aplica el plan de terraform que se creó anteriormente, nuevamente utilizando la plantilla ```terraform```.
Una vez que haya creado este archivo y lo haya enviado a su repositorio, estamos listos para agregar la canalización a Jenkins.

## Creando el Pipeline
De vuelta en la interfaz de usuario de Jenkins, seleccione **Nueva tarea** en la página de inicio. Especifique que este elemento es un **pipeline** y asígnele un nombre, luego haga clic en **OK.**

![image](https://user-images.githubusercontent.com/18565089/124942780-ee123500-dfd9-11eb-8876-f4e87109ad60.png)


En la página siguiente se le presentan muchas opciones de configuración. Todo lo que tenemos que hacer por ahora es decirle a Jenkins que puede encontrar el Pipeline en un repositorio. Desplácese hacia abajo hasta la sección Pipeline y seleccione **Pipeline script from SCM** , luego elija **Git** o **Bitbucket Server** e ingrese la URL de su repositorio. Luego haga clic en **Guardar.**

![image](https://user-images.githubusercontent.com/18565089/124943501-89a3a580-dfda-11eb-8138-79ad804b3b15.png)


Jenkins agrega el Pipeline y lo devuelve a su página de inicio. Ahora para las cosas elegantes. Si cree que la interfaz de usuario de Jenkins parece un poco web 1.0, prepárese para quedar impresionado. Seleccione **Open Blue Ocean** en el menú de la izquierda.

![1_5MsK2MMyntBUUOvS3hNwXQ](https://user-images.githubusercontent.com/18565089/124943624-a8a23780-dfda-11eb-8530-ca8cc1bae534.png)


¡Es como un mundo completamente nuevo! Y se pone mejor. Haga clic en **Iniciar.** Se le notificará que ha comenzado un trabajo y puede hacer clic en él para ver el progreso. Es posible que la primera vez que haga esto le lleve unos minutos.
Obtiene una buena representación visual del pipeline de esta interfaz de usuario. Cuando lleguemos a la etapa de aprobación, Jenkins esperará sus respuesta. En este punto, puede volver a hacer clic en la etapa **TF Plan** y asegurarse de que está satisfecho con el plan que se va a aplicar. Dado que creamos nuestro backend de estado remoto, Terraform debe saber que no hay cambios que hacer, a menos que haya alterado su código de Terraform.

![1_vVAN4tq9K5ZoDvehWloHLw](https://user-images.githubusercontent.com/18565089/124944238-25cdac80-dfdb-11eb-8fc4-047234b07daf.png)


![1_6I_R-weIkc6MxB1WyEyHIg](https://user-images.githubusercontent.com/18565089/124944367-40078a80-dfdb-11eb-9059-0ac50eb9ef42.png)


Si está satisfecho con el plan, siga adelante y dé su aprobación. Todo debe volverse verde y darte una sensación cálida y difusa por dentro.


![1_ft2w6SyhepELB6EGILENWw](https://user-images.githubusercontent.com/18565089/124944495-5a416880-dfdb-11eb-80d0-6f1bfb9be68f.png)


## ¡Es un excelente enfoque!

Bueno, puede ser la primera vez creando Pipelines. Sin embargo, una vez que adquiere el hábito de hacer las cosas de esta manera, realmente no lleva tiempo configurar estas herramientas y procesos. Vale la pena el esfuerzo porque gana mucho con este enfoque de GitOps / DevOps:
- Todo se hace a través de git, lo que hace que la colaboración en equipo sea más efectiva.
- Los Pipelines pueden integrar dependencias, pruebas, otras compilaciones, lo que sea necesario para que su implementación funcione como debería.
- Jenkins mantiene un historial de cambios, etapas de compilación, ejecuciones de canalizaciones e implementaciones.

No hace falta decir que puede construir Pipelines para cualquier cosa, no solo para Terraform. De hecho, es mucho más común usarlos para la implementación de aplicaciones que para la gestión del ciclo de vida de la infraestructura. Pero cada vez que tenga que obtener algo de código de A a B, una canalización es probablemente la forma de hacerlo.

## ¿Que sigue?

En un entorno del mundo real, deberias activar los pipelines desde un **git commit.** Esto se puede hacer con un enlace posterior a la confirmación que llama a la API de Jenkins.

Mediante la configuración de un Pipeline se aprovisionará Infraestructura en los hipervisores **Nutanix** y **VMware**

* [Infraestructura cómo código en VMware](07-iac-deploy-to-vmware.md)
* [Infraestrcutura como código en Nutanix](08-iac-deploy-to-nutanix.md) 
