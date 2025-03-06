+++
date = "2016-10-08"
title = "Instalación de VirtualBox Guest Additions en Kali Linux 2016.2"
author = "Javier Olmedo"
toc = false
+++

Si eres de los que eliges **VirtualBox** para trabajar con tus **máquinas virtuales**, después de la instalación, habrás notado que el **tamaño de la pantalla es muy reducido** e impide que podamos trabajar con ella cómodamente, otras veces, echamos de menos el poder **integrar la máquina anfitrión con la virtual**, por ejemplo, algo tan simple como **copiar un enlace o URL** con el atajo de tecla **Ctrl + C** o **arrastrar un archivo para usarlo en la máquina virtual**, para ello, VirtualBox tiene las llamadas **Guest Additions**.

## ¿Qué son las Guest Additions?

Las Guest Additions es un **paquete de software** desarrollado por Oracle para VirtualBox que **mejora el rendimiento** de las máquinas virtuales (las optimiza) y **añade nuevas funciones**.

Entre alguna de esas funciones encontramos:

- **Carpetas compartidas**: Una forma fácil de poder **intercambiar archivos entre el sistema anfitrión y la máquina virtual** de manera rápida, normalmente yo siempre creo una carpeta llamada **«Compartida»** en la raíz de la instalación de la máquina virtual.
- **Integración del ratón**: Con esta integración, **podemos mover el ratón libremente entre la máquina virtual y la anfitrión** sin necesidad de pulsar ninguna tecla para **«capturarlo»**.
- **Mejor soporte de vídeo**. Incorporan un driver de vídeo que **ofrece una aceleración del vídeo y permite trabajar a altas resoluciones**, si no están instaladas, VirtualBox igualmente proporciona el driver, pero sólo con funciones básicas.
- **Sincronización horaria**. Con esto aseguramos que la hora del sistema virtualizado, sea la correcta.
- **Ventanas sin costuras**. Esta característica nos muestra la ventana del sistema operativo virtualizado **como parte de nuestra máquina anfitriona**, como si formara parte de él.
- **Portapapeles compartido**. Una de las características que más se usa sin duda alguna, **permite copiar y pegar de forma bidireccional independientemente de que estemos en la máquina virtual o anfitrión**.

## ¿Cómo instalarlas en Kali Linux 2016.2?

Podemos instalarlas en Kali Linux de una manera, **sencilla y rápida**, antes de nada, deberemos de **actualizar los repositorios**, esto podemos hacerlo con el comando:

```bash
apt-get update
```

Una vez actualizado los repositorios **lanzamos el comando**:

[![/images/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2_001.png](/images/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2_001.png)](/images/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2_001.png)

```bash
apt-get install -y virtualbox-guest-x11
```

[![/images/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2_002.png](/images/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2_002.png)](/images/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2/instalacion-de-virtualbox-guest-additions-en-kali-linux-2016-2_002.png)

Después de la instalación, reiniciamos para que se apliquen los cambios:

```bash
reboot
```

**Lo primero que notaremos**, será que la máquina virtual, **adaptará la pantalla a las dimensiones de la ventana** del sistema anfitrión, con esto nos aseguramos que las **Guest Additions están funcionando**.

Saludos y hasta la próxima.