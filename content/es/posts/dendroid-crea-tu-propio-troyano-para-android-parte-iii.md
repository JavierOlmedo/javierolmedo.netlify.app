+++
date = "2015-09-13"
title = "Dendroid - Crea tu propio Troyano para Android - Parte III"
author = "Javier Olmedo"
+++

Esta será la **última parte de configuración** de nuestro troyano para Android, en la cual bindearemos (fusionaremos o uniremos) la APK maliciosa generada en el [anterior tutorial](https://hackpuntes.com/dendroid-crea-tu-propio-troyano-para-android-parte-ii/) por nosotros con otra APK legítima de Google Play para camuflarla.

Lo primero, deberemos de **descargarnos una APK desde Google Play**, yo voy a elegir la APK de Angry Birds.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_001.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_001.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_001.png)

Existen muchas maneras de conseguir la APK desde Google Play, yo **os explicaré dos posibilidades**, la primera **accediendo directamente** en nuestro terminal a la ruta **/data/app** (lugar donde se almacenan las APK cuando se descargan de Google Play antes de su instalación) y **copiando la APK a nuestro PC** (esta opción es la más recomendada, pero posiblemente necesitemos ser root en nuestro terminal).

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_002.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_002.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_002.png)

y otra haciendo **uso de una web de terceros** para descargarla directamente en nuestro PC (menos recomendada y mas fácil).

Si os decidís por la segunda opción, podéis hacer uso de esta aplicación web llamada [APK Downloader](http://apps.evozi.com/apk-downloader/) pero debo de comentaros que la APK que nos descargemos **puede estar «modificada»** respecto a la original y tener algún que otro **regalito**, esta conclusión la he sacado yo, desde las comprobaciones que he hecho, dado que realmente lo que hace la aplicación es pedir una URL, obtiene el nombre de la aplicación y accede a un directorio interno del servidor donde se almacenan APKs modificadas, por lo tanto **esta aplicación web no obtiene la APK de Google Play**, si no de su propio servidor.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_003.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_003.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_003.png)

Independientemente de como hayamos obtenido la APK, vamos a **proceder a unirlas**, para ello nos hará falta la última carpeta del proyecto Dendroid que utilizaremos, llamada **APKBinder**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_004.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_004.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_004.png)

Veremos los siguientes archivos

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_005.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_005.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_005.png)

Debemos de abrir desde Visual Studio el archivo APKBinder, podemos descargar la versión gratuita de Visual Studio 2015 desde [AQUI](https://www.visualstudio.com/es-es/downloads/download-visual-studio-vs.aspx).

Nos vamos a **Archivo –> Abrir –> Proyecto o solución**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_006.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_006.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_006.png)

Buscamos nuestra carpeta APKBinder y **abrimos el archivo para Visual Studio llamado también APKBinder**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_007.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_007.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_007.png)

**Botón derecho** sobre el proyecto y **Click** en **Generar**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_008.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_008.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_008.png)

Iniciamos.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_009.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_009.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_009.png)

Y como último paso, hacemos **Click** en el **TextBox** de **Source APK** y buscamos nuestro **DendroidAPK**, en **Target APK** buscamos la **APK de Angry Birds**, y ya tendremos la apk bindeada.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_010.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_010.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-iii/dendroid-crea-tu-propio-troyano-para-android-parte-iii_010.png)

Podemos utilizar otros **binders más recomendados**, pero en este caso he utilizado todo lo que vienen en el proyecto Dendroid.

También se puede hacer uso del archivo **ejecutable jar** ubicado en **\APKBinder\bin\Debug**.

Y hasta aquí la serie de entradas sobre como crear un troyano para Android, espero que os haya gustado y sobre todo que hayáis aprendido.

Un Saludo y sed buenos.