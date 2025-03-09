+++
date = "2017-06-11"
title = "Metasploitable3: Crea una máquina vulnerable para probar tus ataques"
author = "Javier Olmedo"
toc = false
+++

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_banner.jpg](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_banner.jpg)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_banner.jpg)

## ¿Qué es Metasploitable3 y porqué usarlo?

Metasploitable3 es una **máquina virtual gratuita de Windows Server 2008 que cuenta con varias vulnerabilidades** listas para ser explotadas y practicar nuestras técnicas de hacking o simular ataques, Metasploitable3 ha sido **desarrollado por Rapid7**, los mismos **desarrolladores** del conocido **Framework MetaSploit**.

Es **usado por gran parte de la industria de la ciberseguridad** por varios motivos, cómo por ejemplo, la **formación de personal** para la explotación de redes, **pruebas de software** o demostración y **viabilidad de ataques**.

Usar una máquina virtual para probar nuestros ataques es **la manera más cómoda y rápida de conocer hasta dónde podemos llegar** a explotar un sistema sin ponerlo en riesgo, además, siempre contamos con la ventaja de que **si algo sale mal, volvemos a reinstalar** y tenemos de nuevo un sistema vulnerable en cuestión de minutos.

## ¿Qué pasos debo de seguir para instalar y configurar Metasploitable3? 

**Explicaré brevemente lo pasos** que seguiremos a lo largo de este tutorial y **después en cada apartado lo veremos con más detalle**, estos son los pasos:

1. Instalación de VirtualBox
2. Packer, herramienta de creación de imágenes de sistema
3. Parte de Vagrant
4. Cliente Git
5. Instalación de Metasploitable3

## Parte I – Instalación de VirtualBox

VirtualBox es un **software de virtualización** que permite **emular un sistema operativo**, para su instalación, accedemos a su descarga desde su web oficial en https://www.virtualbox.org/wiki/Downloads y descargamos el paquete correspondiente a nuestro sistema, en mi caso, Windows.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_001.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_001.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_001.png)

La instalación es sencilla, **ejecutamos el archivo .exe** descargado y hacemos **click en siguiente en todas las ventanas que aparezcan**, no debemos de configurar nada aquí, la única advertencia que podemos ver es la que nos indica que nuestra interfaz de red será reiniciada.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_002.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_002.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_002.png)

Comentar que **usaremos VirtualBox para emular la máquina de Metasploitable3** (Windows Server 2008) que crearemos a continuación.

## Parte II – Packer

Packer es una **herramienta de código abierto** para la **creación de imágenes de cualquier sistema operativo**. Es muy **fácil de usar** y automatiza muchísimo la creación de cualquier tipo de imagen de sistema.

Para descargar Packer, visitamos su web oficial en https://www.packer.io/downloads.html y elegimos nuestro sistema operativo.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_003.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_003.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_003.png)

Una vez descargado, tendremos un **archivo zip** que deberemos **descomprimirlo para obtener** un archivo ejecutable llamado **packer.exe**.

Este archivo tendremos que **copiarlo** en la ruta **C:\Program Files\Packer** (La carpeta Packer tendremos que crearla previamente antes de copiar el archivo packer.exe)

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_004.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_004.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_004.png)

El siguiente paso, consiste en **añadir Packer** a las **variables de entorno del sistema**, para llegar a las variables del sistema, pulsamos las teclas **Windows + R** y escribimos:

```bash
rundll32.exe sysdm.cpl,EditEnvironmentVariables
```

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_005.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_005.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_005.png)

En la parte de **Variables de sistema** buscamos la variable **Path** y hacemos click en el botón **Editar**.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_006.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_006.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_006.png)

Tenemos que añadir una nueva entrada, **click** en el **botón Nuevo** y escribimos **C:\Program Files\Packer** o **%ProgramFiles%\Packer**

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_007.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_007.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_007.png)

Para **comprobar si packer** está correctamente **añadido en las variables del sistema, abrimos una consola de comandos** (Windows + R y escribimos cmd) y llamamos al ejecutalble tecleando packer, si nos muestra sus opciones, es que lo tenemos todo preparado.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_008.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_008.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_008.png)

## Parte III – Vagrant

Vagrant es una herramienta para la **creación y configuración de entornos de desarrollo virtualizados**, su descarga la podemos hacer desde su web https://www.vagrantup.com/downloads.html

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_009.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_009.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_009.png)

Su instalación es sencilla, una vez **descargado el paquete msi**, hacemos **click en siguiente en todas las ventanas** que nos aparezcan, **al finalizar la instalación** nos solicitará **reiniciar el equipo**.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_010.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_010.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_010.png)

Después de reiniciar, vamos a **instalar un plugin** necesario para Vagrant llamado **reload**, abrimos de nuevo una consola de comandos y escribimos:

```bash
vagrant plugin install vagrant-reload
```

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_011.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_011.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_011.png)

Con esto, terminamos la parte de Vagrant.

## Parte IV – Cliente Git

Seguro que muchos de vosotros ya lo conocéis, Git es un **software de control de versiones**, y para la realización de este tutorial, necesitamos **instalar su cliente** para poder descargar los **archivos necesarios** para la creación de Metasploitable3 desde el **repositorio oficial**.

Git podemos encontrarlo en https://git-scm.com/downloads para varias plataformas.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_012.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_012.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_012.png)

Se recomienda que **durante su instalación, dejemos todas las opciones por defecto** que nos marque el instalador, podemos dejar la opción de **«Crear acceso directo»** si nos es más cómodo para después ejecutarlo.

Llegados a este paso, vamos a descargar (o mejor dicho, **clonar**) el **repositorio de MetaSploitable3** a nuestros disco, yo guardaré el repositorio en **«Mis documentos»**, por lo tanto, **abrimos una consola de Git y nos posicionamos sobre la carpeta Documentos**:

```bash
cd Documents/
```

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_013.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_013.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_013.png)

Necesitamos la URL del repositorio de MetaSploitable3, lo podemos encontrar en https://github.com/rapid7/metasploitable3, seguidamente **copiamos la URL al portapapeles** desde el botón situado debajo de **«Clone or Download»**

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_014.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_014.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_014.png)

Y ahora desde la consola de Git ejecutamos el comando:

```bash
git clone https://github.com/rapid7/metasploitable3.git
```

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_015.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_015.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_015.png)

Una vez descargado, **veremos la carpeta metasploitable3 en «Mis Documentos»**, ya tendremos **todo preparado para el último paso**.

## Parte V – Instalación de MetaSploitable3

Dentro de la **carpeta metasploitable3**, podemos ver un archivo llamado **build_win2008.ps1**

Este **archivo de PowerShell** nos va a ayudar a **automatizar todo el proceso**, hacemos **click derecho** sobre él y en la opción **«Ejecutar con PowerShell»**.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_016.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_016.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_016.png)

Nos **aparecerá un mensaje de alerta** de cambio de directivas, pulsamos la **tecla O y después Enter**.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_017.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_017.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_017.png)

Una vez hecho esto, **esperamos** a que termine de descargar todo y **cree nuestro «Box»** de MetaSploitable3.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_018.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_018.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_018.png)

De nuevo, **volvemos a abrir PowerShell** y nos **posicionamos sobre la carpeta metasploitable3** de **«Mis documentos«**, ahora lo que haremos será **añadir el «Box»** de MetaSploitable3 **a Vagrant** con el comando:

```bash
vagrant box add .\windows_2008_r2_virtualbox.box --name=metasploitable3
```

Después **actualizamos con el comando**:

```bash
vagrant up
```

Este proceso dura bastante tiempo, una vez finalizado, si todo ha ido bien, al abrir VirtualBox **veremos MetaSploitable3 en la lista de máquinas**.

[![/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_019.png](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_019.png)](/images/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques/metasploitable3-crea-una-maquina-vulnerable-para-probar-tus-ataques_019.png)

Sólo nos falta configurar los parámetros de la máquina y arrancarla, el usuario por defecto es **vagrant** con la contraseña **vagrant**

Esto ha sido todo por hoy 🙂

Saludos!!