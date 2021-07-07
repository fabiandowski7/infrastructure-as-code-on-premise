# Terraform 🔧

Terraform es una herramienta para construir, combinar y poner en marcha de manera segura y eficiente la infraestructura. Desde servidores físicos a contenedores hasta productos SaaS (Software como un Servicio), Terraform es capaz de crear y componer todos los componentes necesarios para ejecutar cualquier servicio o aplicación.

Se define la infraestructura completa como código, incluso si se extiende a múltiples proveedores de servicios. Por ejemplo los servidores pueden estar de Openstack, el DNS en AWS, terraform construirá todos los recursos a través de estos proveedores en paralelo, con esto se proporciona un flujo de trabajo y se usa como herramientas para cambiar y actualizar la infraestructura con seguridad.

Las principales características de Terraform son:

- **Infraestructura como código:** La infraestructura se describe utilizando una sintaxis de alto nivel. Esto permite que un blueprint sea versionado y tratado como lo haría con cualquier otro código. Estos archivos que describen la infraestructura pueden ser compartidos y reutilizados.

- **Planes de Ejecución:** Terraform tiene un paso de “planificación” donde genera un plan de ejecución. El plan de ejecución muestra lo que hará Terraform cuando se ejecute. Esto permite evitar sorpresas.

- **Gráfico de recursos:** Terraform crea un gráfico de todos los recursos y paraleliza la creación y modificación de cualquier recurso. Con esto los operadores obtienen información sobre las dependencias en la infraestructura.

- **Automatización de cambios:** Los conjuntos de cambios complejos se pueden aplicar a su infraestructura con una mínima interacción humana. Con el plan de ejecución y el gráfico de recursos mencionados anteriormente, se sabe exactamente qué cambiará Terraform y en qué orden, evitando muchos posibles errores humanos.

#### ¿Qué es Infraestructura cómo código? ✔️

Infraestructura como código hace referencia a la práctica de utilizar scripts para configurar la infraestructura de una aplicación como máquinas virtuales, en lugar de configurar estas máquinas de forma manual.

La infraestructura como código permite a las máquinas virtuales gestionarse de manera programada, lo que elimina la necesidad de realizar configuraciones manuales (y actualizaciones) de componentes individuales. Esto es una construcción de infraestructura más consistente y de mayor calidad con mejores capacidades de administración, maximizando la eficiencia y evitando el error humano.

El resultado es una infraestructura muy elástica, escalable y replicable gracias a la capacidad de modificar, configurar y apagar cientos de máquinas en cuestión de minutos con solo presionar un botón.

<img width="588" alt="Screen Shot 2020-03-30 at 17 18 09" src="https://user-images.githubusercontent.com/45079819/77957806-7455ac80-72aa-11ea-89d6-399e31407f45.png">


Para mas información acerca de Terraform pueden dirigirse a su [Página-Oficial](https://www.terraform.io/) o pueden realizar este [Curso](https://www.youtube.com/watch?v=ec4qHgJQM7c&list=PLfW3im2fiA7XDjPgS9uzgv5Zeyhi_QE9Y) completamente **Gratis** para afianzar los conocimientos en la herramienta. 🤓

