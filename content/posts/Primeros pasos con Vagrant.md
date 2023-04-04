---
title: "Primeros pasos con Vagrant"
date: 2021-01-29T21:17:00-0500 
draft: false
type: "post"
showTableOfContents: true
tags: ["linux", "vagrant", "windows"]
# description: "A blog post"
# image: "/path/to/image.png"
---

# Introducción

Vagrant es un gestor de máquinas virtuales que soporta múltiples [proveedores](https://www.vagrantup.com/docs/providers) (entre ellos Virtualbox) facilitando su construcción (mediante el archivo `Vagrantfile`), gestión y aprovisionamiento.  HashiCorp, la empresa detrás de Vagrant, mantiene un catálogo en línea de imágenes predefinidas que agilizan la creación de las máquinas virtuales.

En la presente publicación se reseñan los comandos necesarios para realizar la instalación, gestión de cajas, creación y gestión de máquinas virtuales, así como consultar su información.

# Instalación

Instalar los siguientes paquetes de software de acuerdo con el sistema operativo y arquitectura que se tenga.

- Virtualbox  
  [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- Vagrant  
  [https://www.vagrantup.com/downloads](https://www.vagrantup.com/downloads)

Se sugiere descargar e instalar manualmente los paquetes requeridos, no utilizar las versiones disponibles en los repositorios estándar ya que generalmente se encuentran muy desactualizados.

# Gestión de cajas

Las cajas o *boxes* son imágenes preinstaladas de diferentes sistemas operativos y aplicaciones.

## Catálogo

HashiCorp, los creadores de Vagrant, mantienen un catálogo de cajas que pueden ser libremente descargadas y utilizadas por los usuarios.

- HashiCorp's Vagrant Cloud box catalog  
  [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)

En este catálogo pueden por ejemplo, ubicarse las imágenes oficiales de [Ubuntu](https://app.vagrantup.com/ubuntu) y en particular del [último LTS](https://app.vagrantup.com/ubuntu/boxes/jammy64).

## Descargar una caja manualmente

```
$ vagrant box add <NOMBRE>
```
Por ejemplo:

```
$ vagrant box add ubuntu/jammy64
```

## Listar las cajas disponibles localmente

```
$ vagrant box list
```

## Remover una caja disponible localmente

```
$ vagrant box remove <NOMBRE>
```

Por ejemplo:

```
$ vagrant box remove ubuntu/jammy64
```

# Creación de una máquina virtual

Para crear a una nueva máquina virtual, se debe ubicar en el directorio donde se almacenará la información del proyecto y ejecutar el comando `init` de la siguiente manera.

```
$ cd <RUTA>/<PROYECTO>

$ vagrant init <NOMBRE>
```

Siendo `<NOMBRE>` el identificador de la caja a partir de la cual se desea crear la máquina virtual.

Por ejemplo:

```
$ vagrant init ubuntu/jammy64
```

La ejecución de este comando generará un archivo `Vagrantfile` base que puede personalizarse antes de iniciar la máquina virtual partir de él.

Una vez se realicen los cambios necesarios en el `Vagrantfile` es posible validar su contenido antes de intentar iniciar una máquina virtual con él, mediante el siguiente comando.

```
$ vagrant validate
```

# Gestionar las máquinas virtuales

Para la ejecución de los siguientes comandos es necesario ubicarse en el directorio donde se encuentre el `Vagrantfile` o especificar el `id` de la máquina (ver `global-status`) sobre la cual se desea aplicar el comando.

## Iniciar la máquina virtual

```
$ vagrant up [<ID>]
```

## Detener la máquina virtual

```
$ vagrant halt [<ID>]
```

## Recargar la máquina virtual

Reinicia la máquina virtual para que se apliquen los cambios hechos sobre el `Vagrantfile`.

```
$ vagrant reload [<ID>]
```

## Destruir la máquina virtual

Remueve la máquina virtual y elimina los archivos asociados a esta.

```
$ vagrant destroy [<ID>]
```

## Suspender/reanudar la máquina virtual

Para suspender la máquina virtual, es decir, detenerla "congelada" manteniendo el estado actual, se debe ejecutar el siguiente comando.

```
$ vagrant suspend [<ID>]
```

Para continuar el uso de una máquina virtual suspendida, se debe utilizar el siguiente comando.

```
$ vagrant resume [<ID>]
```

## Conectarse a la máquina virtual

Para realizar una conexión SSH con la máquina virtual, se debe ejecutar el siguiente comando.

```
$ vagrant ssh [<ID>]
```

Para consultar la información de configuración de SSH de una máquina virtual específica.

```
$ vagrant ssh-config [<ID>]
```

# Consultar acerca de las máquinas virtuales

## Consultar la información general

```
$ vagrant global-status
```

Permite consultar la información general de las máquinas virtuales registradas en el servidor.

- Identificador
- Nombre
- Proveedor
- Estado
- Ubicación del `Vagrantfile`

## Consultar la información específica

```
$ vagrant status <ID>
```  

Permite verificar el estado de una máquina virtual de acuerdo con el `<ID>` especificado.

## Consultar los puertos redirigidos

Permite consultar los puertos que haya sido redirigidos entre la máquina virtual y la máquina anfitrión.

```
$ vagrant port [<ID>]
```  

# Recursos

- Vagrant Documentation  
  [https://www.vagrantup.com/docs](https://www.vagrantup.com/docs)
