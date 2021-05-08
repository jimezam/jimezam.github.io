---
layout: post
title: Niveles de ejecución en Linux Ubuntu 21
date: 2021-05-08 16:33 -0500
categories: Linux
tags: linux ubuntu runlevel systemd
---

## Introducción

A continuación se describe la gestión básica de los _niveles de ejecución_ en Ubuntu Linux, los cuales han cambiado conceptualmente ahora con `systemd`.

Anteriormente se considereban los siguientes.

| _Runlevel_ | Description |
| --- | --- |
| 0 | Detener sistema operativo |
| 1 | Modo monousuario (sin red) |
| 3 | Modo multiusuario (con red) | 
| 5 | Incluye un ambiente gráfico |
| 6 | Reiniciar el sistema operativo |

Actualmente se consideran de la siguiente manera con los _targets_.

| _Runlevel_ | _Target_ |
| --- | --- |
| 0 | `poweroff.target` |
| 1 | `rescue.target` |
| 3 | `multi-user.target` |
| 5 | `graphical.target` |
| 6 | `reboot.target` |

## Comandos relacionados

### Consultar los _targets_ disponibles

```
$ systemctl list-units --type target

  UNIT                       LOAD   ACTIVE SUB    DESCRIPTION                  
  basic.target               loaded active active Basic System                 
  bluetooth.target           loaded active active Bluetooth                    
  cryptsetup.target          loaded active active Local Encrypted Volumes      
  getty.target               loaded active active Login Prompts                
  graphical.target           loaded active active Graphical Interface          
  local-fs-pre.target        loaded active active Local File Systems (Pre)     
  local-fs.target            loaded active active Local File Systems           
  machines.target            loaded active active Containers                   
  multi-user.target          loaded active active Multi-User System            
  network-online.target      loaded active active Network is Online            
  network.target             loaded active active Network                      
  nss-lookup.target          loaded active active Host and Network Name Lookups
  nss-user-lookup.target     loaded active active User and Group Name Lookups  
  paths.target               loaded active active Paths                        
  remote-fs-pre.target       loaded active active Remote File Systems (Pre)    
  remote-fs.target           loaded active active Remote File Systems          
  rpcbind.target             loaded active active RPC Port Mapper              
  slices.target              loaded active active Slices                       
  sockets.target             loaded active active Sockets                      
  sound.target               loaded active active Sound Card                   
  swap.target                loaded active active Swap                         
  sysinit.target             loaded active active System Initialization        
  time-set.target            loaded active active System Time Set              
  time-sync.target           loaded active active System Time Synchronized     
  timers.target              loaded active active Timers                       
  virt-guest-shutdown.target loaded active active Libvirt guests shutdown      

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

26 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.
```

### Consultar el _target_ por defecto

```
$ systemctl get-default

graphical.target
``` 

También es posible consultar el estado del _target_ por defecto.

```
$ systemctl status default.target

● graphical.target - Graphical Interface
     Loaded: loaded (/lib/systemd/system/graphical.target; static; vendor preset: enabled)
     Active: active since Thu 2021-05-06 15:53:56 -05; 2 days ago
       Docs: man:systemd.special(7)

may 06 15:53:56 silver systemd[1]: Reached target Graphical Interface.
```

### Cambiar temporalmente el _target_ actual

Para pasar a _runlevel_ 3, es decir multiusuario con red pero sin ambiente gráfico.

```
$ sudo systemctl isolate multi-user.target
```

Para pasar a _runlevel_ 5, es decir ambiente gráfico.

```
$ sudo systemctl isolate graphical.target
```

### Cambiar el _target_ por defecto

Esta modificación determina hasta que nivel de ejecución llegará el sistema operativo la próxima vez que se inicie.

Para pasar a _runlevel_ 3, es decir multiusuario con red pero sin ambiente gráfico.

```
$ sudo systemctl set-default multi-user.target
```

Para pasar a _runlevel_ 5, es decir ambiente gráfico.

```
$ sudo systemctl set-default graphical.target
```
