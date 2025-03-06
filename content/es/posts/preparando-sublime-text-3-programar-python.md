+++
date = "2016-06-28"
title = "Preparando Sublime Text 3 para programar en Python"
author = "Javier Olmedo"
toc = false
+++

Uno de los **lenguajes de programación que más interés** ha despertado en el mundo de la **Seguridad Informática y Hacking**, es sin duda **Python**.

Python es un **lenguaje de programación interpretado y orientado a objetos**, muy **fácil de aprender y potente**, desde mi punto de vista, recomendado para cualquier persona que desee **aprender a programar**, y mucho más, si desea orientar sus conocimiento a la seguridad de la información, muchas de las herramientas de hacking que se usan hoy en día, están escritas en este lenguaje.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_001.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_001.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_001.png)

En este tutorial, os mostraré **como preparar Sublime Text 3 para empezar a desarrollar en Python**.

Sublime Text es un **potente y ligero editor de texto**, su sistema de resaltado de sintaxis, su interfaz de color oscuro y el amplio abanico de plugins desarrollados por sus usuario, lo convierten sin lugar a dudas, en **la mejor opción para desarrollar en Python**.

## Descargando Python

Antes de empezar a preparar Sublime Text, vamos a descargar Python, podemos hacerlo desde el sitio oficial en https://www.python.org/downloads/

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_002.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_002.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_002.png)

Si nos fijamos, **Python dispone de dos versiones** actualmente, la 3.5.1 y 2.7.11, recomiendo que descarguemos las dos, aunque la mayoría de las herramientas se programan en la 2.7.11.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_003.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_003.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_003.png)

La **instalación** de Python es **bastante simple**, descargamos las versiones y ejecutamos el paquete descargado, durante la instalación es bastante importante que dejemos marcado la siguiente opción **Add Python.exe to Path**:

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_004.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_004.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_004.png)

Está opción **nos permitirá llamar a Python desde cualquier ruta en la que nos encontremos**, únicamente escribiendo python en la consola, seguido de la ruta de nuestro archivo, ejemplo:

```powershell
python C:\mis_programas_python\hola_mundo.py
```

Bueno, ya tenemos Python instalado en nuestro sistema, **vamos a descargar ahora Sublime Text**, lo podemos descargar desde aquí https://www.sublimetext.com/3

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_005.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_005.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_005.png)

Como vemos, es multiplataforma, lo podemos usar tanto en Windows, como en MAC o Linux, en mi caso lo descargaré para Windows.

## Preparando Sublime Text 3

Después de la instalación de Sublime Text, **lo primero** que debemos de hacer es **instalar el gestor de paquetes**. El gestor de paquetes **nos permite añadir y eliminar complementos de terceros** que mejorarán el entorno de desarrollo.

Para instalar el gestor de paquetes, debemos de abrir la consola, esto podemos hacerlo con la combinación de tecla **CTRL + `** o  accediendo al menú **View – Show Console**.

Una vez abierto la consola, copiamos y pegamos el siguiente código de https://packagecontrol.io/installation

```bash
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

y pulsamos **Enter**. El gestor de paquetes tardará unos segundos en instalarse, una vez instalado, ya podremos instalar plugins de terceros en nuestro Sublime Text.

## Instalación de plugins

A continuación muestro una **lista de plugins que no deben de faltarnos**, para instalarlos, debemos de abrir la **Paleta de Comandos**, podemos abrirla con la combinación de tecla **CTRL + SHIFT + P** o en el menú **Tools – Command Palette**.

Una vez abierta la **Paleta de Comandos**, nos tiene que aparecer lo siguiente:

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_006.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_006.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_006.png)

Elegiremos la opción **Install Package** e instalaremos los siguientes puglins:(algunos plugins es posible que no aparezcan, adjuntaré links para descargarlos manualmente):

### Anaconda

Anaconda es un paquete de Python extremadamente potente para Sublime que **convierte nuestro editor de texto en un completo IDE**, algunas de sus características son:

- Autocompletado de código Python.
- Muestra errores de sintaxis y PEP8.
- Ofrece documentación de Python.
- Refactor (cambiar el nombre) del objeto.
- Autoimport.
- [Muchas más opciones](https://damnwidget.github.io/anaconda/IDE/).

### SidebarEnhancements

Muy importante, ofrece una barra lateral desde la cual podemos crear, borrar, editar, etc los archivos.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_007.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_007.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_007.png)

Podemos descargarlo de aquí: https://github.com/titoBouzout/SideBarEnhancements

### Alignment

Un simple plugin que permite **alinear el código**, si te gusta programar de forma muy organizada y entender el código en un simple vistazo, este plugin no puede faltar.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_008.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_008.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_008.png)

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_009.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_009.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_009.png)

**Para utilizar** : Seleccionar las líneas que desee alinear y pulsar **CTRL + ALT + A**

### Colorpicker

Nos muestra una paleta de colores sobre la marcha.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_010.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_010.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_010.png)

**Para utilizar : CTRL + SHIFT + C**

### GitGutter

Este es un plugin que nos dirá qué **lineas han cambiado desde la última modificación**. Un indicador aparecerá al lado de los números de línea.

### FTPSync

Si trabajamos con archivos alojados en un servidor FTP remoto, este plugin nos será de gran ayuda, hay que tener en cuenta que no soporta SFTP, para configurarlo iremos a **Preferences > Package Settings > FTPSync > Setup FTPSync**.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_011.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_011.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_011.png)

Si necesitamos un plugin SFTP podemos utilizar el siguiente, es gratuito para un único usuario https://wbond.net/sublime_packages/sftp.

## Personalización

### Instalar diccionario de Español.

Si programamos y escribimos en castellano, es posible que veamos constantemente **las palabras subrayadas**, y esto puede ser molesto, ya que Sublieme Text entiende que están mal escritas, para decirle a Sublime Text que diccionario usar, primero tendremos que descargarlo.

Podemos descargar el diccionario desde aquí: https://github.com/titoBouzout/Dictionaries

Nos harán falta los siguientes:

- Spanish.aff
- Spanish.dic
- Spanish.txt

Una vez descargados los archivos, accedemos a la carpeta `Packages` dentro de la carpeta de configuración de nuestro editor Sublime text, esta carpeta de encuentra en:

```powershell
C:\Users\nombre_usuario\AppData\Roaming\Sublime Text 3\Packages
```

Dentro de este directorio, deberemos de crear una carpeta llamada `Language - Spanish` y copiamos los archivos descargados dentro de él.

Después, para activarlo, bastará con ir a **View > dictionary > Language – Spanish > Spanish** y activarlo, y una vez activado le indicaremos a Sublime Text que a partir de ahora nuestro corrector ortográfico sea el Español en **View > Spell check** o pulsando la tecla **F6**.

### Desactivar PEP8 en Anaconda

PEP8 describe la **Guía de Estilo de Código Python**, es decir, cómo debemos escribir código Python de manera consistente y elegante.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_012.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_012.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_012.png)

Por defecto viene activado con Anaconda y puede ser molesto los constantes avisos si hemos descargado código de la red, si no queremos cumplir a raja tabla con este estilo lo podemos desactivar modificando el archivo Anaconda.sublime-settings en **Preferences – Package Settings – Anaconda – Settings Default**, modificando la siguiente linea a false.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_013.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_013.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_013.png)

### Combinación de teclas más usadas

Es muy aconsejable **pasar un tiempo aprendiendo los atajos de teclado** más usados, ya que nos ahorraran mucho tiempo cuando estemos programando. Los accesos directos que más utilizo son los siguientes:

- CTRL + K : Borra la línea actual.
- CTRL + X : Corta la línea actual.
- CTRL + SHIFT + ARRIBA : Mueve el texto resaltado arriba.
- Ctrl + Shift + ABAJO : Mueve el texto resaltado abajo.
- CTRL + W : Cierra la pestaña actual.
- CTRL + KK : Borra todo desde el cursor hasta el final de la línea.
- CTRL + F : Buscar.
- CTRL + H : Buscar y reemplazar.
- CTRL + KU : Convertir el texto seleccionado a mayúsculas.
- CTRL + KL : Convertir el texto seleccionado a minúsculas.
- CTRL + KB : Alternar la barra lateral.
- CTRL + [ : Indentar la línea actual.
- CTRL + ] : Sangrar la línea actual.
- CTRL + / : Comentar / Descomentar la línea o la selección actual.
- ALT + SHIFT + [NÚMERO] : Divide la pantalla en X columnas (máximo 4)
- CTRL + 0 : Centra la barra lateral.
- CTRL + 1-4 : Centra la columna 1-4.
- CTRL + SHIFT + 1-4 : Mueve el archivo a la columna 1-4.

Lista completa de atajos para Sublime Text:

http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_win.html

## Compilar y mostrar en la consola

Una vez hemos terminado nuestro programa, es hora de probarlo, Sublime Text cuenta con un atajo de teclado **CTRL + B para compilar y ejecutar nuestro código**, el problema, es que puede que no lo ejecute correctamente si no lo hemos configurado bien.

Vamos a configurar Sublime para que cuando pulsemos el atajo de teclado, nos abra una ventana de consola de comandos y ejecute el código automáticamente, nos vamos a **Tools -> Build System -> New Build System** y pegamos el siguiente código:

```powershell
{
    "cmd": ["start", "cmd", "/k", "c:/python27/python.exe", "$file"],
    "selector": "source.python",
    "shell": true,
    "working_dir": "$file_dir"
}
```

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_014.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_014.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_014.png)

Guardamos el archivo y seleccionamos nuestro ejecutable de Python en **Tools -> Build System**

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_015.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_015.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_015.png)

El código anterior es válido para ejecutar Python 2.7, si os fijáis, tengo uno para Python 2.7 y otro para Python 3, para crear el de Python 3, únicamente cambiamos la ruta del ejecutable a la hora de crear el archivo.

Ahora al pulsar **CTRL + B** veremos como se ejecuta nuestro código en la consola de comandos.

[![/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_016.png](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_016.png)](/images/preparando-sublime-text-3-programar-python/preparando-sublime-text-3-programar-python_016.png)

Esto es todo, espero os sirva de ayuda.

Saludos!!