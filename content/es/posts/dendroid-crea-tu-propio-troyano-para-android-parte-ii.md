+++
date = "2015-07-09"
title = "Dendroid - Crea tu propio Troyano para Android - Parte II"
author = "Javier Olmedo"
toc = false
+++

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_banner.jpg](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_banner.jpg)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_banner.jpg)

En la [entrada anterior](https://hackpuntes.com/dendroid-crea-tu-propio-troyano-para-android-parte-i/) vimos como **crear un panel de control** en un servidor web para administrar los dispositivos que infectaremos posteriormente, en esta entrada, os mostraré como crear una **APK Maliciosa** para **infectar dispositivos** que ejecuten el sistema operativo Android (Smartphone, Tablets, etc).

Para este tutorial, necesitaremos tener instalado [Eclipse](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/marsr) + [SDK de Android](https://developer.android.com/sdk/installing/index.html?pkg=tools) y tener instalado en el sistema el [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), si tenéis cualquier tipo de problema en instalarlo, podéis buscar en [Google](https://www.google.es/webhp?sourceid=chrome-instant&ion=1&espv=2&es_th=1&ie=UTF-8#safe=off&q=eclipse+%2B+sdk+%2B+jdk), existen miles de tutoriales de como preparar Eclipse para el desarrollo de aplicaciones Android.

Una vez tenemos todo instalado en el sistema, abrimos nuestro Eclipse y deberemos **importar el proyecto**, nos vamos a **File –> Import**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_001.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_001.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_001.png)

El proyecto que tenemos que importar, está **dentro de la carpeta Dendroid APK**, recordemos que ya hemos utilizado la carpeta Dendroid Panel y nos faltaría por utilizar una última carpeta llamada APKBinder, que es la necesaria para «camuflar» nuestra APK Maliciosa.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_002.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_002.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_002.png)

Una vez hacemos **Click** en **Import**, debemos de indicarle a Eclipse que es un **proyecto ya existente**, en la categoría **Android**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_003.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_003.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_003.png)

En **Root Directory** buscamos la ruta de la carpeta **Dendroid APK**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_004.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_004.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_004.png)

Debemos de asegurarnos que la casilla **Copy projects into workspace** está marcada, **de lo contrario la carpeta Dendroid APK desaparecerá** y no la podremos volver a utilizar.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_005.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_005.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_005.png)

**Click** en **Finish** y nos aparecerá en la parte izquierda de nuestro Eclipse el proyecto de Dendroid APK, **listo para modificar y compilar**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_006.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_006.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_006.png)

Es de buena práctica, renombrar los proyectos, yo lo llamaré **DendroidAPK**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_007.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_007.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_007.png)

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_008.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_008.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_008.png)

**Vamos a modificar** del proyecto solamente **el valor de 3 variables**, estas variables se encuentran en la clase **DroidianService.java**, dentro del paquete **com.connect** y en la carpeta **src**.

Se llaman **encodedURL, backupURL y encodedPassword**, si nos fijamos bien, veremos que su valor está codificado en **Base64**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_009.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_009.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_009.png)

**El valor de estas variables deberemos de borrarlo**, y los dejamos vacíos, como en la siguiente imagen.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_010.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_010.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_010.png)

**¿Y que debemos de poner si los hemos dejado vacíos?** Lo primero será visitar la siguiente pagina web, [CRYPO](http://crypo.in.ua/tools/eng_base64c.php) es una **herramienta online para cifrar y descifrar** en multitud de sistemas de numeración, nosotros la que utilizaremos será **BASE64**, y deberemos de poner la dirección en la cual quedo instalado nuestro panel de control, recordad que la mía era **http://dendroid.hackpuntes.com/**

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_011.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_011.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_011.png)

Una vez lo tenemos puesto, hacemos **Click** en el boton **encrypt** y nos aparecerá algo parecido a esto:

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_012.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_012.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_012.png)

Ese **String**, es que tendremos que **poner como valor a las variables encodedURL y backupURL**.

Para la variable **encodedPassword** repetiremos el mismo paso, pero esta vez deberemos de cifrar la **contraseña que pusimos a la hora de crear nuestro panel de control** (aquella que cambiamos el valor de **keylimepie**).

Nos quedaría así (evidentemente con la contraseña agregada)

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_013.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_013.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_013.png)

Ya tenemos el proyecto modificado, ahora **deberemos de compilarlo y generar el APK**, nos vamos a la pestaña **File –> Export**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_014.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_014.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_014.png)

Y le indicamos que queremos una **Aplicación Android (APK)**.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_015.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_015.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_015.png)

Hacemos **Click** en **Next** y en la siguiente ventana buscamos nuestro proyecto.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_016.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_016.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_016.png)

La primera vez, nos pedirá una clave, si ya la tenemos marcamos la opción de **Use existing keystore**, en caso contrario la crearemos, debemos de indicar la ubicación en la cual se guardara la llave y ponemos un contraseña (otra distinta de las utilizadas hasta ahora).

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_017.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_017.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_017.png)

Rellenamos los campos.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_018.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_018.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_018.png)

Y por ultimo nos pedirá que marquemos la ruta en la cual se generará el APK.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_019.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_019.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_019.png)

**Click** en **Finish** y ya tenemos nuestra **APK maliciosa**, lista para infectar a dispositivos.

[![/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_020.png](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_020.png)](/images/dendroid-crea-tu-propio-troyano-para-android-parte-ii/dendroid-crea-tu-propio-troyano-para-android-parte-ii_020.png)

Está claro que esta APK **la hace falta el «último retoque»**, en el siguiente capítulo, os mostraré como utilizar la última carpeta del proyecto Dendroid, llamada **APKBinder**, la cual nos **permite fusionar nuestra APK Maliciosa con cualquier otra APK legítima del Google Play**, y esta se instalará en el dispositivo de manera silenciosa a la vez que se instale la APK legítima.

Un Saludo y nos vemos en la próxima entrada.