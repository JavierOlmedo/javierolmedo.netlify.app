+++
date = "2017-12-12"
title = "Instalación de Mozilla Firefox Quantum en Kali Linux"
author = "Javier Olmedo"
toc = false
+++

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_banner.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_banner.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_banner.png)

En esta entrada, veremos **como instalar Firefox Quantum en Kali Linux** y otras distribuciones basadas en Debian.

Firefox Quantum es el nuevo navegador de Mozilla, **mucho más rápido** que su antecesor y con un **nuevo motor** con lo último en tecnología para navegadores.

## Pasos a seguir para su instalación

1) **Descargamos** Firefox Quantum desde su web oficial https://www.mozilla.org/es-ES/firefox/

2) Una vez descargado, **botón derecho** y extraemos los archivos, también podemos descomprimir con el comando **tar**, de la siguiente manera:

```bash
tar -xvzf [NOMBREARCHIVO]
```

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_001.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_001.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_001.png)

3) **Movemos** la carpeta que hemos descomprimido en el directorio `/etc/`

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_002.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_002.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_002.png)

4) **Eliminamos** el Firefox antiguo, esto podemos hacerlo con el comando:

```bash
apt-get remove firefox-est
```

5) Instalamos **alacarte** para personalizar nuestro menú con el comando:

```bash
apt-get install alacarte
```

Y después lo ejecutamos:

```bash
alacarte
```

6) Creamos un **nuevo elemento** dentro de Internet.

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_003.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_003.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_003.png)

7) Configuramos el nuevo elemento, os dejo por aquí el icono que he usado yo, es recomendable que lo guardéis en la ruta `/usr/share/icons`

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_004.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_004.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_004.png)

Descargar el logo

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_005.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_005.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_005.png)

8) Desde el panel, en **mostrar aplicaciones**, buscamos Firefox y lo añadimos a favoritos

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_006.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_006.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_006.png)

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_007.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_007.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_007.png)

9) Ahora **podremos verlo en nuestra barra** de Kali Linux

[![/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_008.png](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_008.png)](/images/instalacion-de-mozilla-firefox-quantum-en-kali-linux/instalacion-de-mozilla-firefox-quantum-en-kali-linux_008.png)

Espero os haya servido de ayuda.

Saludos!!