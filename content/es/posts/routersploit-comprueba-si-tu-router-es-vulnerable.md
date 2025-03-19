+++
date = "2018-07-28"
title = "RouterSploit - Comprueba si tu router es vulnerable"
author = "Javier Olmedo"
toc = false
+++

El **router** es, posiblemente, el **dispositivo más propenso a ataques de nuestro hogar**, ya que es el encargado de dirigir todo el tráfico de nuestra red hacía Internet, lo que **expone a que cualquier atacante externo intente vulnerarlo**. Desafortunadamente, la mayoría de las personas no dedican mucho tiempo a configurar este dispositivo crítico, por ejemplo, **no actualizamos el firmware o dejamos las contraseñas por defecto**. En esta entrada, vamos a ver como podemos auditar de manera muy sencilla nuestro router mediante el framework **RouterSploit.**

## ¿Por qué deberías de comprobar si tu router es vulnerable?

El FBI, [pidío no hace mucho tiempo](https://www.ic3.gov/media/2018/180525.aspx), que reiniciaramos el router de nuestros hogares porque se estaba llevando a cabo **un ciberataque a escala mundial** hacía estos dispositivos, los atacantes, aprovechavan vulnerabilidades para **usar nuestro router con fines delictivos, recolectar tráfico o infectar equipos de la red interna.** A parte de esto, si un ciberdelincuente accediera a nuestra red Wi-Fi, quedaríamos totalmente expuesto y permitiríamos que **espiara todo el tráfico que se generara en nuestra red de forma remota**, incluyendo credenciales y datos confidenciales.

## RouterSploit

RouterSploit, es un **framework** de seguridad **open source** muy similar al conocido Metasploit con el cual podremos auditar nuestros dispositivos (routers, webcam, NAS, etc) para **comprobar si tienen vulnerabilidades** conocidas.

El framework, cuenta con los siguientes 5 módulos:

- **exploits:** módulos que aprovechan las vulnerabilidades identificadas.
- **creds:** módulos para probar credenciales en los servicios de red.
- **scanners:** módulos que verifican si un objetivo es vulnerable a cualquier exploit.
- **payloads:** módulos para generar cargas útiles en diversas arquitecturas.
- **generic:** módulos que realizan ataques genéricos.

## 📥 1. Instalación

RouterSploit, **requiere** de los siguientes paquetes:

- future
- requests
- paramiko
- pysnmp
- pycrypto

Y opcionalmente de:

- bluepy

1.1 Instalamos pip en Python3.

```bash
apt-get install python3-pip
```

1.2 Clonamos el repositorio a nuestro equipo.

```bash
git clone https://www.github.com/threat9/routersploit
```

1.3 Nos posicionamos en él.

```bash
cd routersploit
```

1.4 Instalamos los requisitos.

```bash
python3 -m pip install -r requirements.txt
```

1.5 Ejecutamos RouterSploit

```bash
python3 rsf.py
```

[![/images/routersploit-comprueba-si-tu-router-es-vulnerable/routersploit-comprueba-si-tu-router-es-vulnerable_001.png](/images/routersploit-comprueba-si-tu-router-es-vulnerable/routersploit-comprueba-si-tu-router-es-vulnerable_001.png)](/images/routersploit-comprueba-si-tu-router-es-vulnerable/routersploit-comprueba-si-tu-router-es-vulnerable_001.png)

## 🕵️ 2. Uso

Para el uso de RouterSploit, sólo debemos de **conocer la IP del dispositivo a auditar**, si no has cambiado tus IPs, posiblemente la de tu router sea `192.168.1.1` o `192.168.0.1`

2.1 Una vez lanzado RouterSploit, seleccionamos el módulo scanner con autopwn (esto lanzara todos los exploit contra el objetivo)

```bash
use scanner/autopwn
```

2.2 Marcamos el target

```bash
set target [IP-DISPOSITIVO]
```

2.3 Lanzamos el ataque

```bash
run
```

Os dejo un [vídeo](https://asciinema.org/a/180370) de ejemplo de uso:

## ♻️ 3. Actualizar

Podemos actualizar RouterSploit con los siguientes comandos:

3.1 Nos posicionamos sobre la carpeta que contiene el código fuente del proyecto.

```bash
cd routersploit
```

3.2 Actualizamos con el comando

```bash
git pull
```

## 🔗 4. Proyecto en GitHub

[![/images/routersploit-comprueba-si-tu-router-es-vulnerable/routersploit-comprueba-si-tu-router-es-vulnerable_002.png](/images/routersploit-comprueba-si-tu-router-es-vulnerable/routersploit-comprueba-si-tu-router-es-vulnerable_002.png)](/images/routersploit-comprueba-si-tu-router-es-vulnerable/routersploit-comprueba-si-tu-router-es-vulnerable_002.png)

https://github.com/threat9/routersploit

Hasta la próxima !👋