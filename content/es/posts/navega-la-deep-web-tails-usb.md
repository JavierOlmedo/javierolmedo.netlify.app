+++
date = "2016-05-29"
title = "Navega por la Deep Web con Tails y un USB"
author = "Javier Olmedo"
toc = false
+++

Todo el mundo ha oído alguna vez hablar sobre la Deep Web, en la entrada de hoy, os mostraré como podemos **crear un USB** para **acceder a la Deep Web de manera segura, gratuita y anónima** con una distribución llamada **Tails**.

## ¿Que es la Deep Web?

La Deep Web, o **también conocida como Internet profunda o invisible**, es la parte de Internet que **no ha podido ser indexada por los motores de búsqueda**, ya sea por páginas privadas o material que ha sido ocultado, **quedando así en mayor parte de manera inaccesible**.

Se estima que **la Deep Web es 500 veces mayor que el Internet que conocemos**, siendo el 95 % de esta información públicamente inaccesible.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_001.jpg](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_001.jpg)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_001.jpg)

## ¿Que tiene la Deep Web en especial?

Normalmente cuando se habla sobre la Deep Web **se le atribuye todo tipo de acciones delictivas**, pero **no sólo es usada para estos fines**, debemos de recordar que **en algunas partes del mundo el acceso a Internet está restringido** por los gobiernos, y la Deep Web puede ser la única manera de saltarse las restricciones y tener acceso a Internet.

En la Deep Web nos podemos encontrar lo siguiente:

- Contenido almacenado por los gobiernos u organizaciones, como por ejemplo la NASA (información sobre investigaciones científicas, datos meteorológicos, información de personas).
- Venta de drogas.
- Venta de objetos robados o falsificados.
- Pornografía clasificada como ilegal.
- Mercado negro de sicarios.
- Documentos clasificados como los de wikileaks.
- Foros de crackers en busca de víctimas.
- Páginas para comprar o fabricar armas.
- Piratería de libros, películas, música, software, etc.
- Tráfico ilegal de órganos.

## Preparando nuestro USB con Tails

1. Preparar un USB (Con 2GB es suficiente)
2. Descargar la ISO de Tails desde su web oficial – [AQUI](https://tails.net/index.en.html)
3. Descargar un programa para crear un Live USB con Tails, podemos utilizar [YUMI Multiboot](https://pendrivelinux.com/yumi-multiboot-usb-creator/) o [Linux Live USB Creator](http://www.linuxliveusb.com/).

**Explicaré como hacerlo con YUMI Multiboot**, aunque es igual de sencillo hacerlo con Linux Live USB Creator.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_002.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_002.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_002.png)

**Una vez descargado YUMI ejecutamos el .exe**, nos aparecerá una ventana como la siguiente, a continuación comento como debemos de dejarlo configurado.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_003.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_003.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_003.png)

1. Elegimos nuestro USB.
2. Indicamos que queremos formatear la unidad antes de crear el Live USB
3. Seleccionamos Tails (Anonymous Browsing)
4. Buscamos la ISO que previamente nos hemos descargado desde la web oficial.
5. Iniciamos la creación del USB con Tails.

Nos saldrá una ventana de advertencia, que nos indica que **todo lo que tuviésemos en el USB se perderá** al crear el Live.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_004.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_004.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_004.png)

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_005.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_005.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_005.png)

Una vez terminado el proceso, YUMI nos preguntará si queremos **añadir mas ISOs al USB**, indicamos que no.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_006.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_006.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_006.png)

Una vez terminado, es hora de **reiniciar nuestro ordenador, y presionar la tecla de Boot Options durante el arranque**, normalmente **suele ser con la tecla ESC, F9 o F12**.

Cuando nos aparezca las opción de arranque, indicaremos que queremos arrancar desde el USB.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_007.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_007.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_007.png)

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_008.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_008.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_008.png)

Una vez arrancado Tails, **buscamos el navegador TOR**, dentro de **Applications**.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_009.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_009.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_009.png)

Y **comprobamos si estamos dentro de la red TOR y nuestra verdadera IP oculta**, lo podemos hacer accediendo a la web [whatismyipaddress.com](https://whatismyipaddress.com/).

Sin usar Tails veríamos esto:

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_010.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_010.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_010.png)

Es decir, **navegamos sin proteger nuestra IP**, y somos fácilmente rastreables, en cambio con Tails veríamos esto:

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_011.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_011.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_011.png)

**Aparentemente estamos navegando desde EEUU, en concreto desde la Universidad de Michigan**.

A partir de aquí, ya cada uno usará Tails para sus propios fines, os dejo aquí algunas cosas que me he encontrado, billetes falsos, drogas, venta de armas, etc.

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_012.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_012.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_012.png)

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_013.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_013.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_013.png)

[![/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_014.png](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_014.png)](/images/navega-la-deep-web-tails-usb/navega-la-deep-web-tails-usb_014.png)

Saludos y Happy Hacking!!!