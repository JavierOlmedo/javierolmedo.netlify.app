+++
date = "2019-05-13"
title = "Que es Smali y como parchear una aplicación Android"
author = "José María Arenas"
toc = false
+++

## ¿Qué es Smali?

Las aplicaciones Android contienen ficheros .dex (Dalvik EXecutable). Estos ficheros se pueden decompilar para obtener un código de bajo nivel llamado Dalvik Bytecode. Utilizando [smali/baksmali](https://github.com/JesusFreke/smali) (ensamblador/desensamblador) se puede obtener una representación en un lenguaje de bajo nivel con el que se puede trabajar más fácilmente, al cual llamaremos código Smali. Hoy en día, apktool ya incorpora smali/baksmali, por lo que será suficiente con desempaquetar el fichero .apk con apktool para obtener el código Smali.

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_001.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_001.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_001.png)

Ejemplo de código Smali

El código Smali recuerda al código del lenguaje ensamblador, pero en este caso se ven los nombres de las clases de Java y Android, así como los nombres de los métodos. Para obtener más información sobre la sintaxis de los métodos, clases, tipos primitivos y campos de clases, se puede consultar el siguiente enlace [https://github.com/JesusFreke/smali/wiki/TypesMethodsAndFields](https://github.com/JesusFreke/smali/wiki/TypesMethodsAndFields). A modo de guía rápida para consulta de los nombres de las operaciones propias de Smali, se puede consultar la [lista de opcodes de dalvik](http://pallergabor.uw.hu/androidblog/dalvik_opcodes.html).

Por tanto, y a modo de resumen, Smali es una representación en lenguaje de bajo nivel del código de bajo nivel Dalvik, el cual está contenido en los ficheros .dex que contienen todo el código de la aplicación (salvo el código de las librerías de código nativo, los ficheros .so), los cuales forman parte del fichero de aplicación .apk.

## ¿Cómo parchear una aplicación Android?

El flujo de trabajo general para parchear una aplicación Android es:

1. Desempaquetar el fichero .apk y obtener una representación en código Java
2. Identificar la parte o partes del código Smali a modificar
3. Realizar las modificaciones necesarias
4. Reempaquetar la aplicación y firmarla
5. Instalar el fichero .apk modificado
6. Probar las modificaciones

En este caso, vamos a resolver un crackme de Android sencillo mediante una modificación del código Smali. El enlace a la aplicación es [Crackme](http://deurus.info/archivos/mycrackmes/Android_Crackme03.zip).

El primer paso es desempaquetar la aplicación utilizando la herramienta apktool y obtener una representación **aproximada** en código Java (la conversión de Java a Dalvik supone una pérdida de información que imposibilita obtener un código Java directamente compilable a partir de su código Dalvik asociado). Para ello utilizaremos el siguiente comando:

```bash
apktool d Crackme03.apk && d2j-dex2jar Crackme03.apk
```

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_002.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_002.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_002.png)

Desempaquetado de la aplicación

El resultado será la creación de una carpeta con el mismo nombre del fichero apk que contendrá una serie de subcarpetas con el contenido de la aplicación, y entre ellas, una llamada smali que contendrá el código Smali.

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_003.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_003.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_003.png)

Estructura de carpetas creadas en el desempaquetado con apktool

Ademas, al mismo nivel que el fichero .apk, aparecera un nuevo fichero JAR llamado `nombrecrackme-dex2jar.jar` que contiene los ficheros .class de las distintas clases de la aplicación.

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_004.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_004.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_004.png)

Muestra de los archivos `.class` del `.jar` obtenido con `d2j-dex2jar`

Para obtener un código muy similar a java a partir de este fichero .jar, utilizaremos jd-gui (u otro decompilador). Jd-gui es una herramienta gráfica que muestra los archivos fuente de Java a partir de los archivos .class, Esto permitirá identificar más fácilmente el punto donde se debe hacer la modificación. El comando es:

```bash
jd-gui Crackme03-dex2jar.jar
```

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_005.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_005.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_005.png)

Ejemplo del código Java aproximado obtenido con jd-gui

En el caso de este crackme, se pide un nombre y un número de serie. En caso de que los datos sean incorrectos, la aplicación mostrará el mensaje “Bad boy”:

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_006.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_006.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_006.png)

Mensaje que muestra la aplicación cuando los datos introducidos no son los correctos

La modificación que se va a realizar consiste en hacer que la aplicación acepte cualquier dato que se introduzca y devuelva el mensaje asociado a los datos correctos. En el caso de esta aplicación, el código es muy corto y se puede buscar la cadena mostrada directamente en el código, pero se va a realizar la búsqueda de la cadena en la jerarquia de carpetas que creó apktool con el siguiente comando:

```bash
grep -iR "Bad boy" Crackme03/
```

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_007.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_007.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_007.png)

Búsqueda de la cadena en los archivos del desempaquetado con apktool

Ahora sabemos que la cadena está en el archivo `Crackme03/smali/com/example/helloandroid/HelloAndroid$2.smali`, ese archivo es equivalente a `com.example.helloandroid/HelloAndroid.class` en jd-gui. El método donde aparece la cadena es el siguiente:

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_008.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_008.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_008.png)

Fragmento de código Java aproximado encargado de la comprobación de los datos introducidos

El código Smali asociado a este método tiene 268 lineas. Dicho código realiza las siguientes operaciones:

1. Se toman los valores de los dos campos introducidos por el usuario
2. Se guarda la longitud del campo nombre
3. Se comprueba que la longitud sea 4 o más
4. Se inicia un bucle que realiza una serie de cálculos para calcular el valor correcto
    1. En la parte final de este bucle, se hace la comprobación del valor correcto con el valor introducido

Por tanto, para que la aplicación acepte cualquier valor de los campos, hay que modificar la comprobación de longitud y la comprobación de que el valor introducido es igual al correcto.

La comprobación de longitud en el código Smali se muestra en la siguiente imagen:

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_009.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_009.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_009.png)

Comprobación de la longitud del nombre introducido

Las dos variables de la comprobación son v0 y v1. Si seguimos v0 desde en orden inverso al de ejecución, vemos que se copia desde v11 en la linea 45, se le asocia un nombre en la linea 25, y recibe en la linea 23 el resultado de la operación length de la linea 22. Siguiendo de la misma forma la variable v1, se copia desde v22 en la linea 46 y v22 se inicializa a 4 (0x4 en decimal) en la linea 44. Si la comprobación es correcta, se salta la ejecución a la etiqueta :cond_0 que se corresponde con la siguiente parte de la ejecución en caso correcto.

El código Smali asociado a la comprobación de que el serial introducido es igual al calculado se muestra en la siguiente imagen:

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_010.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_010.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_010.png)

Comprobación del serial introducido

En este caso, la variable que interviene en la comprobación es v22, siguiéndola en sentido contrario a la ejecución se ve que recibe en la linea 164 el resultado de la operación de la linea 163, la cual compara si son iguales las variables v14 y v15. v14 recibe en la linea 160 el resultado de la operación de la linea 159 que es la ultima de la serie de cálculos del valor correcto. v15 es el valor introducido por el usuario y se puede ver en la captura de la comprobación anterior.

Llegados a este punto, los cambios a realizar serían cambiar la comprobación de longitud a un salto incondicional a la etiqueta :cond_0 y comentar la comprobación del serial para que entre en el fragmento de código correcto. Dichas modificaciones serían las siguientes:
    
[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_011.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_011.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_011.png)
    
Original de la comprobación de longitud
    
[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_012.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_012.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_012.png)
    
Modificado de la comprobación de longitud
    
[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_013.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_013.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_013.png)
    
Original de la comprobación del serial
    
[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_014.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_014.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_014.png)
    
Modificado de la comprobación del serial

Sin embargo estos cambios, concretamente el de la longitud del dato introducido, hace que la aplicación sufra errores (se puede hacer el otro cambio solo e introducir 4 caracteres cualesquiera en el campo nombre), por tanto, vamos a optar por un cambio diferente, consiste en realizar un salto hasta la zona del código correcto, en vez de hacer la comprobación de la longitud. Esto evita realizar todos los cálculos y lleva la ejecución al punto que queremos. Para realizar estos cambios, se comentará la línea 47 y se añadirá un salto incondicional hacia la etiqueta :cond_3 (la cual crearemos a continuación) entre la línea 47 y 48 (y será la nueva línea 48):

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_015.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_015.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_015.png)

Primera de las dos modificaciones definitivas

Después añadimos la etiqueta :cond_3 entre las lineas 166 y 167 (y será la nueva línea 167):

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_016.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_016.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_016.png)

Segunda de las modificaciones definitivas

Una vez hechos estos cambios, lo último que queda es re empaquetar la aplicación, firmarla e instalarla. Para re empaquetar la aplicación usaremos el siguiente comando:

```bash
apktool b Crackme03
```

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_017.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_017.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_017.png)

Reempaquetado de la aplicación utilizando apktool

Este comando crea el archivo apk de la aplicación modificada y lo guarda en la ruta `Crackme03/dist/Crackme03.apk`. Para firmar la aplicación necesitaremos un almacén de claves o keystore, el cual podemos crearlo con el siguiente comando:

```bash
keytool -genkey -v -keystore keystore.keystore -alias alias -keyalg RSA -keysize 2048 -validity 3650
```

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_018.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_018.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_018.png)

Creación del almacén de claves con keytool

Finalmente se firma la aplicación con el siguiente comando:

```bash
 jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore keystore.keystore Crackme03/dist/Crackme03.apk alias
```

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_019.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_019.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_019.png)

Firma de la aplicación modificada y reempaquetada

En este momento se debe borrar la aplicación original del dispositivo o emulador ya que al intentar instalarla reemplazándola, aparecerá un error por que los certificados no coinciden. Una vez eliminada la aplicación, instalaremos la aplicación en el dispositivo:

```bash
adb install Crackme03/dist/Crackme03.apk
```

Ahora, sin introducir ningún dato, la aplicación nos devuelve el mensaje correcto:

[![/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_020.png](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_020.png)](/images/que-es-smali-y-como-parchear-una-aplicacion-android/que-es-smali-y-como-parchear-una-aplicacion-android_020.png)

Hasta aquí la entrada, espero que os haya servido de ayuda y finalmente agradecer enormemente a mi compañero Javier Olmedo por la oportunidad de escribir en su blog!.

Un saludo y hasta la próxima.