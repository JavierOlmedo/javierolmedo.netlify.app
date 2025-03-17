+++
date = "2017-11-14"
title = "ESET Remote Administrator (ERA) - Parte II: Instalar Agente ERA mediante tarea"
author = "Javier Olmedo"
toc = false
+++

Buenas a todos, en esta entrada sobre ESET Remote Administrator (ERA) vamos a ver como **instalar el Agente ERA en los equipos mediante una tarea**, a través del agente, podremos administrar los equipos, por ejemplo, después de tenerlo configurado en los equipos comenzaremos a instalar la solución antivirus ESET EndPoint Security **mediante otra tarea y de manera desatendida**.

En mi caso tengo instalado ERA en la siguiente dirección [https://192.168.100.10/era](https://192.168.100.10/era)

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_001.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_001.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_001.png)

Si vamos a la pestaña **“Ordenadores” >> “Equipos”** podemos ver como su estado aparece como **“No administrado”**, aparecen de esta manera porque ERA ha cogido los nombres de los equipos que pertenecen al dominio, pero aún no puede administrarlo porque no tienen el agente ERA instalado.

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_002.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_002.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_002.png)

Para implementar el agente, nos vamos a la pestaña **“Vínculos rápidos”** situado en la parte superior derecha, despues buscaremos la opción **“Instalar o implementar agente ERA”**

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_003.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_003.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_003.png)

En esta parte, vamos a crear una tarea, buscamos la opción **“Crear tarea”** dentro de **“Instalación del agente mediante una tarea del servidor”**.

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_004.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_004.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_004.png)

En el apartado **“Básico”** ponemos nombre a la tarea y descripción, importante marcar las casillas **“Ejecutar tarea inmediatamente después de finalizar”** y **“Configurar activador”**

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_005.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_005.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_005.png)

En el apartado **“Configuración”** tenemos que **indicar los destinos** (los 3 equipos clientes), el **nombre del servidor** (En mi caso la IP 192.168.100.10), un **usuario con permisos de instalación**, el **certificado ERA** que configuramos al principio **y su contraseña**, nos quedaría de la siguiente manera:

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_006.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_006.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_006.png)

En el apartado **“Desencadenador”** configuraremos cuando se ejecutará la tarea, lo vamos a planificar para que se ejecute **diariamente y sin fin**, esto lo hacemos por si hay equipos apagados o sin red, una vez terminado, haremos **“Click”** en **“Finalizar”**

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_007.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_007.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_007.png)

Si accedemos al apartado **“Admin” >> “Tareas del servidor” >> “Todos los tipos de tareas”** podemos ver como la tarea ha sido iniciada.

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_008.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_008.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_008.png)

En el apartado **“Detalles de ejecución”** podemos verificar si la tarea se ha ejecutado en el equipo.

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_009.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_009.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_009.png)

**NOTA:** Si la tarea no se ejecuta en los equipos, verificad si responden a ping, puede que el Firewall esté cortando la conexión.

Una vez terminada la tarea, podemos ir a un equipo y confirmar si está instalado el agente en **“Programas y características”**.

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_010.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_010.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_010.png)

Ahora, el estado de los equipos debería aparecer con un **“Check verde”** que indica que está administrado.

[![/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_011.png](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_011.png)](/images/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea/eset-remote-administrator-era-parte-ii-instalar-agente-era-mediante-tarea_011.png)

Hasta aquí la segunda entrada, en el próximo post veremos como instalar el antivirus ESET EndPoint Security de manera desatendida, si no viste la primera entrada sobre ERA, te la dejo por [aquí](https://hackpuntes.com/eset-remote-administrator-era-parte-i-instalacion/).

Saludos!!