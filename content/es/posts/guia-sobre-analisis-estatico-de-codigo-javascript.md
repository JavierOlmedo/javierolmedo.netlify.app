+++
date = "2019-05-20"
title = "Guía sobre análisis estático de código JavaScript"
author = "Javier Olmedo"
toc = false
+++

**JavaScript** se ha convertido en una de las tecnologías **más utilizadas** de los navegadores modernos gracias a frameworks como AngularJS, ReactJS o Vue.js. Esta entrada pretende ser una guía sobre cómo encontrar y analizar todos los archivos JavaScript que contiene una aplicación web con el objetivo de detectar posibles vulnerabilidades, como en la entrada sobre [toma de control de subdominios](https://hackpuntes.com/guia-sobre-toma-de-control-de-subdominios/), esta guía intentaré tenerla actualizada.

Es esencial que entendamos la superficie de ataque de las aplicaciones y saber **qué debemos buscar y dónde**. El análisis de código JavaScript es uno de ellos, por eso a continuación, se detallan una serie de puntos a seguir para detectar fallos de seguridad en el lado del cliente.

## 1. ¿Qué buscamos?

Nos interesa realizar un análisis de código estático de JavaScript ya que podemos encontrar los siguiente:

- Información de utilidad para aumentar la superficie de ataque (URL, dominios, IP, etc.)
- Obtención de datos sensibles (contraseñas, API Keys, almacenamiento, etc.)
- Descubrimiento de código potencialmente peligroso (eval, InnerHTML, etc.)
- Componentes con vulnerabilidades conocidas (jQuery, Bootstrap, etc.)

## 2. Recopilación de archivos JavaScript

Posiblemente, existan múltiples maneras de recopilar archivos JavaScript de una aplicación web, **las dos que se describen a continuación**, podría decirse que son esenciales para ayudarnos a recopilarlos.

### 2.1 Mediante Burp Suite

De sobra conoces esta herramienta, haz pasar tu navegador por su proxy y empieza a navegar por toda la web, **no te dejes ni un rincón**. Una vez hayas terminado, ve a la pestaña `Proxy > HTTP History` y establece un filtro para mostrar únicamente aquellos archivos que terminen en `.js`, después, selecciona todas las URLs y cópialas.

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_001.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_001.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_001.png)

Filtro aplicado al historial del Proxy HTTP en Burp Suite

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_002.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_002.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_002.png)

Copiado de URLs en Proxy de Burp Suite

💡Si estás utilizando la **versión Pro** de Burp Suite, además de extraer las URLs de los archivos JavaScript, también puedes exportarlos desde la pestaña `Target > Site map` y pulsando sobre la opción `Find scripts`.

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_003.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_003.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_003.png)

Extracción de archivos js del sitio hackpuntes.com

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_004.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_004.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_004.png)

Búsqueda de archivos js del sitio hackpuntes.com

### 2.2 Desde archive.org

[Archive.org](https://archive.org/web/) dispone de una base de datos de **cómo ha sido tu web en el pasado**, tiene una araña que de forma ocasional pasa por tu web para almacenar todos los recursos que encuentre. Buscar archivos JavaScript desde aquí es una técnica completamente pasiva, dado que no enviamos ninguna solicitud al servidor de nuestra aplicación web, además, podemos encontrar algún recurso «olvidado».

Para realizar esta tarea, existe un proyecto en GitHub llamado [Waybackurls](https://github.com/tomnomnom/waybackurls/), es una excelente herramienta para buscar archivos JavaScript (u otros). Con el siguiente comando, podemos listar de manera ordenada ascendentemente y sin valores repetidos los archivos JavaScript de un dominio:

```bash
waybackurls hackpuntes.com | grep "\.js" | uniq | sort
```

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_005.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_005.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_005.png)

Ejemplo de uso de la herramienta waybackurls

El uso de Archive.org puede generar **falsos positivos**, es decir, podemos obtener rutas de archivos que actualmente ya no se encuentran en la aplicación web. Confirmar si estos archivos existen o no es fácil a través de peticiones curl, un ejemplo sería el siguiente, donde el `código 200` nos indica que aún está disponible:

```bash
cat endpoints.txt | parallel -j50 -q curl -w 'Codigo:%{http_code}\t Tamano:%{size_download}\t %{url_effective}\n' -o /dev/null -sk
```

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_006.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_006.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_006.png)

Comprobación de archivos existentes en la aplicación mediante petición curl

## 3. Hacer legible los archivos JavaScript

En la mayoría de las veces, los archivos JavaScript que hayamos recopilado **pueden no ser legibles o entendibles**, esto se debe a que el desarrollador de la aplicación ha podido **ofuscar o minificar el código** para evitar que lo copien o mejorar el rendimiento de la web.

- El **minificado**, es el proceso de **eliminación de datos innecesarios o redundantes** de un archivo con el objetivo de reducir su tamaño (comentarios, nombres de variables largas, código no utilizable, formato) casi seguro has visto recursos que terminan en `*.min.js`, eso significa que el archivo original ha sido minimizado, un ejemplo lo podemos ver con **[jQuery](https://jquery.com/download/)**. También puedes minimizar tu código con [UglifyJS](https://github.com/mishoo/UglifyJS).
- La **ofuscación**, en cambio, implica hacer modificaciones a las variables y funciones para cambiarlas por otras menos entendibles, por ejemplo, si tenemos una función llamada `obtenerUsuario()`, pasaremos a llamarla `xdcvadffasf()`, de esta manera dificultaremos al atacante que entienda el código.

Explicado esto, te preguntarás **¿cómo podemos «desminimizar» el JavaScript?**, para esta tarea, existe una excelente herramienta llamada [JSBeautify](https://github.com/beautify-web/js-beautify), la cual también permite desofuscar código e incluso dispone de una [extensión](https://github.com/HookyQR/VSCodeBeautify) para Visual Studio Code.

```bash
js-beautify [archivo].min.js
```

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_007.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_007.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_007.png)

Ejemplo de visualización de archivo desminimizado con JSBeautify

No existe una herramienta o técnica que permita o se ajuste a la desofuscación del código al 100%, tendremos que probar varias herramientas hasta conseguir nuestro objetivo, que es entender el código. Posiblemente incluso llegues a la conclusión de que debas hacerlo manualmente, de todas maneras, te dejo otras herramientas para que pruebes como [JStillery](https://github.com/mindedsecurity/JStillery), [JSDetox](http://relentless-coding.org/projects/jsdetox) o [IlluminateJS](https://github.com/geeksonsecurity/illuminatejs).

## 4. Identificar información relevante

### 4.1 Rutas relativas y URL completas

Una de las informaciones clave que se deben de **buscar** en los archivos JavaScript son los **puntos finales**, es decir, las URL completas, rutas relativas, etc. Esto nos ayudará a identificar la superficie de ataque y descubrir posibles vulnerabilidades o vectores de ataque.

#### 4.1.1 Rutas relativas

En este punto, [relative-url-extractor](https://github.com/jobertabma/relative-url-extractor) es una herramienta que hace muy bien su trabajo, permite **extraer las rutas relativas partiendo de un js**. Esta herramienta puede funcionar en archivos que se encuentren en local o en remoto, además, también puede trabajar sobre archivos minificados.

```bash
ruby extract.rb [FILE]
```

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_008.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_008.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_008.png)

Uso de la herramienta relative-url-extractor

#### 4.1.2 URL completas

[LinkFinder](https://github.com/GerbenJavado/LinkFinder) es una herramienta para enumerar los archivos JavaScript partiendo de un dominio, está desarrollada en Python y hace uso de JSBeautify. Muy útil para identificar URL, puntos finales y parámetros.

```bash
python linkfinder.py -i https://hackpuntes.com -d -o cli
```

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_009.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_009.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_009.png)

Uso de la herramienta LinkFinder

### 4.2 Recursos en la nube

Mucho de los recursos JavaScript que encontremos, podrían estar alojados en la nube (Amazon S3 o Buckets), con [CloudScraper](https://github.com/jordanpotti/CloudScraper) podremos «arañar» nuestro objetivo

```bash
python3 CloudScraper.py -u https://hackpuntes.com
```

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_010.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_010.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_010.png)

Uso de la herramienta CloudScraper

### 4.3 Buscar datos sensibles

Muchos desarrolladores, guardan información confidencial en los archivos JavaScript, como pueden ser **credenciales hardcodeadas** o **claves APIS** de servicios de terceros. Para buscar esta información sensible, pueden emplearse [expresiones regulares](https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Regular_Expressions). También existe una herramienta llamada [truffleHog](https://github.com/dxa4481/truffleHog) para buscar información jugosa en repositorios, de obligatorio uso si auditamos en caja blanca, y por supuesto, no te olvides del comando `grep` (os dejo una [entrada](https://hackpuntes.com/que-es-smali-y-como-parchear-una-aplicacion-android/) de mi compañero José Arenas dónde hace un buen uso de este comando).

### 4.4 Código potencialmente peligroso

Cuando buscamos en los archivos JavaScript, es muy importante identificar aquellas partes del código en las cuales **el programador ha podido cometer un error**, y llevarnos a posibles problemas de seguridad.

- Un ejemplo, es el uso de **innerHTML**, el cual nos puede indicar de un posible problema de Cross-Site Scripting (XSS). En los frameworks modernos, existen equivalentes a innerHTML como puede ser [dangerouslySetInnerHTML](https://thehackerblog.com/i-too-like-to-live-dangerously-accidentally-finding-rce-in-signal-desktop-via-html-injection-in-quoted-replies/index.html) (en el caso de React), que provocó vulnerabilidades críticas. El uso inadecuado de `bypassSecurityTrustX` en Angular es otro ejemplo que nos puede llevar a lograr un XSS. Por último os diré que **eval** es la función del mal 👿.

- La API de [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) es una alternativa a JSONP, XHR con CORS y otros métodos que permiten el envío de datos entre orígenes pasando por alto la [Política de mismo origen](https://developer.mozilla.org/es/docs/Web/Security/Same-origin_politica) (SOP). Existen varios **inconvenientes para que un atacante pueda [explotar con éxito](https://labs.detectify.com/2016/12/08/the-pitfalls-of-postmessage/) postMessage**, pero una vez se entienda como se implementa, busca `window.postMessage` y un evento `window.addEventListener` asociado, después, prueba si sería posible ejecutar JavaScript.

- `LocalStorage` y `sessionStorage` son objetos de almacenamiento web HTML. Aunque este almacenamiento no es para guardar datos sensibles, se suele utilizar para ello con bastante más frecuencia de la que creemos. Busca en el código `window.localStorage` o `window.sessionStorage`, también puedes echar un vistazo de vez en cuanto a las **DevTools** (F12) de Google Chrome, dentro del menú `Application > Storage`.

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_011.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_011.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_011.png)

Ejemplo de dato almacenado en LocalStorage

En buscar, analizar y entender todo el código JavaScript de una aplicación web se nos va a ir **gran parte del tiempo de la auditoría**.

## 5. Herramientas de análisis de código estático

El uso de herramientas para el análisis estático de código nos facilitará la identificación de vulnerabilidades. [JSPrime](https://github.com/dpnishant/jsprime) es una de esas herramientas a tener siempre en nuestro arsenal aunque no se haya actualizado desde hace tiempo.

Otro proyecto muy popular es [ESLint](https://github.com/eslint/eslint), es fácilmente personalizable y existen [múltiples reglas personalizadas](https://github.com/LewisArdern/eslint-plugin-angularjs-security-rules), especialmente para frameworks modernos (Angular, React, etc).

Por último, os dejo una herramienta excepcional, se trata de [FileChangeMonitor](https://github.com/cablej/FileChangeMonitor/), esta herramienta estará pendiente de que el código fuente de los archivos JavaScript de una aplicación web no cambien, en caso de que lo hagan, recibiremos un correo electrónico con los cambios realizados. Si tenemos pereza y no queremos montarla en nuestra raspberry, siempre podemos usar su [servicio](https://filechangemonitor.io/). Esta herramienta debemos **utilizarla una vez tengamos todos los puntos finales de la aplicación**.

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_012.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_012.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_012.png)

Correo electrónico enviado por FileChangeMonitor al detectar un cambio en archivo javascript

## 6. Librerías JavaScript

De igual manera, debemos **identificar las librerías JavaScript antiguas o vulnerables** que componen una aplicación web. Si utilizamos Burp Suite, existe una extensión que de manera pasiva nos permite identificar las librerías obsoletas, se llama [Retire.js](https://github.com/h3xstream/burp-retire-js) y también tenemos disponible una extensión para los navegadores [Mozilla Firefox](https://addons.mozilla.org/es/firefox/addon/retire-js/?src=search) y [Google Chrome](https://chrome.google.com/webstore/detail/retirejs/moibopkbhjceeedibkbkbchbjnkadmom?hl=es)

[![/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_013.png](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_013.png)](/images/guia-sobre-analisis-estatico-de-codigo-javascript/guia-sobre-analisis-estatico-de-codigo-javascript_013.png)

Reporte del plugin Retire.js en Burp Suite

## 7. Conclusión

Durante la entrada, hemos visto de manera genérica como podemos analizar de forma estática el código JavaScript de una aplicación. Si conoces técnicas u otras herramientas de análisis estático y si desea **compartirlos**, deja un comentario.

¡Hasta la próxima!