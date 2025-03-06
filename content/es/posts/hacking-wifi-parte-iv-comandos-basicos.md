+++
date = "2015-04-05"
title = "Hacking Wifi - Parte IV - Comandos Básicos"
author = "Javier Olmedo"
toc = false
+++

En esta parte de entradas sobre hacking wifi, vamos a empezar a trabajar con nuestra tarjeta de red inalámbrica y a conocer los **comandos básicos**.

Uno de los comandos básicos que siempre debemos de utilizar para comprobar que nuestra tarjeta de red **ha sido aceptada por el sistema operativo** es:

```bash
ifconfig
```

Este comando permite configurar numerosos parámetros de las interfaces de red, como la direccion IP o la máscara de red. **Si se llama sin argumentos** (como la acabo de llamar yo) **muestra la configuración vigente** de las interfaces de red **activas** (con esto confirmamos que la tarjeta de red inalámbrica ha sido aceptada por el sistema y no es necesario drivers para ponerla en funcionamiento), con detalles como la direccion MAC o el tráfico que ha circulado por las mismas hasta el momento.

En la siguiente captura podemos **observar 3 interfaces de red** en mi caso, **eth0** (conexión Ethernet), **lo** (direccion loopback o localhost 127.0.0.1, que suele utilizarse para realizar pruebas de red sobre nuestro propio ordenador) y **wlan0** (esta es nuestra tarjeta inalámbrica que utilizaremos para auditar las redes).

[![/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_001.png](/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_001.png)](/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_001.png)

## Parámetros para el comando ifconfig

Al comando ifconfig **podemos añadirle parámetros** para configurar la interfaz de la siguiente manera:

```bash
ifconfig [interfaz] [parámetro]
```

Por **ejemplo para inhabilitar** la tarjeta inalámbrica lo haría de la siguiente manera:

```bash
ifconfig wlan0 down
```

Los **paramentros mas importantes** para **ifconfig** son:

- **up** –> Habilita la interfaz.
- **down** –> Deshabilita la interfaz.
- **netmask** –> Asigna un mascara de subred (seguidamente el valor).
- **broadcast** –> Asigna dirección de difusión (seguidamente el valor).
- **mtu** –> Fija la unidad máxima de transferencia, es decir el maximo capaz de manejar la interfaz en una única transacción (seguidamente el valor en bytes).
- **arp** –> Activa el uso de ARP, el Protocolo de Resolución de Direcciones, para detectar la dirección física de las máquinas conectadas a la red (este protocolo lo veremos mas adelante para realizar ataques una vez hemos tenido acceso a la red).
- **-arp** –> Desactiva el uso de ARP.
- **promisc** –> Activa el modo promiscuo, esto hace que la interfaz reciba todos los paquetes, independientemente de si eran para ella o no, por ejemplo, si un ordenador A envia un paquete a B por difusión, solamente debería de aceptarlo B, pero si nosotros somos C y tenemos activado el modo promiscuo, podremos capturar lo que ha mandado A a B, esto permite utilizar filtros de paquetes (lo que llamamos «fisgoneo») y es una buena técnica para saber que está pasando en una red.
- **-promisc** –> Desactiva el modo promiscuo.
- **allmulti** –> Activa la recepción y el envio de paquetes a múltiples destinos.
- **-allmulti** –> Desactiva la anterior opción.

Otro de los comandos que tendremos muy en cuenta y **absolutamente necesario** para empezar a capturar paquetes, es el comando **airmon-ng** seguido de sus parámetros, el cual nos permite **activar o desactivar el modo monitor** (no confundir con modo promiscuo).

## ¿Que es el modo monitor?

**Algunas tarjetas wireless** (dependiendo del modelo y el chipset que utilicen) tienen un modo que se conoce como monitor, este modo permite la captura de los paquetes de una red wireless (que van por el aire en ondas de radio) **sin estar asociados a ella**.

El modo monitor es **obligatorio activarlo** para poder realizar cualquier técnica de auditoría inalámbrica, por ejemplo, para romper el cifrado WEP es necesario recoger un gran número de paquetes con IVs débiles.

## Parámetros para el comando airmon-ng

```bash
airmon-ng [start|stop] [interfaz] [canal]
```

Donde sustituiremos:

- **start o stop** –> Para iniciar o parar el modo monitor.
- **interfaz** –> Nombre de la interfaz inalámbrica, ejemplo wlan0.
- **canal** –> Opcionalmente se puede especificar el numero de canal.

Las redes inalámbricas **emiten sus señales en un canal**, que va desde el 1 al 14. Los canales se utilizan (entre otras cosas) para **evitar interferencias** con otras redes Wi-Fi cercanas, al elegir un canal como parámetro en airmon-ng, nos permitirá centrarnos mejor en nuestro objetivo.

Un ejemplo para **poner nuestra tarjeta en modo monitor** sería:

```bash
airmon-ng start wlan0
```

[![/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_002.png](/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_002.png)](/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_002.png)

Vemos como nos indica que el modo monitor ha sido activado en **mon0**

Para **confirmar** que está en **modo monitor**, escribimos:

```bash
iwconfig
```

[![/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_003.png](/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_003.png)](/images/hacking-wifi-parte-iv-comandos-basicos/hacking-wifi-parte-iv-comandos-basicos_003.png)

Y verificamos que está el modo monitor (Mode:Monitor) en la interfaz mon0. Os aclararé que mon0 y wlan0 **son la misma tarjeta inalámbrica**, pero con distinta interfaz.

Nos vemos en la próxima entrada.

Saludos a todos!!!