+++
date = "2015-03-29"
title = "Accediendo al sistema de control de las gasolineras"
author = "Javier Olmedo"
+++

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_banner.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_banner.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_banner.png)

Ayer por la noche estaba leyendo un articulo de **Amador Aparicio de la Fuente** en el que aseguraba que los conversores de serie a Ethernet del tipo **GC-NET2 32-DTE** que **utilizan las gasolineras españolas** para controlar los tanques de combustible **eran vulnerables** (más que vulnerables, mala securización), nada más leer el artículo algo me decía que **ya había leído** yo eso en algún sitio, hasta que recordé y ví el siguiente [tweet](https://x.com/cybergibbons/status/558551285776273408), que en un principio no dí mucha importancia ni me dio por investigar.

El **conversor** del cual hablamos es el siguiente:

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_001.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_001.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_001.png)

Este conversor **envia datos** a un servidor y **monitoriza** el estado de los tanques de combustible: **tipo de combustible almacenado, temperatura del combustible dentro del tanque, cantidad de combustible que queda, porcentaje de agua** (algo que me llamo la atención, agua en el tanque del combustible) etc…

Mediante este sistema, **los administradores** de las gasolineras **saben** cuando pedir combustible a la central, verificar si algo no va bien dentro de los tanques (temperatura, fugas, etc) y alarmar en caso que fuese necesario y tomar medidas.

Pero, **¿Que pasaría si alguien pudiese acceder y cambiar los datos del sistema a su antojo?, ¿Y si cambiase la temperatura del tanque o lo hiciese parecer que esta lleno cuando en realidad está vacío en plena operación salida de Semana Santa?**

Todas estas preguntas pasaron por mi cabeza, y me picó la curiosidad, **lo primero que hice fue buscar gasolineras** que pudiesen ser vulnerables, me fuí a **SHODAN** (buscador que registra dispositivos conectados a Internet con sus servicios) y filtré la búsqueda por **nacionalidad (España) y puerto (10001, el utilizado por el conversor GC-NET2 32-DTE)**.

```bash
country:es port:10001
```

Los primeros resultados, ya me hacían creer que se iba a poder entrar **hasta la cocina**.

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_002.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_002.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_002.png)

Fuí a la terminal y utilicé **Telnet** para intentar entrar al servidor y …..

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_003.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_003.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_003.png)

**Ya estaba dentro del servidor**, ¿Así de fácil?, cuesta creer que algunas gasolineras tenían **deshabilitadas** las contraseñas, y en otras bastaba con leer el manual del fabricante para dar con el **usuario/contraseña** por defecto para acceder al sistema, una vez dentro solo tenía que poner los **códigos necesarios** para gestionarlo, que también lo saqué del manual que podéis encontrar [AQUÍ](https://www.google.es/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0CCEQFjAA&url=http%3A%2F%2Fwww.veeder.com%2Fgold%2Fdownload.cfm%3Fdoc_id%3D4438&ei=5vIVVYSoK4vU7AaagIG4DQ&usg=AFQjCNEq39keelmdvX3Jz_IauT0TtynL6g&sig2=oTy5L31-hVR-N3ofJ8aHrA&bvm=bv.89381419,d.ZGU&cad=rja), los códigos necesarios están en la **página 17**, son los siguientes:

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_004.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_004.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_004.png)

Con toda esta información ya podía **administrar los tanques de combustible** de la gasolinera, evidentemente **no toqué nada**, informé a la gasolinera del grave fallo (varias de sus estaciones eran vulnerables) y los **posibles riesgos** que podrían tener si alguien malintencionadamente modificara los datos. Muestro algunas capturas:

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_005.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_005.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_005.png)

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_006.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_006.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_006.png)

[![/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_007.png](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_007.png)](/images/accediendo-al-sistema-de-control-de-las-gasolineras/accediendo-al-sistema-de-control-de-las-gasolineras_007.png)

La última imagen muestra la existencia de exceso de agua en el tanque, algo que podría estropear el motor de nuestros vehículos.

Con esto queda mostrado la **poca seguridad** de la que disponen las estaciones, algo que podría poner en **riesgo nuestra seguridad**.

Un Saludo a todos.