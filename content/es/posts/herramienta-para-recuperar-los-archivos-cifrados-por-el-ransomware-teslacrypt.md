+++
date = "2015-05-02"
title = "Herramienta para recuperar los archivos cifrados por el ransomware TeslaCrypt"
author = "Javier Olmedo"
+++

Cifrar los archivos de la víctima y luego pedir bitcoins para descifrarlos es un negocio muy lucrativo: **las campañas de ramsonware se están convirtiendo en una amenaza cada vez mayor**. Después de la caída de CryptoLocker surgió [Cryptowall](https://es.wikipedia.org/wiki/CryptoLocker), con técnicas anti-depuración avanzadas, y después **numerosas variantes que se incluyen en campañas dirigidas cada vez más numerosas**. 
 
Una de las últimas variantes se llama **TeslaCrypt y parece ser un derivado del ransomware CryptoLocker original**. Este ransomware está **dirigido específicamente a gamers** y, aunque dice estar usando RSA-2048 asimétrico para cifrar archivos, **realmente está usando AES simétrico**, lo que ha permitido a [Talos Group](http://blogs.cisco.com/author/talos) (Talos Security Intelligence & Research Group) desarrollar una herramienta que descifra los archivos…

[![/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_001.png](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_001.png)](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_001.png)

Primero se analizaron dos muestras con fecha de marzo y abril de 2015. Ambas muestras implementaban los siguientes algoritmos de hash:

- SHA1
- SHA256
- RIPEMD160
- BASE58
- BASE64

## Vector de infección y función de instalación

Este ransomware se distribuye generalmente como un archivo adjunto de correo electrónico o a través de sitios web que redirigen a la víctima al [exploit kit Angler](http://community.websense.com/blogs/securitylabs/archive/2015/02/05/angler-exploit-kit-operating-at-the-cutting-edge.aspx) (también se ha identificado en otros exploits kits como Nuclear y Sweet Orange). Luego **la mayoría de las muestras de TeslaCrypt utilizan técnicas COM+ de evasión de sandbox**. Por ejemplo, el dropper que analizaron utilizaba sencillo código de detección que verifica si la interfaz COM «URLReader2» se ha instalado correctamente en el filtro DirectShow:

[![/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_002.png](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_002.png)](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_002.png)

Si se pasa la verificación, el verdadero dropper se extrae y se ejecuta utilizando un método bien conocido que hace uso de las funciones de la APIZwMap(Unmap)ViewOfSection para extraer la imagen original de memoria del PE y re-mapear otra. El último ejecutable desempaquetado localiza directorios específicos de Windows, como el directorio de datos de aplicación, y construye los archivos de soporte como **el archivo «key.dat» y archivos para almacenar instrucciones de descifrado**.

El ejecutable también ajusta sus propios privilegios (añade «SetDebugPrivilege») y se copia utilizando un nombre de archivo aleatorio en el directorio de datos de aplicación del usuario. Un nuevo proceso se genera entonces y la ejecución se transfiere a la misma. Después se elimina el archivo original del dropper y se crean cinco hilos que realizan lo siguiente:

- **elimina todas las Volume Shadow Copies** mediante la ejecución del comando “vssadmin.exe delete shadows /all /quiet”
- abre el archivo «key.dat» y recupera las claves de cifrado. Si no existe el archivo «key.dat», crea las claves y las almacena de forma cifrada.
- envía la nueva clave de cifrado principal al servidor C&C a través de una petición POST. El ejemplo analizado contenía las siguientes URLs:

    - 7tno4hib47vlep5o.63ghdye17.com
    - 7tno4hib47vlep5o.79fhdm16.com
    - 7tno4hib47vlep5o.tor2web.blutmagie.de
    - 7tno4hib47vlep5o.tor2web.fi

- implementa protección anti-tampering: cada 200 mili segundos, TeslaCrypt enumera todos los procesos en ejecución y si encuentra un proceso con un nombre de archivo que contenga cualquiera de las palabras de abajo, el proceso se termina usando la función API de Windows TerminateProcess:

    - taskmgr
    - procexp
    - regedit
    - msconfig
    - cmd.exe

## Cifrado de archivos – introducción

Como hemos dicho, después de la rutina de inicialización y la supresión de las instantáneas de volumen (Volume Shadows), la muestra crea el archivo «key.dat» donde almacena todas las claves de cifrado. El dropper de marzo 2015 calculaba al menos **dos claves principales diferentes: una clave de pago y una clave de cifrado principal**. El otro dropper implementaba el concepto de una clave adicional conocida como la **«clave de recuperación«**.

«GetAndHashOsData» es la función responsable de la creación del buffer base para la generación de todas las claves. Al principio adquiere la siguiente información:

- estadísticas de la red LAN del PC, utilizando la función API NetStatisticsGet
- 64 bytes aleatorios generados por las cripto-funciones de Windows
- todos los descriptores del heap de su propio proceso
- todos los descriptores de procesos activos y los hilos descriptores de cada proceso
- todos los módulos cargados en cada proceso
- información de la memoria física del PC

Una vez que se adquieren los datos, se genera una gran matriz de valores SHA1, una por cada 20 bytes de datos adquiridos. Al final se calcula y almacena un valor SHA1 global para toda la matriz, en un símbolo llamado «g_lpGlobalOsDataSha1».

Con esos dos ítems, la rutina de «FillBuffWithEncryptedOsData» es capaz de llenar de una manera pseudo-aleatoria un buffer genérico con los datos calculados. Una clave maestra y una clave de pago se generan utilizando esta función (cada clave es de 32 bytes), su SHA256 se calcula y, finalmente, un algoritmo personalizado se utiliza para desplazar a la izquierda y a la derecha las dos claves. Los dos valores desplazados SHA256 se almacenan en el archivo «key.dat».

## El fichero de claves

La rutina «OpenKeyFileAndWrite» intenta abrir el archivo «key.dat» ubicado en el directorio de datos de aplicación del usuario. Si no existe, se generan las dos claves maestras (tres en el caso de los droppers más recientes) junto con las otras claves y los almacena en el archivo.

Aquí hay un pequeño esquema de la disposición del archivo «key.dat»:

[![/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_003.png](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_003.png)](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_003.png)

La última versión del dropper crea un archivo «RECOVERY_KEY.TXT» dentro del directorio de documentos del usuario. Lo hace para lograr un objetivo particular: si el equipo de la víctima está desconectado o si un cortafuegos bloquea la comunicación con el servidor C&C, el dropper procederá a la destrucción de la clave maestra dentro del archivo «key.dat», después de que se haya completado el cifrado de todos los archivos.

**Para recuperar los archivos cifrados, el usuario tendría que conectarse al sitio web TOR del atacante y proporcionar la clave de recuperación**. Los atacantes utilizan un **algoritmo personalizado** para recuperar la clave maestra a partir de la clave de recuperación:

[![/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_004.png](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_004.png)](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_004.png)

El archivo de recuperación de claves contiene 3 piezas de información en un formato legible, separadas por un carácter de retorno de carro:

- La dirección Bitcoin
- El identificador de clave de pago (32 dígitos hexadecimales)
- La clave de recuperación (64 dígitos hexadecimales)

## El algoritmo de cifrado de archivos

El cifrado de archivos se realiza en un thread dedicado. El código para el hilo de cifrado toma la clave maestra desplazada, calcula el hash SHA256 y comienza a enumerar todos los archivos del PC de la víctima (filtrado por tipo de extensión, TeslaCrypt soporta más de 170 extensiones de archivos diferentes).

«EncryptFile» es la función que gestiona todo el proceso de cifrado de archivos. Esta función:

– genera un 16-bytes vector de inicialización para AES, usando la función API GetAndHashOsData
– lee el archivo de destino
– inicializa el algoritmo de cifrado AES a través de la creación de la estructura de datos de contexto AES
– finalmente cifra el contenido del archivo usando un algoritmo AES de 256 bits CBC implementado en la función «EncryptWithCbcAes».

Cuando el proceso se haya completado, se crea el nuevo archivo cifrado. El nuevo archivo contiene una pequeña cabecera (compuesto del vector de inicialización AES en sus primeros 16 bytes seguido por el tamaño del archivo original en los próximos 4 bytes), y luego los bytes cifrados reales.

[![/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_005.png](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_005.png)](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_005.png)

La ventana emergente muestra información engañosa: el **método de cifrado es AES simétrico, y no un RSA-2048 asimétrico** según lo declarado por TeslaCrypt en la imagen anterior.

Como prueba de que TeslaCrypt está realmente utilizando AES simétrico y no RSA asimétrica, Talos publica una utilidad de descifrado capaz de descifrar todos los archivos cifrados por este ransomware (siempre que se tenga la clave maestra).

## La herramienta de descifrado de TeslaCrypt de Talos

La utilidad de descifrado de Cisco es una utilidad en línea de comandos. **Se necesita el archivo «key.dat» para recuperar correctamente la clave maestra utilizada para el cifrado de archivos**. Antes de que comience la ejecución, busca «key.dat» en su ubicación original (directorio de datos de aplicación del usuario), o en el directorio actual. Si no es capaz de encontrar y analizar correctamente el archivo «key.dat», se devuelve un error y sale.

[![/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_006.png](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_006.png)](/images/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt/herramienta-para-recuperar-los-archivos-cifrados-por-el-ransomware-teslacrypt_006.png)

Para utilizar esta herramienta, sólo tienes que copiar el fichero «key.dat» en el directorio de la herramienta y luego especificar el archivo cifrado o un directorio que contiene archivos cifrados. ¡Eso es todo! Los archivos serán descifrados y volverán a su contenido original.

Aquí está la lista de opciones de la línea de comandos:

- /help – Muestra el mensaje de ayuda
- /key – especifica manualmente la clave maestra para el descifrado (32 bytes/64 dígitos)
- /keyfile – Especifica la ruta del archivo «key.dat» que se utiliza para recuperar la clave maestra
- /file – Descifra un archivo cifrado
- /dir – Descifra todos los archivos «.ecc» en el directorio de destino y sus subdirectorios
- /scanEntirePc – descifra archivos «.ecc» en todo el equipo
- /KeepOriginal – Guarda el archivo original(es) en el proceso de cifrado
- /deleteTeslaCrypt – Automáticamente mata y elimina el dropper TeslaCrypt (si se encuentra activo en el sistema de destino)

Eso sí, haz un backup de tus archivos cifrados antes de utilizar esta utilidad, se ofrece siempre sin garantías.

Aquí tenéis los enlaces de la herramienta:

### Binario Windows:

http://labs.snort.org/files/TeslaDecrypt_exe.zip

ZIP SHA256: 57ce1c16e920a9e19ea1c14f9c323857c9a40751619d3959684c7e17956d66c6

### Código fuente del binario de Windows:

https://labs.snort.org/files/TeslaDecrypt_cpp.zip

ZIP SHA256: 45908f0b3f8eb73bf820ded0a886842ac5c3e4c83068097806daad662046b1e0

### Script en Python:

https://labs.snort.org/files/TeslaDecrypt_python.zip

ZIP SHA256: ea58c2dd975ed42b5a30729ca7a8bc50b6edf5d8f251884cb3b3d3ceef32bd4e

Futuras mejoras

A la herramienta todavía le faltan algunas características. En particular, no han tenido tiempo para implementar el algoritmo necesario para recuperar la clave maestra de la clave de recuperación. Esto es importante porque en algunas versiones del dropper, la clave maestra se extrae del archivo «key.dat» tan pronto como se completa el cifrado de archivos. Así que habrá que estar pendiente por si publican actualizaciones.

pd. La utilidad de descifrado es una herramienta de prueba que no está soportada oficialmente y el usuario asume toda la responsabilidad derivada del uso de la misma.

Fuentes:
- Threat Spotlight: TeslaCrypt – Decrypt It Yourself
- New Utility Decrypts Data Lost to TeslaCrypt Ransomware

TRADUCIDO Y PUBLICADO EN HACKPLAYERS POR VICENTE MOTOS

http://www.hackplayers.com/2015/04/recuperar-los-archivos-cifrados-con-teslacrypt.html