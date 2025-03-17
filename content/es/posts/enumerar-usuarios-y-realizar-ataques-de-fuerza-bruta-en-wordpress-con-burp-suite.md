+++
date = "2018-06-20"
title = "Enumerar usuarios y realizar ataques de fuerza bruta en WordPress con Burp Suite"
author = "Javier Olmedo"
toc = false
+++

En esta entrada vamos a ver como 👤 **enumerar usuarios y realizar ataques de fuerza bruta** para intentar acceder al panel de administración de un sitio web desarrollado con el CMS (Gestor de contenido) WordPress, para realizar este ataque, haremos uso de la herramienta 🛠 [Burp Suite.](https://portswigger.net/burp)

Existen varios scripts e incluso herramientas como 🛠 [WPScan](https://wpscan.org/) para realizar enumeración de usuarios y ataques por fuerza bruta, el inconveniente de estas herramientas, es que pueden realizar pruebas activas contra el sitio web que no deseamos realizar.

## ⚠ AVISO ⚠

> **No me hago responsable del mal uso que se pueda dar de lo explicado en este tutorial, estas pruebas siempre tienen que tener el consentimiento del propietario del sitio web o realizarlo sobre nuestras propias máquinas locales o aplicaciones web.**

## 📥 1. Instalar Burp Suite y configurarlo como proxy en Firefox

1.1 Si usamos distribuciones orientadas a la seguridad como Kali Linux, ya tendremos la herramienta Burp Suite instalada por defecto, si no, podemos descargarla desde su [web](https://portswigger.net/burp), la instalación no tiene pérdida.

1.2 Es aconsejable usar el [JDK de Java](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) en la versión 8.

1.3 Arrancamos Burp Suite y configuramos el navegador Firefox para que lo use como proxy, esto podemos hacerlo desde ⚙ **Opciones**, **General**, bajamos hasta abajo del todo y pulsamos en el **botón de configuración** en el apartado **Proxy de red**.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_001.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_001.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_001.png)

1.4 Desde aquí, añadimos la IP `127.0.0.1` (localhost) hacia el puerto `8080`, que es el puerto usado por defecto por Burp Suite, lo dejamos tal y como aparece en la imagen, eliminado las direcciones que aparecen en **No usar proxy para** y marcando la pestaña **Usar el mismo proxy para todo.**

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_002.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_002.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_002.png)

1.5 Accedemos a la dirección [http://localhost:8080](http://localhost:8080/) y descargamos el certificado.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_003.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_003.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_003.png)

1.6 Instalamos el certificado en el **equipo local** y en el almacén **Entidades de certificación raíz de confianza**.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_004.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_004.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_004.png)

1.7 Dentro de Firefox, vamos a ⚙ **Opciones**, 🔒 **Privacidad & Seguridad**, bajamos hasta abajo del todo y pulsamos en el botón **Ver certificados** dentro del apartado **Certificados**.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_005.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_005.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_005.png)

1.8 En la pestaña **Autoridades**, click en el botón **Importar** y seleccionamos el certificado que descargamos, también **marcamos los dos check de confianza en PortSwigger CA**.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_006.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_006.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_006.png)

## 👤 2. Enumeración de usuarios

2.1 En el paso anterior, hemos dejado todo configurado para que el tráfico pase por el Burp Suite cuando naveguemos, ahora, al sitio web le añadiremos `?author=1` en la URL y veremos que Burp Suite ha parado la petición, hacemos **click derecho** y lo enviamos al **Intruder** (Send to Intruder)

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_007.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_007.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_007.png)

2.2 El número **1** que hemos añadido a author, **por defecto es el ID del administrador** del sitio web, ahora vamos a realizar peticiones cambiando el número, por ejemplo, desde el 1 al 10 para **enumerar el resto de usuarios**, nos hemos fijado que al navegar a esta URL aparece una página con las entradas asociadas a este ID (usuario está activo).

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_008.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_008.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_008.png)

2.3 En este punto, **estaréis pensado que no sería necesario Burp Suite** para enumerar usuarios (si la enumeración es de pocos IDs), ya que cambiando manualmente el ID obtendríamos los usuarios, pero si lo intentamos con el ID 3 (he borrado el ID 2 para hacer esta entrada más real), veremos que no obtenemos respuesta, ¿O sí?

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_009.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_009.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_009.png)

2.4 Hemos visto que accediendo al ID 3 no hemos obtenido resultado, pero si **observamos el código fuente**, veremos que tiene el nombre de usuario en el atributo 👁‍ `<title>` dentro del `<head>`

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_010.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_010.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_010.png)

2.5 Ahora ya sabemos de dónde sacar el usuario, volvemos a Burp Suite, a la pestaña **Intruder** y marcamos la posición que queremos realizar la enumeración, haciendo click en el botón **Add**.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_011.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_011.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_011.png)

2.6 Para extraer el contenido de `<title>`, vamos a la pestaña **Options**, **Grep – Extract** y añadimos una condición (dejamos marcado el check **Extract the following items from responses**).

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_012.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_012.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_012.png)

2.7 Lo dejaremos tal y como aparece en la siguiente imagen, en **Start after expression:** y en **End at delimiter –**

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_013.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_013.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_013.png)

2.8 En la pestaña **Payloads**, en **Payload type:** elegimos el tipo **Numbers**, de tipo **secuencial del 2 al 10 con un salto.**

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_014.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_014.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_014.png)

2.9 **Lanzamos el ataque** y veremos los usuarios, el primero es el ID 1 con **código de respuesta 301**, y los demás usuarios válidos, devolverán un **código 200**.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_015.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_015.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_015.png)

2.10 Hemos obtenido los siguientes usuarios:

|ID|Usuario|
|---|---|
|1|administrador|
|3|colaborador|
|5|editor|
|6|autor|
|7|suscriptor|

## 💢 3. Realizar ataque por fuerza bruta a un usuario

3.1 Una vez conocidos los usuarios, intentaremos logearnos en el sitio mediante el formulario de acceso por defecto de WordPress, en la ruta `/wp-admin`, intentaremos logearnos con el usuario administrador y realizando fuerza bruta a la contraseña.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_016.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_016.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_016.png)

3.2 Al realizar la petición, Burp Suite la interceptará, volveremos a hacer uso del **Intruder** y nos fijaremos en el parámetro que contiene la contraseña.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_017.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_017.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_017.png)

3.3 En la pestaña **Positions** del **Intruder**, limpiaremos los parámetros por defecto que marca Burp Suite con el botón **Clear** y añadimos el contenido del parámetro `pwd=` con el botón **Add** como en la siguiente imagen.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_018.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_018.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_018.png)

3.4 En este paso, añadiremos un diccionario con posibles contraseñas para comprobar si alguna es utilizada por el administrador, por ejemplo, voy a usar un diccionario de las [1000 contraseñas más utilizadas](https://github.com/JavierOlmedo/MyWebHackingResources/tree/master/fuzzing/passwords), en el apartado **Payload Options [Simple list]**, click en el botón **Load** y lo añadimos.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_019.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_019.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_019.png)

3.5 **Lanzamos el ataque** y una vez finalizado, ordenamos los códigos de respuesta del servidor para comprobar si tenemos un **302** (contraseña válida).

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_020.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_020.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_020.png)

3.6 Vemos que tenemos un código 302 con el payload **qwerty**, esta sería la contraseña del administrador. Volvemos al panel de acceso en `/wp-config` para comprobar si es correcta y logearnos con las credenciales `administrador:qwerty`.

[![/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_021.png](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_021.png)](/images/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite_021.png)

Hasta aquí la entrada, hemos visto una alternativa al uso de las herramientas más utilizadas como WPScan para enumerar usuario en WordPress y realizar ataques de fuerza bruta, hasta la próxima!! 👋