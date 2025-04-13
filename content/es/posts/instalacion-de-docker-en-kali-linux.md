+++
date = "2019-05-08"
title = "Instalación de Docker en Kali Linux"
author = "Javier Olmedo"
toc = false
+++

En esta entrada veremos como instalar [Docker Community Edition](https://www.docker.com/) en nuestra máquina Kali Linux 2019.

## 1. Añadir la clave PGP de Docker

```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

## 2. Configurar el repositorio de Docker

```bash
echo 'deb https://download.docker.com/linux/debian stretch stable' > /etc/apt/sources.list.d/docker.list
```

## 3. Actualizar

```bash
apt-get update
```

## 4. Eliminar versiones obsoletas de Docker (en caso que las tuviéramos)

```bash
apt-get remove docker docker-engine docker.io
```

## 5. Instalar Docker

```bash
apt-get install docker-ce
```

## 6. Comprobar si Docker se instaló correctamente

```bash
docker run hello-world
```

## 7. Comprobar la versión de Docker instalada

```bash
docker version
```

## Extras

### Añadir usuario al grupo de docker

```bash
usermod -aG docker $USER
```

### Arrancar Docker

```bash
systemctl start docker
```

### Arrancar Docker al inicio

```bash
systemctl enable docker
```

### Instalar DockStation (Gestor de dockers)

```bash
wget https://github.com/DockStation/dockstation/releases/download/v1.5.1/dockstation_1.5.1_amd64.deb
```

```bash
dpkg -i dockstation_1.5.1_amd64.deb
```

[![/images/instalacion-de-docker-en-kali-linux/instalacion-de-docker-en-kali-linux_001.png](/images/instalacion-de-docker-en-kali-linux/instalacion-de-docker-en-kali-linux_001.png)](/images/instalacion-de-docker-en-kali-linux/instalacion-de-docker-en-kali-linux_001.png)

DockStation con docker de MOBSF en ejecución

Hasta la próxima!!