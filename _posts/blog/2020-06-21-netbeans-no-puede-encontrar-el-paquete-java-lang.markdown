---
layout: post
title:  "Netbeans no puede encontrar el paquete java.lang"
date:   2020-06-21 23:39:00 -0500
categories: Development
tags: linux java netbeans
---

## Descripción del problema

Con las versiones recientes de Apache Netbeans (v11 en adelante) frecuentemente encuentro problemas al actualizarlas ya que el IDE parece no reconocer las clases base del API de Java, mostrándo el mensaje `Fatal Error: Unable to find package java.lang in classpath or bootclasspath`.

## Causa del problema

Netbeans está buscando los archivos de las clases nativas de Java en ubicaciones incorrectas.

## Solución propuesta

La solución a este problema es muy simple, consiste en indicarle correctamente a Netbeans donde se encuentra la distribución del JDK que se desea utilizar.

El primer paso consiste en determinar exactamente cuál es el directorio de instalación del JDK.  En Linux es posible verificar las diferentes versiones que se tengan instaladas de la siguiente manera.

```
$ sudo update-alternatives --config java

There is only one alternative in link group java (providing /usr/bin/java):  
/usr/lib/jvm/java-13-openjdk-amd64/bin/java
Nothing to configure.
```

Con esta información es posible editar el archivo `etc/netbeans.conf` donde reside la configuración de Netbeans y ajustar correctamente el valor de la variable `netbeans_jdkhome`.

```
$ vi ~/netbeans/etc/netbeans.conf

## netbeans_jdkhome="/usr"
netbeans_jdkhome="/lib/jvm/java-13-openjdk-amd64"
```
