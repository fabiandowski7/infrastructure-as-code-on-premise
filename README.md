# infrastructure-as-code-on-premise

This tutorial is intended to show what the **Infrastructure as Code** (**IaC**) is, why we need it, and how it can help you manage your infrastructure more efficiently.

It is practice-based, meaning I don't give much theory on what Infrastructure as Code is in the beginning of the tutorial, but instead let you feel it through the practice first. At the end of the tutorial, I summarize some of the key points about Infrastructure as Code based on what you learn through the labs.

This tutorial is not meant to give a complete guide on how to use a specific tool like Ansible or Terraform, instead it focuses on how these tools work in general and what problems they solve.

> The tutorial was inspired by [Kubernetes the Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way) tutorial. I used it as an example to structure this one.

_See [my presentation at DevOpsDays Silicon Valley](https://www.youtube.com/watch?v=XbcW2B7roLo&t=) in which I talk more in depth about the tutorial._

## Need your help!

This tutorial was created in 2018 and wasn't being kept up to date. Therefore, some of the instructions may not work due to updated APIs, obsolete repositories, etc. I apologize for that, but I currently don't have time to update this tutorial. So if you find that something is broken and you find a fix, please submit a PR. 

When submitting a PR, make sure you include a description for it, i.e. what's broken and a test plan for the fix.

Also, note that some of things need to be updated in several different repositories at the same time. Main repositories used in this tutorial:

* https://github.com/Artemmkin/infrastructure-as-code-tutorial
* https://github.com/Artemmkin/infrastructure-as-code-example
* https://github.com/Artemmkin/raddit

## Publico Objetivo

El público objetivo de este tutorial es cualquier persona interesada o que trabaje en TI.

## Herramientas Utilizadas

* Consul
* Jenkins
* Packer
* Terraform
* Ansible
* Bitbucket
* Docker
* Docker Compose
* Kubernetes

## Laboratorio

Este tutorial asume que tiene acceso a una Plataforma VMware o Nutanix on-premise. Si bien se usan estas plataformas para los requisitos básicos de infraestructura, las lecciones aprendidas en este instructivo se pueden aplicar a otras plataformas.

* [Introduccion](docs/00-introduccion.md)
* [Prerequisitos](docs/01-prerequisitos.md)
* [Consul](docs/02-consul.md)
* [Jenkins](docs/03-jenkins.md)
* [Packer](docs/04-packer.md)
* [Terraform](docs/05-terraform.md)
* [Ansible](docs/06-ansible.md)
* [Bitbucket](docs/07-bitbucket.md)
* [Docker](docs/08-docker.md)
* [Docker Compose](docs/09-docker-compose.md)
* [Kubernetes](docs/10-kubernetes.md)
* [Que es Infrastructura como Codigo?](docs/11-que-es-iac.md)
