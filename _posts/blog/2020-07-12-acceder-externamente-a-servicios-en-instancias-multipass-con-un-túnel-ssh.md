---
layout: post
title: Acceder externamente a servicios en instancias Multipass con un túnel SSH
date: 2020-07-12 01:48 -0500
categories: Linux
tags: linux ubuntu multipass ssh tunnel
---

## Descripción del problema

A la fecha, [Multipass](https://multipass.run/) no permite el acceso remoto (desde fuera del anfitrión) a los servicios provistos por las instancias ya que sólo soporta redes NAT.

Según se ha revisado en los registros de [*issues* del proyecto](https://github.com/canonical/multipass) en próximas versiones se estará incorporando la posibilidad de crear redes en *Bridge* lo cual solucionaría esta situación.

## Solución propuesta

Mientras el proyecto incluye la solución final, es posible crear túneles SSH entre un puerto [1] específico (servicio) de la máquina virtual Multipass y un puerto [2] del equipo anfitrión.  Una vez establecido este túnel, los clientes externos podrán acceder al servicio a través del puerto [2] y la dirección IP o nombre FQDN del equipo anfitrión.

Para lograr esto es necesario ejecutar el siguiente comando en el equipo anfitrión.

```
$ sudo ssh $REMOTE_USER@$REMOTE_IP -L $LOCAL_PORT:$REMOTE_IP:$REMOTE_PORT -N \
 	-i $MULTIPASS_PRIVATE_KEY
```
A continuación se describe el significado de cada variable de ambiente utilizada en el comando.

| `REMOTE_USER` | Usuario de conexión SSH con la máquina virtual (`ubuntu`) |
| `REMOTE_IP` | Dirección IP de la máquina virtual Multipass que provee el servicio que se desea exponer |
| `REMOTE_PORT` | Puerto del servicio que se desea exponer en la máquina virtual Multipass |
| `LOCAL_PORT` | Puerto local en la máquina anfitrión que recibirá el túnel y a través del cual se accederá al servicio remoto |
| `MULTIPASS_PRIVATE_KEY` | Ubicación de la llave privada para la autenticación |

Además se describe el significado de las opciones del comando SSH utilizadas.

| `-L` | Especifica la información del puerto que va a ser reenviado (*forwarded*): `[bind_address:]local port:host:remote port` |
| `-N` | Indica no ejecutar ningún comando remoto, sólo realizar el reenvío de puertos. |
| `-i` | Determina el archivo desde el cual se tomará la identidad (llave privada) para realizar la autenticación de llave pública. |
| `-f` | envía el proceso a segundo plano (*background*) justo después de su ejecución (**opcional**) |

Por ejemplo:

```
$ sudo ssh ubuntu@10.218.248.11 -L 8080:10.218.248.11:80 -N \
 	-i /var/snap/multipass/common/data/multipassd/ssh-keys/id_rsa
```

Este túnel permite acceder externamente al servicio provisto por la máquina virtual cuya dirección IP es `10.218.248.11` a través de su puerto `80` desde el puerto `8080` del equipo anfitrión.

## Recursos

 1. SSH tunnel  
 [https://www.ssh.com/ssh/tunneling/](https://www.ssh.com/ssh/tunneling/)
 1. SSH Port Forwarding Example  
 [https://www.ssh.com/ssh/tunneling/example](https://www.ssh.com/ssh/tunneling/example)
 1. SSH Command  
 [https://www.ssh.com/ssh/command/](https://www.ssh.com/ssh/command/)
 1. Opening and closing an SSH tunnel in a shell script the smart way  
 [https://gist.github.com/scy/6781836](https://gist.github.com/scy/6781836)