+++
date = "2015-05-07"
title = "Ataque DNS Spoofing con Cain&Abel"
author = "Javier Olmedo"
toc = false
+++

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_banner.jpeg](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_banner.jpeg)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_banner.jpeg)

En la entrada de hoy, os mostraré como realizar un **ataque DNS Spoofing** en una red de area local, normalmente no suelo trabajar con Windows, pero en esta entrada lo utilizaremos, dado que la herramienta que vamos a trabajar solo está disponible para este SO.

La herramienta se llama **Cain&Abel**, y la podéis descargar de su **pagina oficial** [AQUI](http://www.oxid.it/cain.html), puede que vuestro navegador os indique que estáis a punto de descargar una **herramienta potencialmente peligrosa**, de igual manera somos conscientes del uso de ella y aceptamos para descargarla.

**Antes de realizar** el ataque os comentaré un poco en que consiste este ataque.

## ¿En qué consiste el ataque?

El ataque DNS Spoofing consiste en **alterar las direcciones de los servidores DNS** que utiliza la víctima, de esta manera tomar el control sobre las consultas que la víctima realiza.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_001.jpg](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_001.jpg)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_001.jpg)

Los servidores DNS son los encargados de **transformar los nombres de dominios en IP y viceversa**, de esta manera no tenemos que recordar las direcciones IP de cada servicio a los que accedemos, por ejemplo si quisiéramos visitar Facebook, únicamente accederíamos a **Facebook.com** y no a su IP, mucho mas fácil de recordar. ¿O te acuerdas de la IP de Facebook?

## ¿Para qué se hace este ataque?

Aprovechando este ataque, **permite beneficiarse de la ruta de comunicación** cuando una victima **consulta un sitio web**, de esta forma un atacante podría clonar la pagina de inicio de Facebook o cualquier otra pagina, montarla en un servidor propio y redirigir el trafico hacia su servidor malicioso, esta técnica es conocida como **Phising** y se suele utilizar para el **robo de credenciales**.

## ¿Cómo se puede evitar este ataque?

Es muy importante **deshabilitar la opción de gestión remota de los routers** (para ataques de fuera de la red local) que por defecto suelen venir siempre activadas, muy importante  también mantener el sistema operativo actualizado.

Otro punto para tener en cuenta es la **verificación de las conexiones a sitios que utilizan cifrado**. Es importante prestar atención al protocolo ya que, por ejemplo, si un sitio utiliza HTTPS y cuando se accede utiliza HTTP, el usuario podría estar **accediendo a un sitio falso** réplica del original.

## Pasos para realizar el Ataque DNS Spoofing

1) **Descargar** Cain&Abel del enlace anteriormente publicado e instalarlo en el PC **(aceptar también la instalación de WinPcap)**, si no, no podremos realizar el ataque.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_002.png](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_002.png)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_002.png)

2) **Desactivar el Firewall** de Windows y **ejecutar** Cain&Abel siempre **como administrador**.

3) Vamos a la pestaña **«Sniffer»**, y activamos la opción **«Sniff»**, después vamos al icono **+** para **realizar un escaneo a la red** de los dispositivos que se encuentran conectados a ella.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_003.gif](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_003.gif)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_003.gif)

4) Vamos a la pestaña **ARP** situada en la **parte inferior**.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_004.png](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_004.png)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_004.png)

5) **«Click»** en la ficha de **ARP** (se nos habilitará el botón **+** para elegir entre que dispositivos queremos hacer el ataque).

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_005.png](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_005.png)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_005.png)

6) En la siguiente ventana, elegiremos la **puerta de enlace (Router)** que normalmente suele tener la IP 192.168.1.1, y el **dispositivo que queremos atacar**, es decir, nos queremos **posicionar en la ruta de la comunicación** entre la puerta de enlace y el dispositivo víctima.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_006.gif](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_006.gif)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_006.gif)

7) **«Click»** en el botón **envenenamiento ARP**, situado en la **parte superior**.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_007.png](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_007.png)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_007.png)

8) Una vez que está activado el envenenamiento, vamos a la pestaña de la izquierda llamado **«ARP-DNS»** y hacemos **«Click derecho»**.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_008.png](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_008.png)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_008.png)

9) Nos aparecerá un dialogo, el cual deberemos especificar el **nombre de dominio** que queremos suplantar (parte superior) y la **direccion IP** a la cual se reenviara el trafico (parte inferior) cuando la victima visite el sitio a suplantar.

En la IP podemos poner por ejemplo, la IP de un servidor malicioso con una pagina de acceso de Facebook clonada (para hacer phising y obtener las credenciales), o bien la IP de otra web totalmente distinta, por ejemplo poner la IP de una pagina XXX.

[![/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_009.png](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_009.png)](/images/ataque-dns-spoofing-con-cainabel/ataque-dns-spoofing-con-cainabel_009.png)

Como nota comentaros que si queréis saber la IP de un sitio web, basta con abrir la consola de comandos y teclear:

```bash
ping [nombre-de-dominio]
```

Tened cuidado con este tipo de ataques y verificar siempre el sitio al cual accedéis.

Saludos!!!!!!