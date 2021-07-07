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

Ahora necesitamos definir un backend remoto en nuestro código Terraform. Llamémoslo backend.tf:
