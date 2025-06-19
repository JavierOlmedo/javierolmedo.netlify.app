+++
date = "2019-10-14"
title = "Hackpuntes Newsletter #2"
author = "Javier Olmedo"
toc = false
+++

Hola Pentesters!! Segunda entrega de esta serie de entradas, espero que os sea de ayuda. Esta newsletter cubre la semana del **07 de octubre al 13 de octubre de 2019**.

## 🧰 Herramienta

[TotalRecon](https://github.com/vitalysim/totalrecon)

Este **script** no es una herramienta como tal, pero nos ayuda a instalar algunas de las mejores orientadas a la fase de reconocimiento.

- Fast web fuzzer (ffuf)
- Dirsearch
- Findomain
- Httprobe
- Masscan
- Nmap
- Sublist3r
- WhatWeb
- Subjack
- Amass
- Waybackurls
- Meg
- GitGraber
- getJS
- LinkFinder
- MassDNS
- EyeWitness

## 📝 Writeup

[Referencia insegura a objeto que lleva a la ejecución de comandos](https://www.rahulr.in/2019/10/idor-to-rce.html)

El investigador de seguridad **RahulR** comparte en su blog un fallo de seguridad que descubrió sobre servidores web que desplegan Dockers, nos muestra como se puede obtener ejecución de comandos en otros contenedores a través de referencias inseguras a objetos (cambiando su ID por el de otro usuario).

## 🎬 Video

{{< youtube MQGozZzHUwQ >}}

¿Estás pensando en sacarte alguna certificación de Offensive Security? ¿No sabés como tomar bien tus notas o como realizar el informe? Entonces este vídeo te será de muchísima ayuda, como me ha servido a mi. Aprenderemos a crear documentación de calidad a través de MarkDown y a generar informes PDF a partir de ellos. El código lo tenéis [aquí](https://github.com/JohnHammond/oscp-notetaking)

## 🐛 Exploit

[Joomla 3.4.6 – ‘configuration.php’ Ejecución Remota de Código](https://www.exploit-db.com/exploits/47465)

El Gestor de Contenidos **Joomla** vuelve a tener otra vulnerabilidad **0 day**, muy similar a la que tuvo en 2015 (**CVE-2015-8562**), esta vez solamente afecta a las versiones de la rama 3.x e inferiores a la 3.4.7, aún así, aségurate de no estar afectado por ella.

## 💰 Bug bounty

|Divulgación|Vulnerabilidad|Bounty|
|---|---|---|
|07 octubre 2019|[Know whether private project name exists or not within a group using link comments](https://hackerone.com/reports/495497)|$300|
|08 octubre 2019|[Panorama UI XSS leads to Remote Code Execution via Kick/Disconnect Message](https://hackerone.com/reports/631956)|$9000|
|09 octubre 2019|[Malformed .MDL triggers an Access Violation on GoldSRC (hl.exe)](https://hackerone.com/reports/495793)|$2000|
|09 octubre 2019|[Found Origin IP’s Lead To Access To Grafana Instance , PgHero Instance Can SQL Injection](https://hackerone.com/reports/687908)|$200|
|09 octubre 2019|[Bypass _token in forms Merchant.Kartpay.com](https://hackerone.com/reports/642643)|—|
|09 octubre 2019|[Non-secure requests to www.lahitapiola.fi are not automatically upgraded to HTTPS](https://hackerone.com/reports/161485)|$50|
|10 octubre 2019|[Examples directory is PUBLIC on https://████████mil, leading to multiple vulns](hhttps://hackerone.com/reports/674741)|—|
|10 octubre 2019|[Unauthenticated read and write access to ALL endpoints of a store is possible for removed staff members who had «Apps» permission](https://hackerone.com/reports/700831)|$1500|
|10 octubre 2019|[RCE on https://█████/ Using CVE-2017-9248](https://hackerone.com/reports/491668)|—|
|11 octubre 2019|[Manipulation of exam results at Semrush.Academy](https://hackerone.com/reports/662583)|$600|
|11 octubre 2019|[Reflective Cross-site Scripting via Newsletter Form](https://hackerone.com/reports/709336)|$2000|
|11 octubre 2019|[Disclosure of Email title report in quick award paypout email (no content mode)](https://hackerone.com/reports/689997)|$500|
|11 octubre 2019|[FG-VD-18-165 WordPress Cross-Site Scripting Vulnerability Notification II](https://hackerone.com/reports/460911)|$650|
|13 octubre 2019|[Crash (DoS) when parsing a hostile TIFF](https://hackerone.com/reports/195580)|$500|
|13 octubre 2019|[Memory corruption when parsing a hostile PHAR archive](https://hackerone.com/reports/195586)|$500|
|13 octubre 2019|[Information disclosure in mmap module – python 2.7.12](https://hackerone.com/reports/174632)|$500|

Un saludos a todos y happy hacking!!