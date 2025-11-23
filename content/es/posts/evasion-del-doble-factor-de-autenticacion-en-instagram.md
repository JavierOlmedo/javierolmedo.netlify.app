+++
date = "2020-01-19"
title = "Evasión del doble factor de autenticación en Instagram"
author = "Alejandro Fernández"
toc = false
+++

El **doble factor de autenticación** (2FA) es una capa adicional de seguridad para proteger las cuentas de los usuarios en diferentes servicios de Internet. Muchas personas piensan que al tener habilitada esta característica sus cuentas están completamente protegidas, pero esto no es así.

Hace unos meses escribí una [entrada](https://hackpuntes.com/suplantacion-de-cuentas-de-gmail-con-2fa/) de cómo se realizaría el proceso de ataque para evadir el **2FA** en el servicio de **Gmail**.

En este caso, he preparado una herramienta similar a [2FAGmailPhishing](https://github.com/afernandezb92/2FAGmailPhising) pero para el servicio de **Instagram**.

[![/images/evasion-del-doble-factor-de-autenticacion-en-instagram/evasion-del-doble-factor-de-autenticacion-en-instagram_001.png](/images/evasion-del-doble-factor-de-autenticacion-en-instagram/evasion-del-doble-factor-de-autenticacion-en-instagram_001.png)](/images/evasion-del-doble-factor-de-autenticacion-en-instagram/evasion-del-doble-factor-de-autenticacion-en-instagram_001.png)

Al igual que la herramienta que creada para evadir el **2FA** del servicio de **Gmail**, el funcionamiento del ataque es similar:

- El atacante lanza un servidor web con el portal de **Instagram** suplantado. En este caso, la herramienta hace uso del servidor Apache que trae Kali Linux.
- El atacante redirige a la víctima al portal falso, mediante técnicas como **DNS Spoofing** o **ingeniería social**.
- La víctima introducirá su correo electrónico y su contraseña en el portal falso. Una vez introducidos se la presentará otra web, para introducir el código de verificación, similar a la que se emplea en el sistema legítimo.
- Al enviar los credenciales, la máquina del atacante los almacenará y lanzará un script que haciendo uso de **Selenium** (Framework para la automatización de navegadores web) ejecutará automáticamente un navegador, irá al portal de login de **Instagram** e introducirá las credenciales obtenidas.
- La introducción de las credenciales en el portal de login de **Instagram**, por parte del atacante, provocará que se envie un sms con el código **2FA** al móvil de la víctima.
- Cuando la víctima reciba el código, lo introducirá en el portal falso y será redirigida al portal original de **Instagram**, lo que la hará pensar que ha introducido un código erróneo.
- La máquina atacante al recibir el código completará el inicio de sesión en el portal de **Instagram**, obteniendo acceso a la cuenta de la víctima.

A continuación podemos ver un gif del funcionamiento del ataque:

[![/images/evasion-del-doble-factor-de-autenticacion-en-instagram/evasion-del-doble-factor-de-autenticacion-en-instagram_002.gif](/images/evasion-del-doble-factor-de-autenticacion-en-instagram/evasion-del-doble-factor-de-autenticacion-en-instagram_002.gif)](/images/evasion-del-doble-factor-de-autenticacion-en-instagram/evasion-del-doble-factor-de-autenticacion-en-instagram_002.gif)

He creado un [repositorio](https://github.com/afernandezb92/2FAInstagram) con la herramienta para reproducir el ataque, en el mismo se incluye la documentación necesaria para usar la herramienta.

**⚠️ DISCLAIMER ⚠️**

No me hago responsable del mal uso que se le puedan dar a las técnicas y herramientas empleadas en esta entrada. El objetivo es puramente didáctico.

Un saludo a todos.