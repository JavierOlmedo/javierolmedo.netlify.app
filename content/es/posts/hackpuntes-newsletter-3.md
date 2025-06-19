+++
date = "2019-10-21"
title = "Hackpuntes Newsletter #3"
author = "Javier Olmedo"
toc = false
+++

Hola de nuevo, Pentesters!! Tercera entrega de esta serie de entradas, espero que os sea de ayuda. Esta newsletter cubre la semana del **14 de octubre al 20 de octubre de 2019**.

## 🧰 Herramienta

[Brozzler](https://github.com/internetarchive/brozzler)

Brozzler en un **rastreador web** distribuido desarrollado en Python que utiliza el navegador para buscar páginas y URL con el fin de extraer enlaces. Se diferencia con otras tecnologías de rastreo al interactuar mediante el navegador real y registrar las interacciones entre los servidores web y clientes.

## 📝 Writeup

[Microsoft Edge – Universal XSS](https://leucosite.com/Microsoft-Edge-uXSS/)

El **Cross-Site Scripting Universal** (uXSS), es una vulnerabilidad muy codiciada en los navegadores que permite a un usuario malintencionado ejecutar código malicioso desde cualquier web (es como tener un XSS en todos los sitios). Normalmente, estas vulnerabilidades tienen asociado el elemento iframe o URL.

## 🎬 Video

{{< youtube hM2Zvsak3GM >}}

Video interesante sobre como realizar **ingeniería inversa a malware** con IDA Pro, está muy bien explicado y cuenta con recursos interesantes en la descripción del video.

## 🐛 Exploit

[sudo 1.2.27 – Security Bypass](https://www.exploit-db.com/exploits/47502)

Esta vulnerabilidad descubierta por Joe Vennix en **sudo**, permite ejecutar comandos en el sistema como root modificando el fichero `/etc/sudoers` (en la mayoría de distros Linux), este fallo se puede explotar con tan solo especificar el `ID` de usuario `-1` o `4294967295`. La vulnerabilidad ha sido asociada al CVE-2019-14287.

[![asciicast](https://asciinema.org/a/uibBWR3dEre1lK9AZuUK1Wagi.svg)](https://asciinema.org/a/uibBWR3dEre1lK9AZuUK1Wagi)

## 💰 Bug bounty

|Divulgación|Vulnerabilidad|Bounty|
|---|---|---|
|16 octubre 2019|[XSS vulnerable parameter in a location hash](https://hackerone.com/reports/146336)|$1100|
|16 octubre 2019|[XSS on Desktop Client](https://hackerone.com/reports/473950)|$1000|
|16 octubre 2019|[WebTorrent has DNS rebinding vulnerability](https://hackerone.com/reports/663729)|$100|
|17 octubre 2019|[Blind SQL injection in third-party software, that allows to reveal user statistic from rocket.chat and possibly hack into the rocketchat.agilecrm.com](https://hackerone.com/reports/433792)|—|
|17 octubre 2019|[URL is vulnerable to clickjacking](https://hackerone.com/reports/712376)|—|
|17 octubre 2019|[Flash “local-with-filesystem” Bypass in navigateToURL](https://hackerone.com/reports/150976)|$3000|

Un saludos a todos y happy hacking!!