+++
date = "2019-03-31"
title = "Instalación de Golang y Goca en Kali Linux"
author = "Javier Olmedo"
toc = false
+++

[![/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_banner.png](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_banner.png)](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_banner.png)

Golang, es un lenguaje de programación moderno desarrollado por Google, inspirado en la sintaxis de C, es multiplataforma y orientado a objetos. Muchos de los proyectos Open Source, están pasando a este lenguaje, un ejemplo es [Aquatone](https://github.com/michenriksen/aquatone) o [Goca](https://github.com/gocaio/goca), además, [existen repositorios](https://github.com/dreddsa5dies/goHackTools) interesantes con variedad de herramientas de hacking desarrolladas en este lenguaje, por lo tanto, es recomendado tenerlo en nuestra máquina por si necesitáramos usarlo.

En esta entrada, vamos a ver como instalar Golang en nuestra máquina Kali Linux desde su binario.

## 1. Descargar el archivo comprimido del sitio web oficial

Visita el sitio web de [Golang](https://golang.org/) y descarga el archivo para el sistema operativo Linux.

[![/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_001.png](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_001.png)](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_001.png)

Extraemos el contenido en `/usr/local` con el comando:

```bash
tar -C /usr/local -xzf go1.12.1.linux-amd64.tar.gz
```

## 2. Añadir GOPATH al archivo Bashrc

Con la variable `GOPATH`, estableceremos una ubicación para nuestro espacio de trabajo, por ejemplo, la estableceremos dentro de `/root/go`, editamos el archivo `.bashrc` ubicado en nuestra carpeta personal (si no lo encuentras, debes de marcar la opción «ver archivos ocultos)

```bash
vim ~/.bashrc
```

Al final del archivo, **añadimos** las siguientes líneas:

```txt
# Mi configuración de GO  
export GOPATH=/root/go  
export GOROOT=/usr/local/go  
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

Ahora, actualizamos el archivo `.bashrc` con el comando

```bash
source ~/.bashrc
```

## 3. Descargar un proyecto y probar si todo está correcto

[![/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_002.png](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_002.png)](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_002.png)

Descargamos un proyecto desde GitHub (por ejemplo Goca) con el comando:

```bash
go get github.com/gocaio/goca
```

En nuestro espacio de trabajo `/root/go`, veremos ahora las carpetas:

[![/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_003.png](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_003.png)](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_003.png)

1. El directo `src` es usado para lo paquetes de código fuente
2. El directorio `pkg` contiene los objetos del paquete compilados a partir del código fuente
3. En el directorio `bin` se encuentra el archivo binario ejecutable completo

Entramos en el directorio `src` de goca

```bash
cd go/src/github.com/gocaio/goca/
```

Y lanzamos los siguientes comados para generar el archivo bin:

```bash
export GO111MODULE=on   
go get ./…
```

Ahora, dentro de `bin` tendremos el archivo goca

[![/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_004.png](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_004.png)](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_004.png)

Para lanzarlo

```bash
./goca
```

[![/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_005.png](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_005.png)](/images/instalacion-de-golang-y-goca-en-kali-linux/instalacion-de-golang-y-goca-en-kali-linux_005.png)

Espero os haya servido de ayuda!!

Hasta la próxima!! 🤙