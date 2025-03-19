+++
date = "2018-11-20"
title = "TOP 10 herramientas de seguridad para aplicaciones web"
author = "Javier Olmedo"
toc = false
+++

En esta entrada, os mostraré un listado de las que son (para mí), **las 10 mejores herramientas de seguridad para aplicaciones web.**

No soy un experto en hacking web y puede que no estés del todo de acuerdo con mi lista TOP, por lo tanto, siéntete libre de comentar y añadir la tuya 😉

## TOP 10 Herramientas de seguridad web

### #1 Burp Suite

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_001.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_001.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_001.png)

Es un **conjunto de herramientas** orientadas a la auditoria de aplicaciones web, el gran marco de pruebas que puede abarcar la ha convertido en una herramienta imprescindible para los profesionales de la seguridad web. Dentro de la suite podemos encontrar las siguientes herramientas:

- **Target:** Genera un sitemap de las webs que ha pasado por el proxy.  
- **Proxy:** Intercepta las peticiones entre el navegador y la aplicación.  
- **Spider:** Recopila los recursos de la aplicación de manera automatizada.  
- **Scanner:** Detecta diferentes tipos de vulnerabilidades tanto de forma pasiva como activa.  
- **Intruder:** Automatiza tareas: (fuzzing, fuerza bruta, enumeración, etc).  
- **Repeater:** Permite repetir y manipular las peticiones que pasan por el proxy.  
- **Secuencer:** Analiza la aleatoriedad de los tokens de sesión o cadenas.  
- **Decoder:** Codificador y decodificado de strings(URL, Base64, Hex, hashes, etc).  
- **Comparer:** Compara distintas peticiones y respuestas.  
- **Extender:** Nos permite agregar funcionalidades extras para Burp (plugins).

[Link](https://portswigger.net/burp)

### #2 Acunetix

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_002.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_002.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_002.png)

Es un **escáner para descubrir vulnerabilidades**, permite el uso de múltiples subprocesos para rastrea un sitio web. Esta herramienta rápida y fácil de usar, permite crear macros y grabar acciones sobre la aplicación, no aconsejaría lanzarla sobre un sitio en producción, ya que no tenemos tanto control sobre las acciones que realiza la herramienta.

[Link](https://www.acunetix.com/)

### #3 Nessus

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_003.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_003.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_003.png)

Escáner de vulnerabilidades que permite **escanear tanto aplicaciones web como infraestructuras**, incluye múltiples configuraciones de escáneres predefinidas, como por ejemplo, de malware o compliance. También nos permite escanear rangos de IPs y existen plugins que nos pueden ayudar a mejorar y optimizar las funciones del escáner.

Link](https://www.tenable.com/products/nessus/nessus-professional)

### #4 SQLmap

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_004.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_004.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_004.png)

Es una herramienta desarrollada en python y de código abierto que automatiza el proceso de **detección y explotación de vulnerabilidades de inyección SQL.** Cuenta con un potente motor de detección y gran cantidad de funciones de testeo para múltiples sistemas gestores de bases de datos.

[Link](http://sqlmap.org/)

### #5 Aquatone

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_005.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_005.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_005.png)

Conjunto de herramientas para **descubrimiento de subdominios** a partir de un determinado dominio, dentro de esas herramientas, se encuentran:

- **aquatone-discover:** Identifica los servidores de nombres autorizados para el dominio de destino (subdominios).
- **aquatone-scan:** Descubrimiento de puertos HTTP abiertos en los diferentes hosts encontrados con aquatone-discover.
- **aquatone-gather:** Carga los datos de los archivos creados y comienza a solicitar URLs para recopilar respuestas HTTP y capturas de pantalla.

Actualmente aquatone está siendo desarrollado en Go y puede cambiar su funcionamiento, pero puedes utilizar otras herramientas como [Sublist3r](https://github.com/aboul3la/Sublist3r) o [Subfinder](https://github.com/subfinder/subfinder).

[Link](https://github.com/michenriksen/aquatone)

### #6 Nmap

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_006.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_006.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_006.png)

Software de código abierto **utilizado para rastrear puertos abiertos, descubrimiento de servicios y hosts en redes de datos**. Nmap es extensible mediante el uso de scripts desarrollados por su comunidad, lo que permite optimizar y adaptar los escaneos según las condiciones de la red.

[Link](https://nmap.org/)

### #7 Metasploit

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_007.jpg](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_007.jpg)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_007.jpg)

Framework de código abierto que cuenta con información sobre vulnerabilidades y que está **orientado al desarrollo y ejecución de exploits** contra máquinas remotas.

[Link](https://www.metasploit.com/)

### #8 TheHarvester

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_008.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_008.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_008.png)

Herramienta para **recolectar información sobre emails, subdominios, hosts, nombres de empleados, puertos abiertos y banners** desde fuentes públicas como motores de búsqueda, servidores PGP, LinkedIn y la base de datos de SHODAN.

[Link](https://github.com/laramies/theHarvester)

### #9 WPScan

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_009.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_009.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_009.png)

**Escáner de vulnerabilidades en caja negra para WordPress**, cuenta con diccionarios para listar plugins, temas y una gran base de datos con las vulnerabilidades descubiertas. Algunas de sus caracteristicas son:

- Enumeración de usuarios.
- Descubrimiento de contraseñas débiles.
- Descubrimiento de versión.
- Descubrimiento de vulnerabilidades.
- Enumeración de plugins.
- Descubrimiento de plugins vulnerables.
- Descubrimiento del tema utilizado por la aplicación.
- Listado de directorios.

[Link](https://github.com/wpscanteam/wpscan)

### #10 Firefox + Plugins

[![/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_010.png](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_010.png)](/images/top-10-herramientas-de-seguridad-para-aplicaciones-web/top-10-herramientas-de-seguridad-para-aplicaciones-web_010.png)

Por defecto, uso Mozilla Firefox como navegador predeterminado para mis auditorias con los siguientes plugins:

- **Wappalyzer:** Identificación de tecnologías utilizadas por la aplicación web.
- **Shodan:** Recolección de información encontrada sobre el sitio web en la base de datos de Shodan.
- **FoxyProxy Standard:** Permite modificar el proxy utilizado por el navegador de manera rápida.
- **User-Agent Switcher:** Permite cambiar el User Agent en las peticiones.
- **Cookie Quick Manager:** Elimina, modifica y añade cookies.
- **Clear Cache:** Limpia todas las cookies que almacena el navegador.
- **iMacros for Firefox:** Permite crear macros para automatizar tareas.
- **Web Developer:** Múltiples utilidades, entre ellas, la más destacada que permite mostrar los forms ocultos.
- **Greasemonkey:** Personaliza la manera en la que se muestra o se comporta una web mediante fragmentos en javascript.

Saludos!!