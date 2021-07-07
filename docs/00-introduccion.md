# Introduccion

## ¿Qué es la Infraestructura como código (IaC)?

Infraestructura como código es un método de aprovisionamiento y gestión de infraestructura IT y servicios a través del uso de código fuente, sustituyendo el procedimiento estándar de operación. Básicamente consiste en tratar los servidores, bases de datos, redes y otros elementos de infraestructura como si fuera software. Este código facilita el despliegue de esta infraestructura de un modo rápido, seguro y consistente.

## Beneficios de IaC

### Rapidez
El diseño de una infraestructura con código permite agilizar de manera significativa el despliegue posterior de manera rápida y segura. IaC permite desplegar toda una infraestructura que podría llevar horas o días enteros ejecutando tan sólo un script en cuestión de unos pocos minutos.

Si bien es cierto que el desarrollo del código que permitirá el despliegue de la infraestructura puede ser igual de costoso que un despliegue inicial, aporta la ventaja de que es reutilizable por lo que se pueden importar snippets que automaticen partes y cuando la biblioteca de recursos estándar ya está poblada se reduce mucho el tiempo de desarrollo, esto sin contar que además en caso de tener que levantar varios entornos de la misma arquitectura es donde se demuestra realmente la rapidez de IaC ya que una vez desarrollado permite replicarlo en cuestión de minutos.

### Automatización
La automatización en la replicación de infraestructura es otro punto interesante de la IaC. Es posible tomar el diseño de una infraestructura con código para que sea replicada exactamente igual en otro entorno únicamente modificando los parámetros que se proporcionan durante la creación.

Además las herramientas de IaC normalmente ofrecen APIs que permiten automatizar la ejecución del IaC integrándola con herramientas de “Continuos Delivery” (Jenkins, Drone) para integrar dentro de los ciclos de pruebas la creación de un entorno sobre el que ejecutar las pruebas y destruirlo a la finalización.

Adicionalmente nos ofrece la posibilidad de automatizar la creación de entornos de Disaster Recovery, si los tiempos de RTO y RPO nos lo permiten podemos tener simplemente en una localización alternativa las copias de seguridad de los datos y recrear la infraestructura solo en caso de desastre, con lo que se reduce al mínimo el coste de un entorno DR.

### Minimización de riesgos
Otra de las ventajas que ofrece IaC es la minimización de riesgos. Cuando se despliega infraestructura manualmente es inevitable que en algún momento se cometa un error. IaC permite hacer las comprobaciones necesarias antes de desplegar para que exista una consistencia, minimizando al máximo los errores anteriormente comentados. Aunque un despliegue de un servidor, por poner un ejemplo, es barato, el tiempo del ingeniero que lo despliega no lo es tanto. De modo que si se comete un error de base, como la creación de una red con datos incorrectos, y posteriormente hay que crear una cantidad concreta de servidores sobre esta red, será necesario dar marcha atrás a todo el proceso.

### Actualizaciones controladas y rollbacks
Los sistemas de IaC permiten la actualización de los stacks proporcionando un fichero actualizado y pidiendo que en lugar de crear un  nuevo stack procede a actualizar uno existente, el sistema se encarga de comprar el fichero con los recursos actualmente desplegados y se encarga de hacer únicamente los cambios necesarios.

Si los ficheros descriptivos los tenemos en un sistema de control de versiones además podremos hacer rollback fácilmente a versiones anteriores y comparar los cambios entre una versión y otro.


Next: [Prerequisitos](01-prerequisitos.md)
