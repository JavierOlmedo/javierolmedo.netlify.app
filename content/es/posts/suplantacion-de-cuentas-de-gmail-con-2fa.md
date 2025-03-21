+++
date = "2019-01-22"
title = "Suplantación de cuentas de Gmail con 2FA"
author = "Alejandro Fernández"
toc = false
+++

En este caso, vamos a aprender como los atacantes consiguen obtener acceso a cuentas protegidas mediante el doble factor de autenticación en **Gmail**.

El doble factor de autenticación (**2FA**) es una medida de seguridad empleada por los sistemas de autenticación que proporciona una capa de seguridad adicional al sistema. Consiste en que el usuario que quiere autenticarse presente al menos dos pruebas que demuestren que es quien dice ser.

En la mayoría de los casos basados en este esquema se suele usar como factores de autenticación:

- Algo que el usuario sabe: **contraseña**.
- Algo que el usuario posee: **dispositivo móvil**.

Combinando ambos, el usuario se autentica en el sistema.

Al configurar el mecanismo de doble autenticación los usuarios piensan que ya están completamente protegidos y que nadie va a acceder a su cuenta. Nada más lejos de la realidad.

Pero entonces, si tengo configurado mi móvil para que cada vez que inicie sesión se me envié un código de verificación que debo introducir para poder iniciar sesión, ¿Cómo los atacantes pueden obtener dicho código?¿Pueden acceder a mi móvil?

La técnica conocida como **phising** consiste en engañar a una víctima para que introduzca sus credenciales en una web maliciosa, que tiene apariencia de ser real. La web almacena las credenciales para que el atacante tenga acceso a ellas.

Hasta aquí todo correcto, pero en el momento que se configura un **2FA** el servicio legítimo debe enviarnos un mensaje de texto con un código, ¿Cómo los atacantes son capaces de enviarme dicho código y que sea válido para el sistema legítimo?

Pues muy fácil, ¿Qué pasaría si la web maliciosa a la vez que la víctima introduce sus credenciales usase dichas credenciales para automáticamente iniciar sesión en el sistema legítimo?

Correcto, al iniciar sesión en el sistema se enviaría un código de verificación a la víctima. Posteriormente, la víctima introducirá el código en la web maliciosa y el atacante completará la suplantación del inicio de sesión haciendo uso de dicho código.

[![/images/suplantacion-de-cuentas-de-gmail-con-2fa/suplantacion-de-cuentas-de-gmail-con-2fa_001.png](/images/suplantacion-de-cuentas-de-gmail-con-2fa/suplantacion-de-cuentas-de-gmail-con-2fa_001.png)](/images/suplantacion-de-cuentas-de-gmail-con-2fa/suplantacion-de-cuentas-de-gmail-con-2fa_001.png)

Para demostrar el funcionamiento de esto, he preparado una prueba de concepto que aplica la técnica anteriormente comentada, para el sistema de autenticación de **Gmail**. La prueba está realizada sobre este servicio, pero el escenario se puede exportar a otros sistemas de autenticación.

Antes de realizar el ataque, he generado un clon de la web original de **Gmail,** incluyendo en el código llamadas a scripts de php para almacenar las credenciales y para ejecutar otros scripts. El proceso del ataque es el siguiente:

- El atacante lanza un servidor web con la web falsa. En este caso, he usado el servidor web Apache.
- El atacante redirige a la víctima al portal falso, mediante técnicas como **DNS Spoofing** o **ingeniería social**.
- La víctima introducirá su correo electrónico y su contraseña en la web falsa. Una vez introducidos se la presentará otra web, para introducir el código de verificación, similar a la que se emplea en el sistema legítimo.
- Al enviar los credenciales, la máquina del atacante los almacenará y lanzará un script que haciendo uso de **Selenium** (Framework para la automatización de navegadores web) ejecutará automáticamente un navegador, irá a la web real de **Gmail** e introducirá las credenciales obtenidas.
- La introducción de las credenciales en la web legítima de **Gmail**, por parte del atacante, provocará que se envie un sms con el código **2FA** al móvil de la víctima.
- Cuando la víctima reciba el código, lo introducirá en la web falsa y será redirigida a la web original de **Gmail**, lo que la hará pensar que ha introducido un código erróneo.
- La máquina atacante al recibir el código completará el inicio de sesión en la web legítima de **Gmail**, obteniendo acceso a la cuenta de la víctima.

A continuación podemos ver un gif del funcionamiento de la POC:

[![/images/suplantacion-de-cuentas-de-gmail-con-2fa/suplantacion-de-cuentas-de-gmail-con-2fa_002.gif](/images/suplantacion-de-cuentas-de-gmail-con-2fa/suplantacion-de-cuentas-de-gmail-con-2fa_002.gif)](/images/suplantacion-de-cuentas-de-gmail-con-2fa/suplantacion-de-cuentas-de-gmail-con-2fa_002.gif)

En mi [Github](https://github.com/afernandezb92) he creado un repositorio con todo el contenido necesario para reproducir la POC, esto incluye:

- Portal Web clon de **Gmail**.
- Script para almacenar las credenciales de la víctima.
- Script para lanzar script de **Selenium**.
- Script de **Selenium** (encargado de lanzar el navegador web e introducir las credenciales de la víctima automáticamente).

En la descripción del repositorio se dan más detalles de cada fichero. Tal y como se describe en el mismo, todo el código es mejorable ya que no se ha tenido en cuenta la calidad del código si no únicamente el objetivo de reproducir el ataque.

Puedes acceder al repositorio pulsando [aquí](https://github.com/afernandezb92/2FAGmailPhising).

## ⚠️ DISCLAIMER ⚠️

> No me hago responsable del mal uso que se le puedan dar a las técnicas y herramientas empleadas en esta entrada. El objetivo es puramente didáctico.

Un saludo a todos.