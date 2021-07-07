# Jenkins 🚀

Jenkins es un herramienta open source que cubre algunas de las fases del ciclo de vida de integración continua de tu aplicación. Permite a los equipos de desarrollo compilar, probar y desplegar los desarrollos. Gestionando las entregas continuas desde el código hasta el despliegue en un solo flujo de trabajo, más conocido como “Job” en entorno Jenkins.

La base de Jenkins son las tareas Jobs, desde donde podrás indicar todo el proceso hasta llegar a un **build** final estable. Vamos con un ejemplo: 📎

Puedes programar un Job que se ejecute cada vez que subimos una nueva versión a Git, que este se **compile** y se ejecuten las **pruebas**.

Si el resultado no es el esperado o hay algún error, Jenkins notificará al desarrollador o a quien definas, por email u otros medios. Si la compilación es correcta, podremos indicar a Jenkins que integre el código en una rama ‘master’ del propio repositorio del control de versiones.

Una vez tenemos el código compilado, podrás indicar que se ejecuten las **pruebas**, con métricas de calidad y visualizar los resultados y para finalizar, podrás **desplegar** una versión estable del software al entorno de pruebas para ser **testeado**, en pre-producción o producción. Con el desarrollo disponible, solo queda esperar nuevos reportes de mejora.
