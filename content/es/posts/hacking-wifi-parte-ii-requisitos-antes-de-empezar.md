+++
date = "2015-03-25"
title = "Hacking Wifi – Parte II – Requisitos antes de empezar"
author = "Javier Olmedo"
+++

[![/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_banner.jpg](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_banner.jpg)](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_banner.jpg)

**Antes de empezar** con estas entradas, aclararé que deberemos de probar nuestros ataques **únicamente sobre redes propias**, la idea de estas entradas es para fines educativos y de aprendizaje, **no me responsabilizo de un mal uso de ellas**.

**Acceder a una red Wi-Fi ajena** a la nuestra, se traduce jurídicamente en conectarse fraudulentamente a través de un sistema de conexión inalámbrico para evitar tener que soportar su coste y que lo asuma la empresa proveedora del servicio o bien un particular, algo que **está penado** en los art. 255, 256 y 623.4 de nuestro Código Penal, en resumen estaréis cometiendo un **delito**.

## ¿Que necesito antes de empezar?

Si tienes algo de conocimiento sobre sistemas Linux, te será mas ameno seguir las entradas, en caso contrario no te preocupes porque lo explicaré todo.

1. Lo más importante de todo, **necesitaremos un Linux**, preferiblemente si está basada en **Debian**, como dije en el anterior post, hay distribuciones que ya vienen preparadas para auditar redes, pero nosotros **aprenderemos desde 0**. Aconsejo si eres nuevo utilizar **[Ubuntu](https://ubuntu.com/download/desktop)**.

[![/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_001.png](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_001.png)](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_001.png)

2. Tarjeta de red, preferiblemente con el **chipset RTL8187L**, una de las mas conocidas es la ALFA AWUS036H de 1W.

[![/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_002.jpg](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_002.jpg)](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_002.jpg)

3. Suite de software de seguridad inalámbrica **Aircrack-ng**, consiste en un analizador de paquetes de redes, un crackeador de redes **[WEP](https://es.wikipedia.org/wiki/Wired_Equivalent_Privacy)** y **[WPA/WPA2-PSK](https://es.wikipedia.org/wiki/Wi-Fi_Protected_Access)** y otro conjunto de herramientas.

[![/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_003.jpg](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_003.jpg)](/images/hacking-wifi-parte-ii-requisitos-antes-de-empezar/hacking-wifi-parte-ii-requisitos-antes-de-empezar_003.jpg)

4. Cualquier router sin utilizar que tengáis por casa, podéis probar con el que estáis utilizando actualmente para conectaros a Internet, pero probaremos distintas configuraciones dependiendo de la situación para probar todos los ataques y podremos causar algún problema si hay mas usuarios.

Bueno, si ya tenemos esto preparado, en el siguiente post empezaremos a instalar Aircrack-ng y a probar nuestra tarjeta desde 0.

Un Saludo a todos y no olvides de compartir.