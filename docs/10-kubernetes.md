# Kubernetes 🌀

Kubernetes es una plataforma portable y extensible de código abierto para administrar cargas de trabajo y servicios. Kubernetes facilita la automatización y la configuración declarativa. Tiene un ecosistema grande y en rápido crecimiento. El soporte, las herramientas y los servicios para Kubernetes están ampliamente disponibles.

Kubernetes actua como un **orquestador** de contenedores. Un orquestador es el encargado de gestionar el ciclo de vida de los contenedores de una aplicación. Kubernetes, como orquestrador ofrece las siguientes caracteristicas:

- Manejo del clúster (permitir añadir o quitar nodos al clúster)
- Gestión del ciclo de vida de los contenedores (p. ej. reiniciar contenedores que fallen)
- Service Discovery (que un contenedor pueda encontrar las rutas IP/DNS de otro contenedor)
- Servicios de red y load-balancing (repartir la carga entre las distintas máquinas del clúster)
- Servicios de monitorización
- Servicios de chequeo de estado de salud (del clúster y de cada uno de los contenedores)

#### ¿Cómo funciona Kubernetes? ✔️

Kubernetes es un sistema de orquestación de contenedores, lo que significa que el software no se encarga de crearlos, sino de administrarlos. Para ello, Kubernetes aplica la automatización de procesos, lo que vuelve más fácil para los desarrolladores comprobar, mantener o publicar aplicaciones. La arquitectura de Kubernetes consta de una clara jerarquía, compuesta por los siguientes elementos:

- **Contenedor:** incluye las aplicaciones y entornos de software.
- **Pod:** este elemento de la arquitectura de Kubernetes se encarga de agrupar aquellos contenedores que necesitan trabajar juntos para el funcionamiento de una aplicación.
- **Nodo:** uno o varios pods se ejecutan en un nodo, que puede ser tanto una máquina virtual como física.
- **Clúster:** en Kubernetes, los nodos se agrupan en clústeres

![Figura1](https://user-images.githubusercontent.com/45079819/77954436-fc38b800-72a4-11ea-8490-c533fd2b1543.png)


Para mas información acerca de Kubernetes pueden dirigirse a su [Página-Oficial](https://kubernetes.io/) o pueden realizar este [Curso](https://www.youtube.com/watch?v=5ovqsvqwtZM&feature=youtu.be) completamente **Gratis** para afianzar los conocimientos en la herramienta. 🤓

