# Docker 🐳

Docker es una plataforma de software que le permite crear, probar e implementar aplicaciones rápidamente. Docker empaqueta software en unidades estandarizadas llamadas contenedores que incluyen todo lo necesario para que el software se ejecute, incluidas bibliotecas, herramientas de sistema, código y tiempo de ejecución. Con Docker, puede implementar y ajustar la escala de aplicaciones rápidamente en cualquier entorno con la certeza de saber que su código se ejecutará.

A primera vista se piensa en Docker como una especie de máquina virtual “liviana”, pero la verdad no lo es. En Docker no existe un hypervisor que virtualice hardware sobre el cual corra un sistema operativo completo. En Docker lo que se hace es usar las funcionalidades del Kernel para encapsular un sistema, de esta forma el proyecto que corre dentro de el no tendrá conocimiento que está en un contenedor. Los contenedores se encuentran aislados entre sí y se comportaran como máquinas independientes.

Iniciar un contenedor no tiene un gran impacto a diferencia de iniciar una máquina virtual ya que no tiene que iniciar un sistema operativo completo (desde cero). Gracias al uso de contenedores la demanda de recursos baja limitándose sólo al consumo de la aplicación que contenga. Un contenedor inicia en milisegundos.



<img width="914" alt="Screen Shot 2020-03-26 at 18 13 49" src="https://user-images.githubusercontent.com/45079819/77697409-93dc9480-6f8d-11ea-8eb7-cd49b2e5d1f9.png">


#### ¿Qué son los Contenedores? ✔️

Un contenedor es una unidad de software estándar que empaqueta el código y todas sus dependencias para que la aplicación se ejecute de manera rápida y confiable. Una imagen de contenedor de Docker es un paquete de software ligero, independiente y ejecutable que incluye todo lo necesario para ejecutar una aplicación: código, tiempo de ejecución, herramientas del sistema, bibliotecas y configuraciones del sistema.

Contenedores Docker que se ejecutan en Docker Engine: 🚢

- **Estándar:** Docker creó el estándar de la industria para contenedores, por lo que podrían ser portátiles en cualquier lugar
- **Ligero:** los contenedores comparten el núcleo del sistema operativo de la máquina y, por lo tanto, no requieren un sistema operativo por aplicación, lo que aumenta la eficiencia del servidor y reduce los costos de servidor y licencias
- **Seguro:** las aplicaciones son más seguras en contenedores y Docker proporciona las capacidades de aislamiento predeterminadas más sólidas de la industria

![docker](https://user-images.githubusercontent.com/45079819/77698192-f84c2380-6f8e-11ea-9466-887b11dad91e.jpg)


Para mas información acerca de Docker pueden dirigirse a su [Página-Oficial](https://www.docker.com/) o pueden realizar este [Curso](https://codigofacilito.com/cursos/docker) completamente **Gratis** para afianzar los conocimientos en la herramienta. 🤓

