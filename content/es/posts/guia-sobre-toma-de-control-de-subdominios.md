+++
date = "2019-04-11"
title = "Guía sobre toma de control de subdominios"
author = "Javier Olmedo"
toc = false
+++

En esta publicación, voy a hablaros sobre una vulnerabilidad conocida como **Toma de control de subdominios** (subdomain takeover), explicaré en qué consiste y como podemos detectarla. Es posible que pueda ir **actualizando esta entrada** con nuevas herramientas o técnicas de descubrimiento, así que, te sugiero que la guardes en tus favoritos. 😉

## ¿En qué consiste la vulnerabilidad?

La vulnerabilidad de toma de control de subdominio, ocurre cuando un **registro DNS** (principalmente CNAME) habilita la delegación de zona **a un servicio de terceros** (Amazon S3, Azure, GitHub, Heroku, etc) y, pasado un tiempo, este servicio **puede dejar de ser utilizado o borrado y reclamado por un usuario malintencionado**, un ejemplo de un caso real sería el siguiente:

1. El departamento de desarrollo de la empresa Hackpuntes, está trabajando en una aplicación web en pruebas, y solicita al departamento de infraestructuras que apunte el dominio **test.hackpuntes.com** a un servicio alojado en **Amazon S3** dónde se encuentra la aplicación que están desarrollando.

2. El departamento de infraestructura, añade un registro (CNAME o A) en el panel de configuración DNS del dominio hackpuntes.com para que el dominio **test.hackpuntes.com** apunte a **hackpuntesenpruebas.amazonaws.com**

3. Después de 2 largos años, la aplicación está lista, y **el departamento de desarrollo borra** la aplicación de **hackpuntesenpruebas.amazonaws.com** pero se olvida de notificarlo al departamento de infraestructuras.

4. Un usuario malintencionado, ahora tendrá la oportunidad de crear una aplicación maliciosa en **hackpuntesenpruebas.amazonaws.com** y aprovecharse que el dominio **test.hackpuntes.com** apunta a ella.

[Aquí](https://github.com/EdOverflow/can-i-take-over-xyz) puedes ver una lista de servicios que pueden ser vulnerables.

## Riesgos

Las grandes empresas, generalmente, **no auditan su configuración DNS** de forma periódica, esto hace que la toma de control de subdominio, sea un vector de ataque bastante jugoso para cualquier usuario malintencionado.

Este tipo de vulnerabilidades, en la mayoría de los casos, **se consideran de gravedad alta** ya que pueden llegar a comprometer el dominio principal o varios subdominios.

Para un atacante, es un **escenario perfecto para realizar una campaña de phising** que se aproveche de la reputación de una marca conocida (con la correspondiente mala reputación que conlleva), además, puede llevar asociado otros ataques de mayor severidad, dado que es bastante frecuente que las aplicaciones expongan las cookies de sesión a un dominio comodín (*.dominio.com), por lo que, desde cualquier subdominio se podría acceder a ellas (incluso aquellas que se marquen como seguras).

## Mitigación

Mitigar esta vulnerabilidad es bastante sencillo, bastaría con crear un proceso estandarizado (o llevar un control) para **agregar, modificar o eliminar** las entradas en los registros DNS.

Por otra parte, los **proveedores de servicios** en la nube deberían de verificar la propiedad de un dominio (aunque la responsabilidad sea del usuario), como por ejemplo, la de [GitLab](https://about.gitlab.com/2018/02/05/gitlab-pages-custom-domain-validation/).

## Herramientas

### [Sublist3r](https://github.com/aboul3la/Sublist3r)

Una de las herramientas para la enumeración de subdominios más populares, aunque hace uso de un proyecto independiente llamado subbrute (para fuerza bruta), mayormente realiza la enumeración de forma pasiva.

```bash
python sublist3r.py -d example.com
```

### [Aquatone](https://github.com/michenriksen/aquatone)

Nos permite obtener una vista previa de las tecnologías utilizadas a partir de una lista de subdominios, muy interesante herramienta para visualizar de manera rápida nuestra superficie de ataque.

```bash
./aquatone | targets.txt
```

### [Amass](https://github.com/OWASP/Amass)

Si disponemos de tiempo suficiente y contemplamos la fuerza bruta como una opción, esta herramienta de OWASP contiene buenos diccionarios y hace un excelente scraping en fuentes públicas, esta desarrollada en Go, por lo que te recomiendo que leas antes esta [entrada](https://hackpuntes.com/guia-sobre-toma-de-control-de-subdominios/).

```bash
amass -brute -d domain.com
```

### [Subjack](https://github.com/haccer/subjack)

Una de mis favoritas, podemos comprobar de manera rápida si la lista de subdominios son potencialmente vulnerables a la toma de control.

```bash
./subjack -w subdomains.txt -t 100 -timeout 30 -o results.txt -ssl
```

### [theHarvester](https://github.com/laramies/theHarvester)

Encuentra las direcciones de correo electrónico, así como subdominios y los hosts virtuales. Sin embargo, en comparación con Sublist3r, proporciona menos resultados.

```bash
python theHarvester.py -d domain.com -b all
```

## Otras técnicas

A parte del uso de herramientas, podemos utilizar otras técnicas, basadas en buscadores o en consultas a transferencias de zona para intentar completar la lista de subdominios, algunas técnicas son:

### Transferencia de zona

La técnica más sencilla y básica es probar una solicitud AXFR directamente en el servidor DNS.

```bash
dig @ns.domain.com domain=.com AXFR
```

### Google Dorking

Google hacking no puede faltar, podemos usar el operador `site:` para encontrar todos los subdominios que Google ha indexado.

```bash
site:dominio.com
```

### Rapid7 DNS Dataset

Rapid7 proporciona de forma pública su repositorio con el objetivo de descubrir todos los subdominios que se encuentran en Internet, aunque hace muy bien su trabajo, la lista definitiva puede no estar completa, puedes leer más sobre ella [aquí](https://github.com/rapid7/sonar/wiki/Forward-DNS).

```bash
zcat snapshop.json.gz |   
 jq -r 'if (.name | test("\.domain\.com$")) then .name else empty end'
```

### Censys.io

Buscador muy similar a Shodan, permite buscar palabras clave en certificados, y por lo tanto, descubrir nuevos subdominios.

```bash
https://censys.io/certificates?q=.domain
```

```bash
Crt.sh
```

Servicio en línea para la búsqueda de certificados emitidos por COMODO. Utiliza un conjunto de datos diferente al de Censys.

```bash
https://crt.sh/?q=%25.domain.com
```

## Ejemplos de toma de control

### Amazon

1. Ve al panel S3
2. Haz click en **Crear**.
3. Configura el nombre del bucket como nombre de dominio de origen (es decir, el dominio que desea asumir)
4. Haz click en **Siguiente** varias veces hasta terminar.
5. Abre el bucket creado.
6. Haz click en **Cargar**.
7. Selecciona el archivo que se utilizará para la PoC (archivo HTML o TXT). Recomiendo nombrarlo de manera diferente a index.html
8. En la pestaña **Permisos**, selecciona «Otorgar acceso de lectura público a este objeto».
9. Después de cargar, selecciona el archivo y haga clic en **Más -> Cambiar metadatos**.
10. Haz click en **Agregar metadatos**, seleccione Tipo de contenido y el valor debe reflejar el tipo de documento. Si es HTML, elija text/html.
11. (Opcional) Si el bucket fue configurado como un sitio web.
    1. Cambia a pestaña Propiedades.
    2. Haz click en **alojamiento web estático**.
    3. Selecciona «Usar este bucket para alojar un sitio web».
    4. Como índice, elige el archivo que subiste.
    5. Haz click en Guardar.

Si todo fue bien, la toma de control está completa. Los archivos estarán presentes en la ruta URL apropiada. Si subiste el archivo poc.html, entonces será `http://sub.example.com/poc.html`.

### GitHub

1. Ve a la página del nuevo repositorio
2. Establece el nombre del repositorio en el nombre de dominio canónico (es decir, {algo} .github.io del registro CNAME)
3. Haz click en **Crear repositorio**.
4. Sube el contenido usando git al repositorio creado.
5. Cambia a la pestaña **Configuración.**
6. En la sección de páginas de GitHub , elige la rama maestra como fuente
7. Haz click en **Guardar.**
8. Después de guardar, establece el dominio personalizado en el nombre de dominio de origen (es decir, el nombre de dominio que desea asumir)
9. Haz click en Guardar

### Heroku

1. Abre la nueva aplicación Heroku .
2. Elige el nombre y la región.
3. Sube la PoC usando git a Heroku. El proceso se describe en la pestaña Implementar .
4. Cambia a la pestaña Configuración .
5. Desplázate hasta Dominios y certificados .
6. Haz click en Agregar dominio.
7. Proporciona el nombre de dominio que desea tomar, haz click en Guardar cambios .
8. Puede tomar algún tiempo para que la configuración se propague.

### Readme.io

1. Ve al panel de control .
2. Establece el nombre del proyecto y su subdominio . El subdominio no necesita coincidir con el dominio que está intentando tomar.
3. En la barra lateral izquierda, ve a Configuración general -> Dominio personalizado .
4. Establece el dominio personalizado en el dominio que desea adquirir.
5. Haz click en Guardar .

Si has encontrado alguna vulnerabilidad de toma de control de subdominio, ¡Enhorabuena! Puede que hayas ganado **$5000.**

[![/images/guia-sobre-toma-de-control-de-subdominios/guia-sobre-toma-de-control-de-subdominios_001.png](/images/guia-sobre-toma-de-control-de-subdominios/guia-sobre-toma-de-control-de-subdominios_001.png)](/images/guia-sobre-toma-de-control-de-subdominios/guia-sobre-toma-de-control-de-subdominios_001.png)

Recompensa de $5000 en la plataforma de bugbounty Hackerone

Hasta aquí la entrada, espero sacar tiempo para ir actualizándola!!!