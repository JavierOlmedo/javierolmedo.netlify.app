+++
date = "2025-03-07"
title = "Snyk Fetch The Flag - ClickityClack"
description = "Este artículo detalla la resolución de un desafío de captura de bandera (CTF) llamado ClickityClack de Snyk. El reto involucra el análisis de un archivo pcapng que contiene tráfico USB, específicamente pulsaciones de teclado."
author = "Javier Olmedo"
toc = false
tags = ["ctf", "usb"]
+++

[![/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_banner.png](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_banner.png)](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_banner.png)

En este reto, partimos de un fichero `pcapng` que contiene la flag oculta en su interior. Inicialmente, intenté analizarlo con **Wireshark**, pero no encontraba nada relevante. Fue entonces cuando una simple búsqueda en Google me dio la pista clave:

```txt
pcapng usb ctf
```

[![/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_001.png](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_001.png)](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_001.png)

Esta búsqueda me llevó a descubrir una herramienta llamada [Usb_Keyboard_Parser.py](https://raw.githubusercontent.com/5h4rrk/CTF-Usb_Keyboard_Parser/refs/heads/main/Usb_Keyboard_Parser.py) que parsea los paquetes de tráfico USB, como las pulsaciones de teclas.

Basta con pasar el fichero al script para obtener la bandera al final de las pulsaciones.

```bash
python Usb_Keyboard_Parser.py click.pcapng
```

```txt
[-] Found Modifier in 10 packets [-]

[+] Using filter "usbhid.data" Retrived HID Data is : 

aaaaacaaaaaaaaaabaaaaaababadaaaabaaaaaacaaaabaaaaaaadabbacbabccabcccabcgccccccgccdcdcgccdfccdccdfccddccfccfdbccaccbacbbcbbcbbbcbbcbbbbbcbbcbbcgbcbbbcbbbccbacbcbbbcbbcbbbbbbcbbafbbbababaaeabaaaaaaacaaaaaaaacaaaaaaeabaaaaaaabaaababbbbacbbabbbbbbbbbbbebbbabbbcbebbcbbcbbebbcacbbbbbfbbbcacbgcababbdbbdbababbabbcbbababbbaebabaabbbaeabbaababbabaebabbabbaeabaaabaaaabaabaabaaabaaaaebaaaaaaabacaabaaaaaaaaaaaaeaaaaaaaaaaaaaaaaaaaaaaacaaaaaaaaaaabaabaaafaabababbabaaaacaababaaacabaababbbbbbbebcbdcccccccfdceddceddcddefeeheeefeaeeeeiefeefefnefefeiefeeiefefefeeieefaeeeejeedfedhdedbdegddcdgdddcdcdacdfdccccdfcccbbbcbbbbbbbbbadaaaaaaaaaaflag{a3ce310e9a0dc53bc030847192e2f585}

↑↑Thhis iis a ppreetty ccool thhing tthatt I hhave ffigguredd ooutt.
Yoou can capturre thhe chharacteers yyouuu type in PCAPs.aa↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmmaaaaaaabdbaaaaaaaaabaaaaaa
```

## 🚩 Flag

```txt
flag{a3ce310e9a0dc53bc030847192e2f585}
```