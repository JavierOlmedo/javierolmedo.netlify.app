+++
date = "2015-03-27"
title = "Hacking Wifi – Parte III – Instalación de Aircrack-ng"
author = "Javier Olmedo"
+++

En esta tercera parte, comenzaremos con la práctica, lo primero que haremos será **instalar Aircrack-ng** y comprobar que ha sido correctamente instalado.

Tengo que aclarar que Aircrack-ng **NO es una sola herramienta**, es una **suite** de software de seguridad inalámbrica, es decir, una recopilación de herramientas para auditar redes inalámbricas, esto debe de quedar claro porque utilizaremos varias herramientas de la suite de forma independiente, algunas de las herramientas más utilizadas son:

- **Aircrack-ng** (descifra la clave de los vectores de inicio)
- **Airodump-ng** (escanea las redes y captura vectores de inicio)
- **Aireplay-ng** (inyecta tráfico para elevar la captura de vectores de inicio)
- **Airmon-ng** (establece la tarjeta inalámbrica en modo monitor, para poder capturar e inyectar vectores)

Teniendo claro el concepto de suite, vamos a proceder a instalarlo, en mi caso lo instalaré sobre una máquina que tiene como sistema operativo **Ubuntu 14.10**. La suite Aircrack-ng está **incluida en los repositorios de Ubuntu 14.10**, los repositorios son fuentes desde las cuales podemos descargar software, para instalarlo **nos vamos a la terminal o consola** y escribimos:

```bash
sudo bash
```

Desde este momento, tenemos una terminal con permisos de **superusuario** (evidentemente tendremos que poner la contraseña), es **obligatorio** antes de instalar nada en el sistema tener los permisos necesarios para no tener problemas en la instalación, procedemos ahora a instalar la suite **Aircrack-ng**:

```bash
apt-get install aircrack-ng
```

Explico de forma detallada para quien se inicia en Linux, **apt-get** (Avanced Package Tool) es la herramienta que utiliza Debian y sus derivadas (Ubuntu 14.10 es derivada), para gestionar los paquetes disponibles en los repositorios, con la opción **install** indicaremos que  queremos instalar el nombre del paquete **aircrack-ng**.

Si todo ha ido bien, la terminal nos mostrará lo siguiente:

[![/images/hacking-wifi-parte-iii-instalacion-de-aircrack-ng/hacking-wifi-parte-iii-instalacion-de-aircrack-ng_001.png](/images/hacking-wifi-parte-iii-instalacion-de-aircrack-ng/hacking-wifi-parte-iii-instalacion-de-aircrack-ng_001.png)](/images/hacking-wifi-parte-iii-instalacion-de-aircrack-ng/hacking-wifi-parte-iii-instalacion-de-aircrack-ng_001.png)

Y ahora comprobaremos que está instalado, volvemos a la terminal y escribimos:

```bash
aircrack-ng
```

y comprobaremos que lo está.

[![/images/hacking-wifi-parte-iii-instalacion-de-aircrack-ng/hacking-wifi-parte-iii-instalacion-de-aircrack-ng_002.png](/images/hacking-wifi-parte-iii-instalacion-de-aircrack-ng/hacking-wifi-parte-iii-instalacion-de-aircrack-ng_002.png)](/images/hacking-wifi-parte-iii-instalacion-de-aircrack-ng/hacking-wifi-parte-iii-instalacion-de-aircrack-ng_002.png)

Hasta aquí la entrada de hoy, si alguien tiene problemas que comente y se intentarán revolver.

Saludos!!!!