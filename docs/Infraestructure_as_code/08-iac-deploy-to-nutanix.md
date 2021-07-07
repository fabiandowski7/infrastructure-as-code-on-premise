# Administre máquinas virtuales Nutanix AHV existentes usando Terraform

Implementar y administrar recursos a través de Terraform tiene muchos beneficios. Es fácil, puede controlar la versión de sus diferentes configuraciones y puede asegurarse de que el estado de las máquinas virtuales sea coherente con su declaración. En la mayoría de los casos de uso, definirá sus recursos a través del código Terraform e implementará esas cargas de trabajo usando Terraform. En algunos casos, es posible que desee administrar cargas de trabajo existentes (¿ya aprovisionadas manualmente?) Que se ejecutan sobre Nutanix con Terraform. Dado que Terraform solo administrará los recursos que forman parte de su archivo de estado, no administrará las cargas de trabajo existentes o ya implementadas. Afortunadamente, existe una forma de incluir cargas de trabajo existentes en su archivo de estado.

Le mostraremos los pasos para importar una máquina virtual existente en su archivo de estado y administrarla a través de Terraform. El código se puede encontrar en este repositorio::
https://github.com/yannickstruyf3/terraform-nutanix-import-example

## Identifique sus cargas de trabajo existentes
Primero debe identificar qué carga de trabajo / vm desea administrar a través de Terraform. Para este ejemplo, usaré Prism Element (PE) para encontrar mi máquina virtual. Busque la máquina virtual y anote la ID. Esto se utilizará más tarde.
![alt_text](https://github.com/yannickstruyf3/terraform-nutanix-import-example/raw/master/images/1_identify_vm.png )
También analice el diseño actual de la máquina virtual:
- ¿En qué Clúster de Nutanix se está ejecutando?
- ¿Cuántas CPU virtuales / sockets?
- ¿Cuanta memoria?
- ¿Cantidad de discos? ¿Imagen utilizada?
- ¿Cantidad de nics?

Basándonos en esta información, modelaremos nuestra máquina virtual utilizando recursos de Terraform.

## Modelar la máquina virtual
El siguiente paso es modelar la máquina virtual en código. El repositorio de código contiene código Terraform de ejemplo. Contiene los siguientes archivos:
- `provider.tf`: Inicializa el proveedor y configura la versión de proveedor requerida
- `datasources.tf`: Realice búsquedas de entidades vinculadas requeridas (imagen, clúster, subred)
- `variables.tf`:  Declaraciones de variables (opcionales y obligatorias)
- `main.tf`: Definición de máquina virtual

El código más importante para la importación se encuentra en el `main.tf` archivo. Aquí definiremos las propiedades de la máquina virtual que queremos gestionar.  
**Nota:**Mantenga esta definición lo más cercana posible a la máquina virtual original; de lo contrario, Terraform actualizará la máquina virtual cuando realice una nueva `terraform apply`

## Importando la máquina virtual
Primero debemos asegurarnos de que terraform se haya inicializado:
terraform init
```
Output:
```
Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "nutanix" (terraform-providers/nutanix) 1.1.0...

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

A continuación, importaremos la máquina virtual. Para hacer esto usaremos el `terraform import` comando. Para importar un recurso, debemos informar a Terraform qué recurso importaremos seguido del ID de ese recurso. En este caso queremos importar el `nutanix_virtual_machine.myImportedVM` recurso que está identificado por el UUID de la máquina virtual que encontramos en Prism Element. Ejecute el comando:
```
terraform import nutanix_virtual_machine.myImportedVM fcb451ed-509e-480d-80b3-2d09eef6e1a0
```
Output:
```
nutanix_virtual_machine.myImportedVM: Importing from ID "fcb451ed-509e-480d-80b3-2d09eef6e1a0"...
nutanix_virtual_machine.myImportedVM: Import prepared!
  Prepared nutanix_virtual_machine for import
nutanix_virtual_machine.myImportedVM: Refreshing state... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0]

Import successful!

The resources that were imported are shown above. These resources are now in
your Terraform state and will henceforth be managed by Terraform.

```
You will notice that a new `terraform.tf` state file has been created. Terraform now has enough information to manage the resource.
Run a `terraform plan` to verify if the import was successful.
```
terraform plan
```
Output:
```
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

data.nutanix_subnet.mySubnet: Refreshing state...
data.nutanix_image.myImage: Refreshing state...
data.nutanix_cluster.myCluster: Refreshing state...
nutanix_virtual_machine.vm: Refreshing state... [id=58981fb6-ec5b-4ac4-8944-8822342c34ee]
nutanix_virtual_machine.myImportedVM: Refreshing state... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0]
###
# Removed output
###
        api_version                                      = "3.1"
        id                                               = "fcb451ed-509e-480d-80b3-2d09eef6e1a0"
      ~ memory_size_mib                                  = 1024 -> 2048

        name                                             = "yst-manual-deployment"
        num_sockets                                      = 1
        num_vcpus_per_socket                             = 2
###
# Removed output
###

Plan: 0 to add, 1 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

In my case above I did not model my resource identical to the running virtual machine. Terraform will detect this and will update the virtual machine so it is compliant to the desired state. When I now run `terraform apply` it will increase the amount of memory from 1GB to 2GB. 

```
terraform apply
```
Output:
```
data.nutanix_image.myImage: Refreshing state...
data.nutanix_subnet.mySubnet: Refreshing state...
data.nutanix_cluster.myCluster: Refreshing state...
nutanix_virtual_machine.vm: Refreshing state... [id=58981fb6-ec5b-4ac4-8944-8822342c34ee]
nutanix_virtual_machine.myImportedVM: Refreshing state... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0]
###
# Removed output
###
nutanix_virtual_machine.myImportedVM: Modifying... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0]
nutanix_virtual_machine.myImportedVM: Still modifying... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0, 10s elapsed]
nutanix_virtual_machine.myImportedVM: Still modifying... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0, 20s elapsed]
nutanix_virtual_machine.myImportedVM: Still modifying... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0, 30s elapsed]
nutanix_virtual_machine.myImportedVM: Still modifying... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0, 40s elapsed]
nutanix_virtual_machine.myImportedVM: Modifications complete after 47s [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```

## Modifying a resource
Modifying the virtual machine (or deleting) can now be performed via Terraform. You can add a disk (40 GB) by modifying the `myImportedVM` resource definition:

```
##main.tf##
## VM resource definition
resource "nutanix_virtual_machine" "myImportedVM" {
  name                 =  "yst-manual-deployment"
###
# Code
###
  disk_list {
    disk_size_bytes = 40 * 1024 * 1024 * 1024
  }
###
# Code
###
}
```

```
terraform apply
```
Output:
```
data.nutanix_image.myImage: Refreshing state...
data.nutanix_subnet.mySubnet: Refreshing state...
data.nutanix_cluster.myCluster: Refreshing state...
nutanix_virtual_machine.myImportedVM: Refreshing state... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:
###
# Removed output
###
      + disk_list {
          + data_source_reference  = (known after apply)
          + disk_size_bytes        = 42949672960
          + volume_group_reference = (known after apply)

          + device_properties {
              + device_type  = (known after apply)
              + disk_address = (known after apply)
            }

          + storage_config {
              + flash_mode = (known after apply)

              + storage_container_reference {
                  + kind = (known after apply)
                  + name = (known after apply)
                  + url  = (known after apply)
                  + uuid = (known after apply)
                }
            }
        }
###
# Removed output
###
nutanix_virtual_machine.myImportedVM: Still modifying... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0, 1m0s elapsed]
nutanix_virtual_machine.myImportedVM: Still modifying... [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0, 1m10s elapsed]
nutanix_virtual_machine.myImportedVM: Modifications complete after 1m15s [id=fcb451ed-509e-480d-80b3-2d09eef6e1a0]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```

Ahora tenemos una máquina virtual que se ejecuta en Nutanix administrada por Terraform que tiene 2 GB de memoria y un disco adicional de 40 GB.

![alt_text](https://github.com/yannickstruyf3/terraform-nutanix-import-example/raw/master/images/2_updated_vm.png )
