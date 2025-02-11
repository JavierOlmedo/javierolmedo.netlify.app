+++
date = "2015-05-24"
title = "Hacking Wifi - Parte VII - Seleccionar el Objetivo y capturar los paquetes"
author = "Javier Olmedo"
+++

Lo que intentaremos en esta entrada, será **conseguir paquetes de la red objetivo** (evidentemente sin estar asociados a ella) y **guardarlos** en un archivo con extension **.PCAP** (Packet Capture Data), para que cuando tengamos suficientes **IVs** crackearlo para obtener la clave.

[![/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_banner.png](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_banner.png)](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_banner.png)

Me he encontrado por casa un router que tenía sin utilizar para realizar esta entrada, lo he configurado con **cifrado WEP** y he cambiado el **SSID** (nombre de la red) a **WiFi-Hackpuntes**.

## ¿Que son los IVs?

Son los **vectores de inicialización**, podríamos decir que es un contador que varia según se generen las tramas en la comunicación.

La técnica criptográfica utilizada en WEP es el cifrado de flujo (stream cipher) RC4 que **combina una clave WEP con un vector aleatorio de 24bits (IV)**, esto genera un **paquete combinado** que es el utilizado para **descifra** el paquete recibido y **cifrar** el **paquete** enviado.

## Vamos a la captura de estos IVs de nuestra red objetivo.

Lo primero que haremos será ver las **tarjetas de red inalámbricas** que tenemos disponibles, y elegir la que utilizaremos, al ser posible se utilizará una que **permita la inyección de paquetes**, recomendable el **chipset RTL8187L** de **ALFA INC**.

Tecleamos en la terminal:

```bash
iwconfig
```

[![/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_001.png](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_001.png)](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_001.png)

Vemos las **interfaces inalámbricas disponibles**, nos acordaremos del nombre de la interfaz que utilizaremos, yo en mi caso utilizaré **wlan1** puesto que es la única que tengo.

Ahora vamos a ponerla en **modo monitor**, para poder interceptar todos los paquetes, **de manera opcional** podemos cambiar la **dirección MAC** de nuestra tarjeta para más seguridad a la hora de auditar, os dejo el [enlace](https://hackpuntes.com/es/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger/) a otra entrada donde explico como hacerlo.

El modo monitor lo activaremos con **airmong-ng** de la siguiente manera, y siendo **superusuario**:

```bash
airmon-ng start [interfaz]
```

Ejemplo en mi caso:

```bash
airmong-ng start wlan1
```

[![/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_002.png](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_002.png)](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_002.png)

Nos fijamos que la interfaz **wlan1** a pasado a **modo monitor** en la interfaz **mon0**.

Ya podemos capturar paquetes, ahora **escaneamos a todas las redes al alcance** para identificar nuestra red, necesitaremos el **canal, el BSSID (MAC del AP) del objetivo y opcionalmente el ESSID (Nombre del AP)** para otros ataques si fuera necesario.

Para escanear las redes utilizaremos **airodump-ng**.

```bash
airodump-ng [interfaz-modo-monitor]
```

En mi caso sería:

```bash
airodump-ng mon0
```

[![/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_003.png](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_003.png)](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_003.png)

En el momento de lanzar el comando, nos empezará a mostrar las redes al alcance.

[![/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_004.png](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_004.png)](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_004.png)

Después de fijar nuestro objetivo, sabemos su **ESSID** (WiFi-Hackpuntes), su **BSSID** (D8:61:94:64:7D:47), su **canal** de emisión es el (1) y ademas observamos que es una red en la cual **existe un cliente conectado a ella** puesto que existe trafico **(#Data) de IVs**, y en la parte inferior de la imagen podemos ver que nos muestra que el BSSID (D8:61:94:64:7D:47) tiene un cliente (00:0C:13:05:00:02).

Ya es momento de ir **recolectando IVs (#Data) en un archivo .PCAP** para posteriormente crackearlo.

Para capturar utilizaremos airodump-ng con las siguiente opciones.

```bash
airodump-ng -c [canal] -b [BSSID] -w [archivo de captura] [interfaz]
```

En mi caso sería

```bash
airodump-ng -c 1 -b D8:61:94:64:7D:47 -w capturahackpuntes.pcap mon0
```

[![/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_005.png](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_005.png)](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_005.png)

Observamos como se crea un archivo, donde ira guardando los IVs.

[![/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_006.png](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_006.png)](/images/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes/hacking-wifi-parte-vii-seleccionar-el-objetivo-y-capturar-los-paquetes_006.png)

Para crackear la clave, es necesario capturar miles de **IVs (#Data)**, en la próxima entrada veremos como aumentar el tráfico en la red y **aumentar la velocidad a la que se capturan IVs**, lo haremos tanto si existen clientes como si no los hubiese.

Un Saludo a todos!!!