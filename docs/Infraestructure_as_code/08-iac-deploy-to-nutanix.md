![124823286-02552400-df3f-11eb-8ad1-4e6b0efc9a01](https://user-images.githubusercontent.com/18565089/124981387-81ac2b80-e003-11eb-8342-0f134373164a.png)


# Deploy una o multiples VM(s) en Nutanix con Terraform 🚀


## Terraform

[HashiCorp Terraform](https://www.terraform.io/) permite que la infraestructura se exprese como código en un lenguaje simple y legible por humanos llamado HCL (HashiCorp Configuration Language). Terraform utiliza este lenguaje para proporcionar un plan de ejecución de cambios, que puede revisarse por seguridad y luego aplicarse para realizar cambios.

Casi cualquier tipo de infraestructura se puede representar como un **resource** en Terraform. Si bien los recursos son la construcción principal en el lenguaje Terraform, los comportamientos de los resources dependen de sus tipos de recursos asociados, y estos tipos los definen los _providers_.

[Providers](https://www.terraform.io/docs/providers/index.html) son responsables de comprender las interacciones de la API y de exponer los recursos al mundo exterior. Los proveedores extensibles permiten que Terraform administre una amplia gama de recursos, incluidos los servicios de hardware, iaas, paas y saas.

En el siguiente ejemplo, el `vsphere_virtual_machine` resource del [Nutanix provider](https://registry.terraform.io/providers/nutanix/nutanix/latest/docs) se aprovecha para clonar y configurar varias máquinas virtuales de Nutanix.


## Requerimientos

* [Terraform](https://www.terraform.io/downloads.html) 0.12+

## Configuracion

The set of files used to describe infrastructure in Terraform is simply known as a Terraform _configuration_.:
El conjunto de archivos utilizados para describir la infraestructura en Terraform se conoce simplemente como _configuration_:

    ├── main.tf
    ├── output.tf
    ├── terraform.tfvars
    └── variables.tf


1. El archivo `main.tf` contiene la definición de mi proveedor, así como la **logica**: mientras que los _data sources_ permiten que los datos se obtengan o se calculen para su uso en otros lugares de la configuración (p. Ej., Clúster de Nutanix, almacén de datos, grupo de puertos, etc.), los bloques de recursos describen las máquinas virtuales a crear.
3. El archivo `variables.tf` contiene la definición de variables dentro de su configuración de Terraform (pero no los valores de esas variables que están definidas en  `terraform.tfvars`).
4. Para todos los archivos que coinciden con `terraform.tfvars` o `*.auto.tfvars` presentes en el directorio actual, Terraform los carga automáticamente para completar las variables. **Este archivo debe actualizarse para que coincida con la configuración de su infraestructura**.
5. (opcional) El archivo `output.tf` proporciona información útil para solucionar problemas.


## Recursos

Se definen 3 bloques de recursos `vsphere_virtual_machine` :

 - `vpos_server` clona una plantilla de Nutanix de Linux (SUSE Server) en una nueva máquina virtual y personaliza el invitado.
 - `vpos_pos1` clona una plantilla de Nutanix de Linux en varias máquinas virtuales nuevas y personaliza a los invitados; `count.index` wse usó para hacer un bucle sobre los recursos, pero se pueden usar otros mecanismos como reemplazo (como `for_each` o `for` loops).
 - `vpos_pos2` clona una plantilla de Nutanix de Linux en varias máquinas virtuales nuevas y personaliza a los invitados; `count.index` wse usó para hacer un bucle sobre los recursos, pero se pueden usar otros mecanismos como reemplazo (como `for_each` o `for` loops).

## Ejecución

### Init

El primer comando que se ejecutará para una nueva configuración es  `terraform init`, que inicializa varias configuraciones y datos locales que serán utilizados por comandos posteriores. Este comando también descargará e instalará automáticamente cualquier proveedor definido en la configuración.

    \❯ terraform init

    Initializing the backend...

    Successfully configured the backend "consul"! Terraform will automatically
    use this backend unless the backend configuration changes.

    Initializing provider plugins...
    - Finding nutanix/nutanix versions matching "1.2.0"...
    - Installing nutanix/nutanix v1.2.0...
    - Installed nutanix/nutanix v1.2.0 (unauthenticated)

    Terraform has created a lock file .terraform.lock.hcl to record the provider
    selections it made above. Include this file in your version control repository
    so that Terraform can guarantee to make the same selections by default when
    you run "terraform init" in the future.

    Terraform has been successfully initialized!

    You may now begin working with Terraform. Try running "terraform plan" to see
    any changes that are required for your infrastructure. All Terraform commands
    should now work.

    If you ever set or change modules or backend configuration for Terraform,
    rerun this command to reinitialize your working directory. If you forget, other
    commands will detect it and remind you to do so if necessary.

### Plan

El comando `terraform plan` se utiliza para crear un plan de ejecución. Terraform realiza una actualización, a menos que se deshabilite explícitamente, y luego determina qué acciones son necesarias para lograr el estado deseado especificado en los archivos de configuración.

Este comando es una forma conveniente de verificar si el plan de ejecución para un conjunto de cambios coincide con sus expectativas sin realizar ningún cambio en los recursos reales o en el estado.

### Apply

El comando `terraform apply` se utiliza para **aplicar los cambios necesarios para alcanzar el estado deseado de la configuración .**.

    \❯ terraform apply
    data.vsphere_datacenter.target_dc: Refreshing state...
    data.vsphere_network.target_network: Refreshing state...
    data.vsphere_compute_cluster.target_cluster: Refreshing state...
    data.vsphere_virtual_machine.source_template: Refreshing state...
    data.vsphere_datastore.target_datastore: Refreshing state...
    vsphere_virtual_machine.vpos_pos1[2]: Refreshing state... [id=422fd79c-755b-a2d4-bb09-c9d6476217f5]
    vsphere_virtual_machine.vpos_server: Refreshing state... [id=422faaf3-2f12-b3ee-f0ed-8d602bfa4b11]
    vsphere_virtual_machine.vpos_pos1[1]: Refreshing state... [id=422ff5fb-7c96-1494-a865-6969a6fdd52f]
    vsphere_virtual_machine.vpos_pos2[0]: Refreshing state... [id=422f4d66-fc03-14ed-767b-62ade0142d19]
    
    An execution plan has been generated and is shown below.
    Resource actions are indicated with the following symbols:
      + create
      ~ update in-place
    
    Terraform will perform the following actions:
    
      # vsphere_virtual_machine.vpos_server will be updated in-place
      ~ resource "vsphere_virtual_machine" "vpos_server" {
          - annotation                              = "SUSE Linux Enterprise 12 - 2021-07-05" -> null
    
    [...]
    
    Plan: 1 to add, 3 to change, 0 to destroy.
    
    Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.
    
      Enter a value: yes

Una vez que se aprovisionan los recursos, el estado se almacenará de forma predeterminada en un archivo local llamado `terraform.tfstate`; también se puede almacenar de forma remota, lo que funciona mejor en un entorno de equipo. En nuestro caso se almacenará en Consul.

### Destroy

Si está utilizando Terraform para activar múltiples entornos, como entornos de laboratorio, desarrollo o prueba, destruir es una acción útil.

Los recursos se pueden destruir usando el comando `terraform destroy`, que es similar a  `terraform apply`, pero se comporta como si todos los recursos se hubieran eliminado de la configuración.

**Disfruta!**

![image](https://user-images.githubusercontent.com/18565089/124981529-ad2f1600-e003-11eb-81b6-20cb3c3585e4.png)
