# Ansible :spades:

Ansible es un software que automatiza el aprovisionamiento de software , la gestión de configuraciones y el despliegue de aplicaciones. Está categorizado como una herramienta de orquestación , muy útil para los administradores de sistema y DevOps.

En otras palabras, Ansible permite a los DevOps gestionar sus servidores, configuraciones y aplicaciones de forma sencilla, robusta y paralela

Ansible gestiona sus diferentes nodos a través de SSH y únicamente requiere Python en el servidor remoto en el que se vaya a ejecutar para poder utilizarlo. Usa YAML para describir acciones a realizar y las configuraciones que se deben propagar a los diferentes nodos.

#### Compatibilidad de Ansible ✔️

Ansible se distribuye en Fedora, Red Hat enterprise Linux, CentOS y Scientific Linux mediante los paquetes EPEL, además está disponible para diferentes distribuciones Linux aparte de las anteriores mencionadas puedes verlo en este enlace y descargar la que necesites.

También está disponible para MAC, pero no para Windows, aunque podemos usarlo en máquinas virtuales.

#### ¿Qué es un playbook de Ansible? ✔️

Los playbooks nos proporcionan una manera totalmente diferente de utilizar Ansible . Los comandos ad-hoc hacen de Ansible una herramienta muy potente, pero los playbooks la convierten en “la herramienta”. Mientras que podemos ejecutar comandos ad-hoc con Ansible, los playbooks podemos tenerlos en un control de versiones como Git . Además, pueden ejecutar tareas tal y como nosotros queramos haciendo uso de los inventarios, tags, roles, etc. 

#### Arquitectura de Ansible ✔️

En Ansible existen dos tipos de servidores:

- **Controlador:** La máquina desde la que comienza la orquestación
- **Nodo:** Es manejado por el controlador a través de SSH.

<img width="586" alt="Screen Shot 2020-03-30 at 15 50 53" src="https://user-images.githubusercontent.com/45079819/77950008-49655b80-729e-11ea-9074-74e274b9674e.png">


Para mas información acerca de Ansible pueden dirigirse a su [Página-Oficial](https://www.ansible.com/) o pueden realizar este [Curso](https://www.youtube.com/watch?v=wf_Ax0PpZxI) completamente **Gratis** para afianzar los conocimientos en la herramienta. 🤓

