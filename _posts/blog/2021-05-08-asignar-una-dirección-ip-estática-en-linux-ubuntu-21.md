---
layout: post
title: Asignar una dirección IP estática en Linux Ubuntu 21
date: 2021-05-08 16:59 -0500
categories: Linux
tags: linux ubuntu network netplan
---

## Introducción

En la presente publicación se describen los pasos necesarios para asignar la información de red de manera estática a una interfaz de red bajo Linux Ubuntu 21 el cual utiliza `netplan` para su gestión.

## Procedimiento

### Identificar interfaz de red

En este paso se identifica cuál es la interfaz de red que se va a modificar.  Esto se puede realizar de varias maneras, a continuación se muestran dos de ellas.

```
$ nmcli d

DEVICE  TYPE      STATE      CONNECTION         
enp0s3  ethernet  connected  Wired connection 1 
lo      loopback  unmanaged  --     
```

También es posible obtener esta información a través del siguiente comando.

```
$ networkctl list

IDX LINK   TYPE     OPERATIONAL SETUP
  1 lo     loopback carrier     unmanaged
  2 enp0s3 ether    routable    configured
```

En este caso se modificará la interfaz **enp0s3**.

### Puerta de enlace

En este paso se identificará la puerta de enlace (_gateway_) de la red, suponiendo que se cuenta actualmente con asignación dinámica con DHCP.

```
$ ip route

default via 192.168.0.1 dev enp0s3 proto dhcp metric 100 
169.254.0.0/16 dev enp0s3 scope link metric 1000 
192.168.0.0/24 dev enp0s3 proto kernel scope link src 192.168.0.30 metric 100 
```

En este caso, la puerta de enlace es la **192.168.0.1**.

### Configurar la interfaz de red

Para finalmente configurar la interfaz de red, se debe editar el archivo ubicado bajo `/etc/netplan`, este archivo puede recibir un nombre dependiendo de la versión y tipo de Ubuntu, por ejemplo `1-network-manager-all.yaml` (Linux Mint), `00-installer-config.yaml` (Ubuntu server), `01-network-manager-all.yaml` (Ubuntu desktop), entre otros (`01-netcfg.yaml`, `50-cloud-init.yaml`, etc).


Se recomienda hacer una copia de seguridad del archivo de configuración original.

```
$ sudo cp /etc/netplan/00-installer-config.yaml /etc/netplan/00-installer-config.yaml.bak
```

Editar el archivo mencionado y ajustar su contenido como se muestra a continuación.

```
$ sudo vi /etc/netplan/00-installer-config.yaml

network:
  version: 2
  renderer: systemd
  ethernets: 
    enp0s3:
      dhcp4: no 
      addresses: [192.168.0.200/24]
      gateway4: 192.168.0.1
      nameservers: 
        addresses: [8.8.8.8, 8.8.4.4]
```

A continuación se describen los valores utilizados durante esta configuración.

| Variable | Descripción |
| --- | --- |
| `renderer` | Puede ser `systemd` o `NetworkManager` según el que se utilice. |
| `enp0s3` | Intefaz de red a asignársele la dirección IP. |
| `addresses` | Dirección IP a asignarse de manera estática.  En este caso: `192.168.0.200/24`. |
| `gateway4` | Puerta de enlace de la red.  En este caso: `192.168.0.1`. |
| `nameservers` | Servidores DNS a utilizarse.  En este caso se usaron los gratuitos de Google: `8.8.8.8` y `8.8.4.4`. |

### Aplicar los cambios

Para hacer que se tengan en cuenta los cambios recién realizados es necesario ejecutar el siguiente comando.

```
$ sudo netplan apply
```

En caso de haber problemas, es posible utilizar la siguiente variante que muestra los mensajes de depuración asociados.

```
$ sudo netplan --debug apply
```

## Recursos

- Netplan configuration examples.  
  [https://netplan.io/examples/](https://netplan.io/examples/)