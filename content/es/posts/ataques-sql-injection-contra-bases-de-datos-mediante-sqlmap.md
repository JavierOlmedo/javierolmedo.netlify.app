+++
date = "2015-03-11"
title = "Ataques SQL injection contra Bases de Datos mediante SQLmap"
author = "Javier Olmedo"
toc = false
+++

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_banner.jpg](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_banner.jpg)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_banner.jpg)

Hoy en día, uno de los ataques que **más se utiliza** contra aplicaciones web son los llamados **SQL Injection,** estos ataques tienen como **objetivo** acceder a la base de datos del sitio web, y **obtener información sensible** sobre ella, como por ejemplo los **datos para administrar el sitio web, contraseñas de los usuarios, correos electrónicos, DNI, dirección e incluso números de tarjetas de crédito con los códigos de verificación, etc.**

Seguro que mas de uno de nosotros, nos hemos encontrado alguna vez sin querer (o queriendo) algún **mensaje de alerta** como este en algún sitio web, y sin darle demasiada importancia hemos seguido con lo nuestro.

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_001.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_001.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_001.png)

Ejemplo Vulnerabilidad SQL 1

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_002.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_002.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_002.png)

Ejemplo Vulnerabilidad SQL 2

Si vemos esto, podríamos decir que tenemos **altas posibilidades** de obtener datos sensibles sobre el sitio web..

Esta vulnerabilidad ocurre por un **error en la validación de las variables**.

Para ataques SQL Injection de forma automatizada, uso siempre **SQLmap**, lo puedes descargar de **[AQUI](https://github.com/sqlmapproject/sqlmap)**, como siempre las mejores herramientas son libres.

**En otra entrada** explicaré mas detenidamente como buscar webs con fallos SQLi y publicaré unos **Dorks** para ayudarnos a encontrarlas dependiendo lo que queramos obtener, pero no quiero alargar mucho esta entrada por lo que voy al grano.

Una vez localizada la web con fallo SQLi **debemos de saber como se llama la base de datos** que está manejando la web, lo haríamos de la siguiente manera (En rojo debemos de poner la URL, lo tapo por motivos de seguridad).

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_003.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_003.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_003.png)

y obtenemos el nombre de la BBDD.

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_004.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_004.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_004.png)

Obteniendo el nombre de la base de datos

Como ya sabemos el nombre de la Base de Datos, debemos de avanzar en nuestro ataque, **el siguiente paso es sacar todas las tablas de la base de datos** que nos interesa, evidentemente la primera es la mas importante, procedemos como en el anterior y ademas le añadimos el nombre de la BBDD que obtuvimos.

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_005.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_005.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_005.png)

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_006.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_006.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_006.png)

**Parece que hemos encontrado algo muy interesante,** seguimos con el ataque, ahora **vamos a por las columnas de la tabla webmaster**.

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_007.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_007.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_007.png)

Y nos muestra **como resultado 5 columnas,** de las cuales las mas importantes son usuario y contrasena.

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_008.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_008.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_008.png)

Y finalmente, el ultimo ataque para llegar hasta los datos, al final indicamos a SQLmap que saque los datos con **–dump**.

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_009.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_009.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_009.png)

[![/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_010.png](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_010.png)](/images/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap/ataques-sql-injection-contra-bases-de-datos-mediante-sqlmap_010.png)

Ya tenemos los datos de administración del sitio, una vez **comprobada y verificada la vulnerabilidad,** nos ponemos en contacto con el admin del sitio para que corrija el fallo.

Terminó la entrada de hoy, **no olvides compartir.**

Gracias y Happy Hack!!!!