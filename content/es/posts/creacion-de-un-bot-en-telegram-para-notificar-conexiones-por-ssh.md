+++
date = "2020-04-09"
title = "Creación de un bot en Telegram para notificar conexiones por SSH"
author = "Miguel Garrido"
toc = false
+++

Ahora que estamos más tiempo en casa debido a la situación excepcional que vivimos a causa del Covid-19, podemos aprovechar e intentar agregar algún tipo de seguridad en nuestra red interna, por eso pense en escribir este breve post, tan sencillo como útil… la de notificarnos cuando alguien accede a uno de nuestros dispositivos.

En este artículo, vamos a crear un bot de Telegram que nos avise cada vez que se inicie sesión por SSH en alguno de nuestros dispositivos (para este ejemplo, usaré mi Raspberry).

Encontré un script en github [https://github.com/MyTheValentinus/ssh-login-alert-telegram](https://github.com/MyTheValentinus/ssh-login-alert-telegram) (todos los créditos son de [@MyTheValentinus](https://github.com/MyTheValentinus), yo no he tenido nada que ver en la creación de ese script), simplemente vengo a explicar cómo utilizarlo en nuestros dispositivos y como generar el bot en Telegram.

**Requisitos**

Para poder configurar todo necesitamos:

- Una Raspberry con acceso por SSH
- Cuenta Telegram

**Proceso**

El primer paso será crear el bot de Telegram. Desde la propia aplicación se puede buscar @BotFather o vía web accedemos al siguiente enlace [https://telegram.me/BotFather](https://telegram.me/BotFather) 

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_001.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_001.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_001.png)

Información de BotFather

Una vez abierto lo primero que haremos será iniciar el bot con el comando:

```bash
/start
```

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_002.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_002.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_002.png)

Comando /start para iniciar la comunicación con BotFather

El bot nos mostrará la lista de comandos que tiene disponibles para interactuar con él, para crear un bot nuevo necesitaríamos escribir:

```bash
/newbot
```

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_003.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_003.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_003.png)

Comando /newbot para iniciar el bot

Posteriormente, nos solicitará introducir el nombre que deseamos darle a nuestro bot, en este caso será `Pruebademo_bot`

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_004.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_004.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_004.png)

Nombre del bot

A continuación, nos pide generar el nombre de usuario del bot, para esta demostración vamos a poner `**demohackpuntes_bot**` (cada uno puede poner el que desee, pero recordar que debe ser nombre_bot).

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_005.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_005.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_005.png)

Nombre del usuario asociado al bot

Una vez asignados los nombres, se generará un token que tendremos que guardar.

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_006.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_006.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_006.png)

Token del bot

Para comprobar si se ha creado correctamente el bot, deberíamos ir desde la aplicación y buscar [@demohackpuntes_bot](https://web.telegram.org/#/im?p=%40demohackpuntes_bot):

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_007.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_007.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_007.png)

Búsqueda del bot en Telegram

Es posible que exista un botón que indique “Iniciar”, aunque se recomienda escribir cualquier frase en el chat para activarlo.

Por último, tendremos que ir al siguiente enlace, [https://api.telegram.org/bot<KEY>/getUpdates](https://api.telegram.org/bot%3CKEY%3E/getUpdates) (sustituir la palabra [KEY] por el token anterior) y nos devolverá una imagen similar a esta:

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_008.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_008.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_008.png)

ID del usuario asociado al bot

Ahí obtendremos el id del usuario que necesitaremos para configurar el script.

Ahora procederemos a descargar el script de [https://github.com/MyTheValentinus/ssh-login-alert-telegram](https://github.com/MyTheValentinus/ssh-login-alert-telegram). Desde nuestra Raspberry debemos seguir estos pasos para la instalación:

- En primer lugar debemos descargar el script con el siguiente comando`:`

```bash
cd /opt/ && git clone https://github.com/MyTheValentinus/ssh-login-alert-telegram
```

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_009.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_009.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_009.png)

Descarga del script

- Para editar la configuración de las variables del archivo credentials.config, debemos escribir:

```bash
nano ssh-login-alert-telegram/nano credentials.config.
```

Allí debemos introducir tanto el id de usuario como el token. Y en él añadimos dos valores tal y como se puede apreciar en la imagen:

**USERID**: Como recordatorio, en este campo se debe introducir el id del usuario asociado al bot. Para obtener este campo hay que enviar un mensaje cualquiera a nuestro bot a través de Telegram y acceder a la ruta [https://api.telegram.org/bot<KEY>/getUpdates](https://api.telegram.org/bot%3CKEY%3E/getUpdates) (sustituimos la parte de <KEY> por el token de antes).

**KEY**: Es el token que nos ha proporcionado @BotFather al crear nuestro bot.

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_010.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_010.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_010.png)

Edición del archivo credentials.config

- De manera opcional podemos modificar el archivo **alert.sh** (para añadir parámetros o traducirlo) https://hackpuntes.com/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_011.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_011.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_011.png)

Edición del script alert.sh

- Ejecutar el script con el comando:

```bash
bash deploy.sh
```

- Probar el script. Como podéis comprobar el script funciona a las mil maravillas.

[![/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_012.png](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_012.png)](/images/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh/creacion-de-un-bot-en-telegram-para-notificar-conexiones-por-ssh_012.png)

Prueba de funcionamiento

Espero que os haya gustado y entretenido un rato.

Nos vemos 😉