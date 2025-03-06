+++
date = "2016-09-11"
title = "Hacking Cámaras IP - Montándome mi propio Gran Hermano"
author = "Javier Olmedo"
toc = false
+++

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_banner.jpg](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_banner.jpg)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_banner.jpg)

Para los que no os hayáis enterado, la **edición 17 de Gran Hermano está a punto de comenzar**.

## ¿ Y si me monto mi propio Gran Hermano?

Pues si, esa fue la pregunta que mi hice, y si **tu también quieres montarte el tuyo**, te invito a que leas este tutorial sobre **Hacking de cámaras IP**.

En esta entrada, os mostraré como podemos buscar Camarás IP accesibles desde Internet y cómo acceder a ellas.

## Lo primero, buscar las cámaras.

Buscar cámaras IP accesibles desde Internet **resulta una tarea bastante sencilla** si contamos con el llamado **«buscador de los hacker»**, [Shodan](https://www.shodan.io/) es un motor de búsqueda parecido a Google, con la diferencia que, en vez de indexar el contenido, **almacena los dispositivos conectados a Internet** (routers, servidores, cámaras, etc).

Por ejemplo, **voy a buscar cámaras IP, de la marca Honeywell y que se encuentren en Madrid**, para ellos accedemos a Shodan y buscamos con el siguiente filtro:

```bash
Honeywell city:Madrid
```

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_001.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_001.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_001.png)

Para utilizar filtros, **es necesario estar registrado en Shodan**, así que, si todavía no estás registrado, creo que merece la pena que lo hagas.

Bien, he encontrado cuatro IPs que tiene cámaras de la marca Honeywell en Madrid, puede parecer que hemos encontrado pocas cámaras, pero debemos de tener en cuenta que Shodan es un buscador, y **es posible que no haya registrado el 100% de las cámaras accesibles**, por lo tanto, voy a **usar Nmap para rastrear dentro del rango** de IPs (es muy probable que haya más).

[Nmap](https://nmap.org/) **es un escáner de puertos**, y permite rastrear IPs que dispongan de servicios a la escucha filtrando por puerto (fijémonos en la captura anterior que las cámaras **Honeywell usan los puertos 3310, 2181 y 2000)**, por lo tanto, supongamos que hemos obtenido la IP 172.26.100.50 (no intentéis probar con esta) de Shodan, la cuál tiene la cámara. Lo siguiente es buscar si dentro de su rango hay más cámaras, buscaremos en el rango 172.26.0.0 al 172.26.255.255, y filtraremos por los puertos 3310, 2181 y 2000, esto lo podemos hacer con el siguiente comando:

```bash
nmap 172.26.0.0/16 -p 3310, 2181, 2000
```

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_002.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_002.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_002.png)

Bueno, ya tenemos unas cuantas IPs que posiblemente podamos acceder a ellas, **a las cámaras IPs se pueden acceder de 2 maneras**:

## Accediendo con las credenciales por defecto.

**Aunque parezca increíble que aún haya gente que no cambie las credenciales por defecto** de los dispositivos, existe muchísimas cámaras IPs que las tienen, y no solo cámaras IPs, **sino cualquier tipo de dispositivo**, podemos ver un listado de las contraseñas por defecto de las cámaras IPs [aqui](http://www.netviewcctv.co.uk/blog/ipcampassword/):

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_003.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_003.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_003.png)

## Usando un exploit.

**Un exploit es un código o parte de un programa que intenta beneficiarse de la vulnerabilidad de un sistema para acceder a él**, podemos ver un exploit que ha sido publicado a fecha 22 de Agosto de 2016 para las cámaras Honeywell [aquí](https://www.exploit-db.com/exploits/40283).

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_004.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_004.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_004.png)

Ya sabemos las 2 maneras de entrar, **voy a intentar entrar a una IP con las contraseñas por defecto**, abro Mozilla Firefox, pongo la IP y relleno el formulario.

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_005.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_005.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_005.png)

Ya estoy dentro, que fácil fue acceder a las cámaras de una oficina.

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_006.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_006.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_006.png)

Ahora para ver las cámaras, debemos de **hacer «click» en un cuadrado del centro**, y después al **icono pequeño de la cámara del panel izquierdo**.

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_007.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_007.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_007.png)

Voy a ir añadiendo cámaras, **también podemos moverlas** (cuando no haya gente como en mi caso, ya que es domingo por la mañana y parece ser unas oficinas) desde el panel derecho.

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_008.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_008.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_008.png)

## Preservando el acceso.

En este momento, tenemos acceso a las cámaras, pero **es posible que el administrador cambie las contraseñas por defecto** y ya no tengamos acceso a ellas. Una vez dentro, podemos **crear un segundo (o tercero y cuarto) usuario** con permisos de administrador, yo he usado el user root para crearlo, pero para pasar mas desapercibido, podríamos poner como nombre de usuario gerente, responsable, etc.

[![/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_009.png](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_009.png)](/images/hacking-camaras-ip-montandome-propio-gran-hermano/hacking-camaras-ip-montandome-propio-gran-hermano_009.png)

Un Saludo!!!

Y ya sabéis, **si no queréis formar parte de un Gran Hermano, cambiad las credenciales por defecto y actualizar el firmware** de vuestras cámaras.

Happy Hacking!!!