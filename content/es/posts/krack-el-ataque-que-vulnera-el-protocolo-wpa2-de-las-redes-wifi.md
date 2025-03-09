+++
date = "2017-10-18"
title = "KRACK (Key Reinstallation Attacks): El ataque que vulnera el protocolo WPA2 de las redes WiFi"
author = "Javier Olmedo"
toc = false
+++

[![/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_banner.png](/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_banner.png)](/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_banner.png)

Ayer se hacía pública una de las noticias que más relevancia ha tenido en los últimos meses en el mundo de la ciberseguridad, el **protocolo de seguridad WPA2**, usado en la mayoría de redes WiFi de todo el mundo **ha sido roto (vulnerado)**.

El investigador Mathy Vanhoef ([@vanhoefm](https://x.com/vanhoefm)) de la Universidad KU de Leuven ha descubierto esta vulnerabilidad y lo ha puesto en conocimiento de la comunidad para que los fabricantes saquen parches para solucionarlo, algunas empresas como Microsoft se han dado prisa y [ya han puesto una solución a sus productos](https://msrc.microsoft.com/update-guide/en-US/advisory/CVE-2017-13080).

## ¿En qué consiste la vulnerabilidad?

Esta vulnerabilidad aprovecha un fallo en el **four-way handshake que se utiliza la primera vez que te conectas** al punto de acceso en el momento que se establecen las claves de cifrado, este four-way handshake **se utiliza para confirmar que tanto el cliente como el punto de acceso poseen las credenciales correctas**, en la siguiente imagen podemos ver como sería el proceso:

[![/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_001.png](/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_001.png)](/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_001.png)

En concreto en el paso 3, **después de recibir el mensaje que protege la sesión del usuario**, es dónde se puede explotar esta vulnerabilidad y realizar el ataque de reinstalación de clave (Key Reinstallation Attacks), en este paso, el cliente y punto de acceso negocian una nueva clave de cifrado para el tráfico que pueda generarse.

## ¿Qué riesgos existen?

En primer lugar, hay que aclarar que mediante este ataque **nuestra clave de red WiFi no se ve expuesta** dado que el fallo de seguridad se encuentra del lado del cliente, es decir, el atacante no obtendrá acceso a nuestra red pero **sí podrá «escuchar» todo o la mayoría del tráfico** que se esté generando en ella.

Cabe destacar que para poder explotar esta vulnerabilidad, **un atacante debería de estar físicamente dentro del alcance de la red WiFi**, y dependiendo de la configuración de red, podría inyectar y manipular los datos, obtener fotos y conversaciones de chats/mensajería, enviar malware o ransomware, obtener todo tipo de contraseñas (si se usa junto con SSLstrip incluso de sitios web con certificado, [puedes ver una entrada de este blog relacionada con este ataque](https://hackpuntes.com/obtener-credenciales-https-con-bettercap-y-sslstrip/)).

## ¿Mi red WiFi es vulnerable?

**La criticidad de este ataque es importante**, afecta a todas las redes WiFi con protocolo WPA2 que no hayan sido parcheadas (pocos fabricantes tienen solución de momento), por lo tanto, hay una **alta probabilidad que si no has aplicado ningún parche de seguridad a tu router o dispositivo seas vulnerable** al ataque.

[![/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_001.png](/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_001.png)](/images/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi/krack-el-ataque-que-vulnera-el-protocolo-wpa2-de-las-redes-wifi_001.png)

## ¿Qué medidas de seguridad debemos tomar?

Por el momento, **la mejor medida de seguridad que podemos usar es protegernos haciendo uso de redes privadas virtuales (VPN)** y asegurarnos que navegamos por páginas web mediante el protocolo https.

Consejos y recomendaciones:

- Estad atentos de los parches de seguridad que saldrán en los próximos días.
- Evita conectarte a redes WiFi que no sean conocidas (aunque estén protegistas con contraseña).
- Cambia y oculta el nombre de tu red WiFi.
- Contrata o haz uso de un servicio VPN.
- Verifica que estás navegando por https.
- No cambies el protocolo de seguridad de tu WiFi, aunque WPA2 haya sido roto, sigue siendo el más seguro de momento.

## Poc del ataque KRACK (Key Reinstallation Attacks)

{{< youtube Oh4WURZoR98 >}}

Puedes ver el paper de Mathy Vanhoef ([@vanhoefm](https://x.com/vanhoefm)) en la [Computer and Communications Security (CCS 2017)](https://papers.mathyvanhoef.com/ccs2017.pdf)

Una vez más, la seguridad informática queda en entredicho.

Un Saludo!!