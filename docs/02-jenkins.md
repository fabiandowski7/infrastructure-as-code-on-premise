# Jenkins 🚀

Jenkins es un herramienta open source que cubre algunas de las fases del ciclo de vida de integración continua de tu aplicación. Permite a los equipos de desarrollo compilar, probar y desplegar los desarrollos. Gestionando las entregas continuas desde el código hasta el despliegue en un solo flujo de trabajo, más conocido como “Job” en entorno Jenkins.

La base de Jenkins son las tareas Jobs, desde donde podrás indicar todo el proceso hasta llegar a un **build** final estable. Vamos con un ejemplo: 📎

Puedes programar un Job que se ejecute cada vez que subimos una nueva versión a Git, que este se **compile** y se ejecuten las **pruebas**.

Si el resultado no es el esperado o hay algún error, Jenkins notificará al desarrollador o a quien definas, por email u otros medios. Si la compilación es correcta, podremos indicar a Jenkins que integre el código en una rama ‘master’ del propio repositorio del control de versiones.

Una vez tenemos el código compilado, podrás indicar que se ejecuten las **pruebas**, con métricas de calidad y visualizar los resultados y para finalizar, podrás **desplegar** una versión estable del software al entorno de pruebas para ser **testeado**, en pre-producción o producción. Con el desarrollo disponible, solo queda esperar nuevos reportes de mejora.

<img width="1042" alt="Screen Shot 2020-03-26 at 16 25 07" src="https://user-images.githubusercontent.com/45079819/77688175-62100180-6f7e-11ea-850f-1578849684e7.png">

#### ¿Qué son la integración/distribución continuas (CI/CD)? ✔️

La CI/CD es un método para distribuir aplicaciones a los clientes con frecuencia mediante el uso de la automatización en las etapas del desarrollo de aplicaciones. Los principales conceptos que se atribuyen a la CI/CD son la integración continua, la distribución continua y la implementación continua. La CI/CD es una solución para los problemas que puede generar la integración del código nuevo a los equipos de desarrollo y de operaciones


En el desarrollo de una aplicación, la integración continua se organiza en las siguientes fases: 📄

- **Desarrollar** nuevas integraciones o mejoras en nuestra aplicación.
- **Compilación** del código fuente, obteniendo el ‘build’.
- **Pruebas**, realizando análisis y test unitarios, con métricas de calidad para la detección preventiva de errores
- **Despliegue** y aprovisionamiento de la aplicación en el entorno que definamos, como por ejemplo test o producción.
- **Tests** funcionales y de integración, automatizados o manuales.
- **Reportes** de nuestros usuarios o automatizados con por ejemplo Sentry, New relic, etc.


<img width="1412" alt="Screen Shot 2020-03-26 at 16 45 34" src="https://user-images.githubusercontent.com/45079819/77689928-478b5780-6f81-11ea-8ee7-c30fdb337e44.png">


Para mas información acerca de Jenkins pueden dirigirse a su [Página-Oficial](https://jenkins.io/) o pueden realizar este [Curso](https://www.youtube.com/watch?v=FX322RVNGj4) completo totalmente gratis. 
