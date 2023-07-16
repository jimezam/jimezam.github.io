---
title: "Instalación de Docker en Ubuntu Linux"
date: 2023-04-18T01:24:18-05:00
draft: false
type: "post"
showTableOfContents: true
tags: ["linux", "docker", "ubuntu"]
# description: "A blog post"
# image: "/path/to/image.png"
---

# Introducción

En el presente artículo se detalla el proceso de instalación de Docker Engine en un Ubuntu Linux mediante el uso de un repositorio.

# Desinstalación

Es recomendable desinstalar cualquier versión previa que se tenga de Docker mediante el siguiente comando.

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

# Configuración del repositorio

## Permitir acceso a repositorios HTTPS

```bash
$ sudo apt-get update

$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
```

## Agregar la llave GPG del repositorio oficial

```bash
$ sudo install -m 0755 -d /etc/apt/keyrings

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

## Agregar el repositorio oficial

```bash
$ echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

# Instalación

```bash

$ sudo apt-get update

$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
``` 
# Postinstalación

Una vez instalados los paquetes, se recomienda realizar las siguientes tareas.

* Agregar al usuario al grupo Docker

  ```bash
  $ sudo usermod -a -G docker $USER
  ```

# Verificar el funcionamiento

Para verficiar que la pasada instalación de Docker fue exitosa, se debe crear y visualizar la salida de un contenedor *Hola Mundo*.

```bash
$ sudo docker run hello-world
```

La ejecución exitosa de este comando deberá mostrar una salida similar a la siguiente.

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:4e83453afed1b4fa1a3500525091dbfca6ce1e66903fd4c01ff015dbcb1ba33e
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

# Recursos

- Install Docker Engine on Ubuntu  
  https://docs.docker.com/engine/install/ubuntu/
- Docker - samples overview  
  https://docs.docker.com/samples/
