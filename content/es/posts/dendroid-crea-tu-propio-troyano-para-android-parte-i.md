+++
date = "2015-06-13"
title = "Dendroid - Crea tu propio Troyano para Android - Parte I"
author = "Javier Olmedo"
+++

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_banner.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_banner.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_banner.png)

Android se ha convertido en el **sistema operativo más utilizado** en nuestros dispositivos móviles, objeto por el cual **ha sido blanco de muchos ciberdelincuentes**, que han pasado de atacar ordenadores personales a dispositivos móviles, puesto que es mas **fácil de atacar**, cuenta en cierta medida con **menos protección** y el porcentaje de **ataque con éxito es mayor**.

En esta entrada os mostraré como **crear nuestro propio troyano** para dispositivos Android, cada uno utilizará los conocimientos adquiridos en este tutorial con sus propios fines éticos, pero esto nos puede servir de gran ayuda en el caso que **nuestro móvil se pierda o nos lo roben**, ya que tendremos total control sobre el dispositivo que infectemos, recordad que infectar un dispositivo ajeno al nuestro sin consentimiento del propietario puede ser un **delito**.

**Existen 2 proyectos** muy conocidos para labores de administración remota de dispositivos Android, uno en **AndroRAT** y otro **DENDROID**. El primero es gratuito, pero es un proyecto que se ha quedado estancado y no ha seguido desarrollándose dado que empezó como un proyecto universitario, en cambio el segundo no es gratuito y puede conseguirse por el equivalente a **300$** en monedas no rastreables como BitCoins, aunque hace tiempo se filtró su código fuente y podéis conseguirlo facilmente buscando en [Google](https://www.google.es/webhp?sourceid=chrome-instant&ion=1&espv=2&es_th=1&ie=UTF-8#safe=off&q=dendroid+source+code).

El que utilizaré será **DENDROID**, y estas son algunas de sus características:

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_001.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_001.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_001.png)

```bash
# FUNCIONES
Ringer Up/Down
Media Up/Down
Screen On
Intercept On/Off
Block SMS On/Off
Record Audio
Record Video
Take Photo Front/Back
Record Calls On/Off
Get:
SMS Inbox/OutBox
Browser History/Bookmarks
Call History
Contacts
User Accounts
Installed Apps
Send SMS
Delete SMS
Send SMS To All Contacts
Call Numbers
Delete Call Log
Open Page
Open Dialog (Toast Notification)
Open App
HTTP Flood
Update App
Transfer Bot
```

Como veis está bastante completito y nos permite cualquier acción sobre el dispositivo una vez infectado.

## Requisitos antes de empezar

- Disponer de un Servidor Web con PHP y MySQL
- phpmyadmin
- OpenJDK JRE x64
- Java JDK

Yo he creado un subdominio en esta web para realizar el tutorial, llamado **dendroid.hackpuntes.com**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_002.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_002.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_002.png)

Este subdominio redirige a una carpeta que he montado en **public_html/dendroid**, lugar en el cual subiremos nuestros archivos.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_003.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_003.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_003.png)

### Configuración Panel de Control en Servidor Web

La primera parte del tutorial la dedicaré a mostraros como **crear el panel de administración** de nuestro **DENDROID**, desde él podremos controlar todos los dispositivos que tengamos infectados.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_004.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_004.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_004.png)

Si descomprimimos nuestro archivo .rar con el código fuente de DENDROID, nos encontraremos con 3 carpetas y un archivo, podemos echar un vistazo antes de empezar a montar el panel al archivo **readme.md**, en él nos explica como realizar todo el proceso, eso sí, en inglés.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_005.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_005.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_005.png)

En la carpeta Dendroid Panel, podemos ver los siguientes archivos:

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_006.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_006.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_006.png)

Otra vez mas tenemos un readme, esta vez mas especifico para ayudarnos a montar el panel en el servidor web, **primero nos centramos en la carpeta Panel**, que tienes estos archivos.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_007.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_007.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_007.png)

Tenemos que editar con algún editor de texto, los siguientes archivos:

```bash
applysettings.php
blockbot.php
clearawaiting.php
clearmessages.php
deletebot.php
deletefile.php
deletepics.php
functions.php
table.php
```

y cambiar el valor de la variable **$url** que por defecto es **«http://pizzachip.com/rat/»** al nombre de nuestro dominio donde almacenaremos el panel de control, en mi caso **«http://dendroid.hackpuntes.com»**, de esta manera.

**Antes**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_008.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_008.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_008.png)

**Después**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_009.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_009.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_009.png)

Este paso lo tendremos que realizar en todos los archivos que anteriormente he descrito y **asegurarnos que todos los archivos empiezan por <?php**.

Ademas en el archivo **reg.php** cambiaremos el valor de `$allowebDomains` respetando las **www**.

**Antes**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_010.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_010.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_010.png)

**Después**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_011.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_011.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_011.png)

Ahora toca modificar los siguientes archivos **para poner una contraseña**:

```bash
get.php
get-functions.php
new-upload.php
upload-pictures.php
```

Buscamos el valor **«keylimepie»** y lo sustituimos por nuestra contraseña:

**Antes**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_012.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_012.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_012.png)

**Después**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_013.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_013.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_013.png)

Es hora de pasar al servidor web, **necesitamos crear una base de datos**, con un **usuario con todos los privilegios**, debemos de recordar nombre de la base de datos, usuario, etc porque nos hará falta después.

Es posible que dependiendo del servidor que utilicéis los siguientes pasos pueden cambiar, mi servidor web utiliza **CPanel**, en caso de que el vuestro sea distinto y no sepáis como crear una base de datos con usuario con privilegios, poneos en contacto con la ayuda de vuestro hosting.

Creando base de datos.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_014.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_014.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_014.png)

Añadiendo un usuario a la base de datos.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_015.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_015.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_015.png)

Asignándole todos los privilegios.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_016.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_016.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_016.png)

Buenos ya tenemos la base de datos y el usuario, ahora debemos de crear las tablas dentro de la base de datos, para crearlas haremos lo siguiente, dentro de la carpeta **«Other Files»** tenemos un archivo que se llama `SQL.sql`, lo abrimos con el editor y **copiamos todo lo que contenga**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_017.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_017.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_017.png)

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_018.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_018.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_018.png)

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_019.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_019.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_019.png)

Ahora necesitamos ir a **phpmyadmin**, y en la base de datos que creamos anteriormente, tenenos una pestaña que dice SQL, **pegamos el código anterior y lo ejecutamos**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_020.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_020.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_020.png)

El resultado será las siguientes tablas creadas.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_021.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_021.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_021.png)

Es hora de subir todos los archivos que modificamos anteriormente por FTP al servidor, podemos utilizar un programa como [Filezilla]() para ello, simplemente arrastramos los archivos a la carpeta **public_html/dendroid** nos quedaría así:

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_022.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_022.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_022.png)

Nos dirigimos a la dirección de nuestro servidor donde tenemos almacenadas las carpetas, en mi caso como os comente anteriormente cree un subdominio.

Por lo tanto accedemos a **dendroid.hackpuntes.com** donde nos aparecerá la pantalla de configuración del panel, como esta:

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_023.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_023.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_023.png)

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_024.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_024.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_024.png)


En ella nos dice que es la primera vez que accedemos, que tenemos que crear una cuenta para el login, poner el nombre de la base de datos, el usuario de dicha base y también nos indica que está optimizado para funcionar en **Google Chrome**, pero yo lo he probado sobre Mozilla Firefox y sin problemas.

Si hacemos **Click** en **Begin Setup**, nos encontraremos con la que es posiblemente el paso mas importante a la hora de crear el panel de administración, la configuración de este.

Podéis dejarlo como veis en la imagen, es lo que trae **por defecto** y añadir en los campos correspondientes los datos que nos pide, menos mal que aún nos acordamos del nombre que pusimos a la base de datos, el usuario y la contraseña :).

En la parte derecha, podéis ver con fondo amarillo el campo **Username y Password**, este será el login para **poder acceder al panel de control**, nada que ver con el user y pass que creamos para la base de datos, además, es recomendable que no utilicemos las mismas contraseñas.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_025.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_025.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_025.png)

Una vez rellenos los campos, **Click** en el boton **Continue**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_026.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_026.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_026.png)

Y nos dirá que todo está completo, **Click** en **Finish Setup** y nos aparecerá el login, podremos el **User y Pass** que pusimos en los campos amarillos del paso de configuración.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_027.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_027.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_027.png)

Y finalmente, ya estamos dentro de nuestro panel de administración, totalmente configurado y listo para crear el troyano y administrar los dispositivos desde él.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_028.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_028.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-i/dendroid-crea-tu-propio-troyano-para-android-parte-i_028.png)

Nos vemos en la siguiente entrada, donde configuraremos el troyano e infectaremos a las victimas para tomar el control del dispositivo.

Un Saludo.

Happy Android Hacking!!!!!