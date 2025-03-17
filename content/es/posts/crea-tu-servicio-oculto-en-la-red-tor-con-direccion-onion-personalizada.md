+++
date = "2018-04-19"
title = "Crea tu servicio oculto en la red TOR con dirección onion personalizada"
author = "Javier Olmedo"
toc = false
+++

Cuando hablamos de la 🖧 **red TOR**, solemos asociarla a todo tipo de actos delictivos (tráfico de drogas, cibercrimen organizado, venta de armas u otras actividades ilícitas), pero, al margen de esta mala imagen de la red TOR, es una **excelente herramienta** que puede ayudarnos a mantener nuestra **🕵️‍♂️ privacidad y anonimato** en la red, sobre todo si queremos ofrecer algún servicio sin revelar nuestra identidad.

En esta entrada veremos como **crear un servicio oculto en nuestro equipo y hacerlo accesible a la red TOR mediante una dirección onion personalizada**, los pasos que seguiremos serán los siguientes:

## 🖥 1. Instalación de un servidor NGINX

1.1 Actualizar los repositorios

```bash
apt-get update && apt-get upgrade
```

1.2 Parar el servicio de apache (en el caso que lo tengamos arrancado)

```bash
service apache2 stop
```

1.3 📥Instalar Nginx

```bash
apt-get install nginx
```

1.4 Arrancar el servicio Nginx

```bash
service nginx start
```

1.5 Comprobar el estado del servicio Nginx

```bash
service nginx status
```

1.6 Crear un archivo 📄 `index.html` dentro del directorio 📁 `/var/www/html`

Puedes usar este código de ejemplo:

```html
<html>
<body>
Este es el servicio oculto de Hackpuntes
</body>
</html>
```

1.7 Acceder a localhost para comprobar que funciona

[http://localhost](http://localhost/)

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_001.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_001.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_001.png)

1.8 ⚠ En el caso de tener UFW firewall, añadir excepción para Nginx

```bash
ufw allow 'Nginx HTTP'
```

1.9 Comprobar estado del Firewall UFW

```bash
ufw status
```

1.10 🔒 Para mayor seguridad, ocultaremos los banner del servidor añadiendo la línea `server_tokens off;` en el archivo `/etc/nginx/nginx.conf` (Intentaré hacer otra entrada sobre como securizar el servidor 😉)

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_002.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_002.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_002.png)

## 🕵️‍♂️ 2. Crear un servicio oculto

2.1 📥 Instalar TOR

```bash
apt-get install tor
```

2.2 Comprobar la ubicación de la instalación de TOR (normalmente será 📁 `/etc/tor/`)

```bash
whereis tor
```

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_003.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_003.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_003.png)

2.3 Nos movemos al directorio de TOR con el comando `cd`, listamos archivos y comprobamos que tenemos el archivo 📄 `torrc`

```bash
cd /etc/tor && ls
```

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_004.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_004.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_004.png)

2.4 Abrimos el archivo 📄 `torrc` para editarlo y buscamos la parte del archivo dónde aparece

```txt
############### This section is just for location-hidden services ###
```

2.5 Descomentamos las siguiente líneas (72 y 73) quitando la `#`

```txt
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:80
```

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_005.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_005.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_005.png)

## ⚙ 3. Iniciar y comprobar el servicio TOR  

3.1 Iniciar el servicio TOR

```bash
tor
```

3.2 La primera vez que iniciamos el servicio TOR, se generará automáticamente una dirección `.onion` y se almacenará en la ruta 📁 `/var/lib/tor/hidden_service`, accedemos a ella y listamos los archivos.

```bash
cd /var/lib/tor/hidden_service && ls
```

3.3 Dentro de este directorio tenemos 2 archivos importantes, el archivo 🖥 `hostname` (que contine la dirección `.onion` generada automáticamente) y el archivo 🔑 `private_key` (el cual modificaremos más adelante)

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_006.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_006.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_006.png)

3.4 Copiamos la dirección `.onion` que contiene el archivo 📄 `hostname`

3.5 Desde otro equipo y accediendo a TOR (📦[**descargar su bundle**](https://www.torproject.org/download/download-easy.html.en)) accedemos a la dirección que anteriormente hemos copiado.

3.6 Si todo ha ido bien, veremos nuestra página accesible desde TOR

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_007.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_007.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_007.png)

## 🔗 4. Generar una dirección para TOR (onion) personalizada

4.1 Para crear una dirección .onion personalizada, necesitaremos 🔑 **generar una clave privada que coincida con el nombre de host a personalizar**, dado que las direcciones de TOR son parcialmente hashes, necesitaremos **usar fuerza bruta** para generar la dirección que deseemos.

### ¿Cuanto tiempo tardaría en generar mi dirección onion personalizada?

Cuantos más caracteres consecutivos de una palabra o frase queramos personalizar, más tiempo tardaremos en generarla, ya que cuanto más larga es la secuencia, el tiempo se alargará **de forma exponencial.** Aproximadamente, y utilizando un **procesador de 1,5Ghz** se tardaría:

|Caracteres|Tiempo necesario para generarlo|
|---|---|
|1|menos de 1 segundo|
|2|menos de 1 segundo|
|3|menos de 1 segundo|
|4|2 segundos|
|5|1 minuto|
|6|30 minutos|
|7|1 día|
|8|25 días|
|9|2,5 años|
|10|40 años|
|11|640 años|
|12|10 milenios|
|13|160 milenios|
|14|2,6 millones de años|

Es decir, si quisiera personalizar por completo los **14 caracteres** de nuestra dirección .onion tardaríamos **2,6 millones de años** aproximadamente en generarlo, para esta prueba vamos a generar uno de 6 caracteres (30 minutos aprox.), de manera que nos generará algo parecido a `hackptxxxxxxxx.onion`

4.2 Para generar esta dirección, haremos uso de la herramienta [Eschalot](https://github.com/ReclaimYourPrivacy/eschalot) y lo haremos de la siguiente manera:

📥 Clonamos el repositorio a nuestro equipo

```bash
git clone https://github.com/ReclaimYourPrivacy/eschalot
```

Accedemos al directorio

```bash
cd eschalot/
```

Instalamos la herramienta

```bash
make
```

Generamos la dirección personalizada, recordemos que usaré 6 dígitos `(hackpt)` lo que nos dará una dirección `hackptxxxxxxxx.onion`

```bash
./eschalot -vct2 -p hackpt
```

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_008.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_008.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_008.png)

Con este comando, indicamos a eschalot que queremos usar **2 núcleos** del procesador, con el **prefijo hackpt** en lo primeros caracteres y que una vez encuentre uno válido, siga buscando más.

Una vez hayamos encontrado una clave privada válida, paramos la herramienta con `Ctrl + C` y copiamos la clave

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_009.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_009.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_009.png)

## 🔑 5. Agregar la dirección personalizada al servicio oculto

5.1 Asegurarnos que el servicio TOR está parado, esto lo podemos hacer cerrando la terminal donde ejecutamos `tor` con `Ctrl + C`

5.2 Eliminar el archivo 📄 `hostname` del directorio 📁 `/var/lib/tor/hidden_service`

```bash
rm /var/lib/tor/hidden_service/hostname
```

5.3 Agregar la clave que hemos generado con **Eschalot** en el archivo 🔑 `private_key` entre las etiquetas `«—– BEGIN RSA PRIVATE KEY —–«` y `«—– END RSA PRIVATE KEY —–«` quedando de la siguiente manera

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_010.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_010.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_010.png)

5.4 Volvemos a iniciar TOR

```bash
tor
```

5.5 Verificar que se ha vuelto a generar el archivo 📄 `hostname` en el directorio 📁 `/var/lib/tor/hidden_service` y que su contenido es la direccion `onion` generada (en mi caso `hackptzefraj3gce.onion`)

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_011.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_011.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_011.png)

5.6 Comprobar que desde otro equipo conectado a la red TOR accedemos al servicio oculto con la nueva dirección.

[![/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_012.png](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_012.png)](/images/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada/crea-tu-servicio-oculto-en-la-red-tor-con-direccion-onion-personalizada_012.png)

⚠ Si no puedes acceder, acuérdate de tener el **servicio de Nginx y tor** levantado en el servidor.  

Fin de la entrada!!

Hasta la próxima👋

P.D Con mucho cariño para mi compañero [@v3sc4p4m](https://twitter.com/v3sc4p4m) por prestarme sus libros sobre la Deep Web 😉