---
layout: post
title: Ubuntu Multipass en Linux
date: 2020-07-07 01:55 -0500
categories: Linux
tags: linux ubuntu multipass snap
---

## Introducción

*Multipass* es un gestor liviano de máquinas virtuales diseñado para manejar fácilmente máquinas virtuales de Ubuntu sobre las plataformas Linux, MacOS y Windows utilizando KVM, HyperKit y Hyper-V respectivamente (el uso de Virtualbox es opcional).  Adicionalmente permite la especificación de metadatos para `cloud-init` lo que permite simular despliegues pequeños en la nube en el equipo local.

## Instalación

```
$ sudo snap install multipass --classic
```

## Imágenes

### Listar imágenes disponibles

```
$ multipass find

Image                       Aliases           Version          Description
snapcraft:core              core16            20200706         Snapcraft builder for Core 16
snapcraft:core18                              20200707         Snapcraft builder for Core 18
snapcraft:core20                              20200703         Snapcraft builder for Core 20
core                        core16            20200213         Ubuntu Core 16
core18                                        20200210         Ubuntu Core 18
16.04                       xenial            20200610         Ubuntu 16.04 LTS
18.04                       bionic,lts        20200701         Ubuntu 18.04 LTS
19.10                       eoan              20200703         Ubuntu 19.10
20.04                       focal             20200702         Ubuntu 20.04 LTS
daily:20.10                 devel,groovy      20200704         Ubuntu 20.10
appliance:adguard-home                        20200605         Ubuntu AdGuard Home Appliance
appliance:mosquitto                           20200605         Ubuntu Mosquitto Appliance
appliance:nextcloud                           20200605         Ubuntu Nextcloud Appliance
appliance:openhab                             20200605         Ubuntu openHAB Home Appliance
appliance:plexmediaserver                     20200605         Ubuntu Plex Media Server Appliance
```

## Instancias

### Listar las instancias

```
$ multipass list

Name                    State             IPv4             Image
pruebas                 Running           10.218.248.234   Ubuntu 20.04 LTS
experimentos            Stopped           --               Ubuntu 20.04 LTS
laboratorio             Suspended         --               Ubuntu 20.04 LTS
otra                    Deleted           --               Not Available
```

### Crear una instancia

```
$ multipass launch --name <NOMBRE> <IMAGEN>
```

Por ejemplo:

```
$ multipass launch --name pruebas 20.04
```

También es posible especificar la cantidad de CPU, disco y memoria que se le va asignar a la instancia de la siguiente manera.

```
$ multipass launch --name <NOMBRE> --cpus <CPUS> --disk <DISK> --mem <MEM> <IMAGEN>
```

| `CPUS` | Cantidad de CPUs a asignar (mínimo 1, por defecto 1).  Número entero. |
| `DISK` | Espacio en disco a asignar (mínimo 512M, por defecto 5G).  Número entero con sufijo `K`, `M` o `G`. |
| `MEM` | Cantidad de memoria a asignar (mínimo 128M, por defecto 1G).  Número entero con sufijo `K`, `M` o `G`. |

### Iniciar una instancia

Permite iniciar una instancia que se encuentra *detenida* o *suspendida*.

```
$ multipass start <NOMBRE>
```

### Detener una instancia

```
$ multipass stop <NOMBRE>
```

### Reiniciar una instancia

```
$ multipass restart <NOMBRE>
```

### Suspender una instancia

```
$ multipass suspend <NOMBRE>
```

### Destruir una instancia

```
$ multipass delete <NOMBRE>
```

Una vez una instancia se encuentra en modo `Deleted` puede ser recuperada con el comando `recover`.

```
$ multipass recover <NOMBRE>
```

O pueden ser removidas (con archivos incluidos) para siempre con el comando `purge`.

```
$ multipass purge
```

## Información

### Obtener información de una instancia

```
$ multipass info <NOMBRE>

Name:           pruebas
State:          Running
IPv4:           10.218.248.234
Release:        Ubuntu 20.04 LTS
Image hash:     f9d2cf216314 (Ubuntu 20.04 LTS)
Load:           1.05 0.28 0.09
Disk usage:     1.3G out of 4.7G
Memory usage:   123.7M out of 981.2M
```

## Interacción

### Conectar al shell de una instancia

```
$ multipass shell <NOMBRE>
```

### Ejecutar un comando en la instancia

```
$ multipass exec <NOMBRE> -- <COMANDO>
```

### Montar directorios

Permite montar rutas del sistema de archivos local en el sistema de archivos de la instancia.

```
$ multipass mount <RUTA_LOCAL> <NOMBRE>:<RUTA_INSTANCIA>
```

| **Advertencia.**  `RUTA_LOCAL` debe estar obligatoriamente bajo el directorio `$HOME`. ([Issue #1598](https://github.com/canonical/multipass/issues/1598#issuecomment-645259898)) |

Es posible mapear los `UID` y `GID` entre los usuarios del sistema operativo anfitrión y el de la instancia utilizando las opciones `--uid-map <host>:<instance>` y `--gid-map <host>:<instance>` respectivamente.

Desmontar un punto de montaje entre el equipo local y la instancia se realiza de la siguiente manera.

```
$ multipass umount <NOMBRE>:<RUTA_INSTANCIA>
```

### Transferir archivos

Permite copiar archivos desde el sistema de arhivos local al de la instancia y viceversa.

```
$ multipass transfer <ORIGEN> <NOMBRE>:<DESTINO>
```

Cuando se hable de una ruta en el sistema de archivos de una instancia se deberá utilizar el formato `<NOMBRE>:<DESTINO>`.

## Gestión

Para [acceder a los mensajes](https://multipass.run/docs/accessing-logs) (*logs*) de Multipass utilice el parámetro `--verbose` en cualquier invocación o consulte los mensajes generales a través del siguiente comando.

```
$ journalctl --unit snap.multipass*
```

## Recursos

 - Canonical Multipass  
   [https://multipass.run/](https://multipass.run/)
 - Código fuente de Multipass en Github  
   [https://github.com/canonical/multipass](https://github.com/canonical/multipass)
