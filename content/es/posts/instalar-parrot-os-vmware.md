+++
date = "2016-04-17"
title = "Cómo instalar Parrot OS en VMware"
author = "Javier Olmedo"
toc = false
+++

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_banner.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_banner.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_banner.png)

Hoy vengo a hablaros sobre una de las distribuciones «top» orientadas a la seguridad informática y pentesting, **Parrot OS**, que junto a **Kali Linux** y **Bugtraq** son a día de hoy **las más utilizadas**.

Parrot OS es una **distribución de Linux basada en Debian** con el kernel de Linux 4.3 (en su versión 2.2.1), ha sido desarrollada por **un equipo de hackers italianos llamados FrozenBox, utiliza MATE 1.8.1 como escritorio** y debido a la gran cantidad de herramientas y la importancia de actualizarlas, **tiene un modelo de actualización continua** (Rolling release).

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_001.jpg](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_001.jpg)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_001.jpg)

Veamos cómo instalar esta excelente distribución de Linux sobre una máquina virtual en VMware.

Para descargarla, visitamos su **web oficial** en https://www.parrotsec.org/download.fx

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_002.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_002.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_002.png)

En VMware **«click»** en **«Create a New Virtual Machine»** o en el menú **«File» -> «New Virtual Machine»**.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_003.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_003.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_003.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_004.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_004.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_004.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_005.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_005.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_005.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_006.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_006.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_006.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_007.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_007.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_007.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_008.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_008.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_008.png)

**Si tenemos buena máquina**, y podemos permitírnoslo, seleccionaremos 1 procesador con 2 núcleos, y 2048 de RAM, en caso contrario, pondremos 1 procesador con 1 núcleo y 1024 o 512 de RAM.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_009.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_009.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_009.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_010.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_010.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_010.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_011.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_011.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_011.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_012.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_012.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_012.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_013.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_013.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_013.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_014.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_014.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_014.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_015.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_015.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_015.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_016.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_016.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_016.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_017.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_017.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_017.png)

Una vez preparada la máquina virtual, tendremos que **seleccionar la ISO** de Parrot OS que previamente nos hemos descargado **en la unidad CD/DVD virtual**, para ello vamos a **«Edit virtual machine settings»**.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_018.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_018.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_018.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_019.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_019.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_019.png)

Arrancamos nuestra máquina y comenzamos a instalar.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_020.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_020.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_020.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_021.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_021.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_021.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_022.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_022.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_022.png)

Al iniciar configuramos nuestro idioma, zona horaria y teclado.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_023.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_023.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_023.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_024.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_024.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_024.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_025.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_025.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_025.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_026.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_026.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_026.png)

Uno de los pasos más importantes, es definir la contraseña del usuario root, el usuario root (**superusuario**) es el nombre de la cuenta que posee todos los privilegios sobre el sistema y sus usuarios.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_027.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_027.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_027.png)

Después tendremos que configurar otra cuenta de usuario con su contraseña, está será la que usemos, siempre y cuando no sea necesario iniciar con la cuenta de root.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_028.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_028.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_028.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_029.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_029.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_029.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_030.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_030.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_030.png)

Configuramos la zona horaria, como dato a tener en cuenta, os diré que si queremos mantener el mayor anonimato en nuestras labores de pentesting, deberemos de iniciar la instalación con el idioma inglés, y configurar una zona horaria distinta a la cual nos encontramos.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_031.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_031.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_031.png)

Pasamos al particionado de discos, como es una máquina virtual y solo usaré Parrot OS utilizaré el método **«Guiado – utilizar todo el disco»** con todos los ficheros en una partición y sin cifrar.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_032.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_032.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_032.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_033.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_033.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_033.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_034.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_034.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_034.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_035.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_035.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_035.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_036.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_036.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_036.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_037.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_037.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_037.png)

Después de esperar la instalación, nos preguntará si queremos instalar el cargador de arranque, le indicaremos que sí, y dejaremos la que nos marqué por defecto /dev/sda.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_038.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_038.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_038.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_039.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_039.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_039.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_040.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_040.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_040.png)


Aquí hemos terminado la instalación, nos queda quitar la imagen ISO de Parrot OS que montamos sobre la unidad CD/DVD al principio de la instalación y dejar por defecto el HDD (autodetected).

Volvemos a **«Edit virtual machine settings»** y lo dejamos de la siguiente manera.

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_041.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_041.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_041.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_042.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_042.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_042.png)

Arrancamos para ver si todo ha sido instalado correctamente, iniciamos sesión como root y a disfrutar del hacking!!!!!

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_043.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_043.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_043.png)

[![/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_044.png](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_044.png)](/images/instalar-parrot-os-vmware/instalar-parrot-os-vmware_044.png)

Saludos y espero os sea de ayuda.