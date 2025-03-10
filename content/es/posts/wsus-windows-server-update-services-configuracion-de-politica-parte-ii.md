+++
date = "2017-10-27"
title = "WSUS (Windows Server Update Services): Configuración de política - Parte II"
author = "Javier Olmedo"
toc = false
+++

Seguimos con la **segunda parte sobre WSUS (Windows Server Update Services)**, en esta entrada vamos a ver como **crear una política en el dominio para aplicar a los equipos** clientes, si no has empezado esta serie de entradas, [puedes ver la primera parte](https://hackpuntes.com/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/).

## Crear política para equipos clientes

1. Abrimos **“Administración de directivas de grupo”**, en **“Herramientas”**.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_001.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_001.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_001.png)

2. En mi caso, **tengo creada una unidad organizativa** dentro de **«Usuarios y equipos del Active directory»** para ubicar todos los equipos clientes del dominio, **«Click derecho»** y **«Crear una GPO en este dominio y vincularlo aquí»**, ponemos un nombre y aceptamos.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_002.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_002.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_002.png)

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_003.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_003.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_003.png)

3. Vamos a editar la política para configurarla correctamente, **«Click derecho»** sobre ella y **«Editar»**

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_004.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_004.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_004.png)

4. Ahora en **«Configuración del equipo» –> «Directivas» –> «Plantillas administrativas» –> «Componentes de Windows» –> «Windows Update»** se encuentran las **5 directivas básicas que tendremos que modificar**, modificad las demás en función de vuestras necesidades.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_005.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_005.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_005.png)

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_006.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_006.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_006.png)

5.1 Directiva **“Configurar Actualizaciones automáticas”**

Esta directiva nos permite **descargar automáticamente las actualizaciones** que estén disponibles en el servidor WSUS y programarlas para que se instalen **todos los días a las 14:00h** (hora en la cual sabemos que están encendidos los equipos y pueden instalar actualizaciones sin perjudicar a ningún usuario)

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_007.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_007.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_007.png)

5.2 Directiva **“Especificar la ubicación de Windows Update en la Intranet”**

En esta directiva **vamos a hacer un poco más de hincapié**, previamente **vamos a crear un registro en el DNS** para posteriormente configurarlo en la directiva, esto lo hago porque es preferible configurarlo con un registro DNS a ponerlo con el nombre del servidor, con esto ganamos que **en el caso de migrar el servicio de WSUS a otro servidor, no sea necesario modificar las políticas**, únicamente cambiaríamos la IP en el registro DNS.

Nos vamos a **«Administrador de DNS» -> «Zona de búsqueda directa»**, marcamos nuestro dominio y **«Click derecho» -> «Host nuevo (A o AAAA)»**

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_008.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_008.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_008.png)

Ponemos un nombre y añadimos la IP del servidor, marcamos la opción **«Crear registro del puntero (PRT) asociado»**

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_009.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_009.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_009.png)

Para **comprobar que el registro está correctamente creado**, nos vamos a cualquier equipo del dominio y tecleamos en la consola de comando **nslookup [nombre_registro]**, si nos devuelve correctamente la IP estará todo preparado para configurar la política.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_010.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_010.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_010.png)

```bash
nsllkup [nombre_registro]
```

Quedaría de esta manera configurada la política, **añadimos el puerto 8530** que por defecto es el que escucha el servicio de WSUS (8531 para HTTPS)

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_011.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_011.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_011.png)

5.3 Directiva **“Frecuencia de detección de Actualizaciones automáticas”**
En esta directiva vamos a configurar el **intervalo en el cual el equipo cliente se pondrá en contacto con el servidor WSUS** para comprobar si existen actualizaciones para él, 8 horas creo que es el intervalo más adecuado.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_012.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_012.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_012.png)

5.4 Directiva **“Volver a programar las instalaciones programadas de Actualizaciones automáticas”**

A veces, el equipo se encuentra apagado o por algún motivo no puede conectarse con el servidor WSUS para recibir actualizaciones, en esta política, lo que vamos a establecer es el **tiempo que tardará el equipo en volver a reprogramar la tarea de sincronización**, por ejemplo, si el equipo está apagado y tenemos configurado esta política en 5 minutos, cuando encienda tardará ese tiempo en reprogramar la tarea de volver a ponerse en contacto con el servidor WSUS.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_013.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_013.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_013.png)

5.5 Directiva **“No reiniciar automáticamente con usuarios que hayan iniciado sesión”**
De esta política queda bastante claro todo, **no reiniciar los equipos si hay usuario trabajando con los ellos**, con esto evitaremos que se pierdan documentos/información.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_014.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_014.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_014.png)

## Resumen de la política creada

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_015.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_015.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_015.png)

Para que las políticas se hagan efectivas en los equipos, tenemos que reiniciarlo o **ejecutar el comando gpupdate /force**

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_016.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_016.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_016.png)

```bash
gpupdate /force
```

Esto lo podríamos hacer con PowerShell, para no tener que estar ejecutándolo equipo por equipo, en otras entradas hablaremos sobre administración de equipos con PowerShell.

Ya tendríamos todo, podemos ver los 2 equipos con todas las actualizaciones al día.

[![/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_017.png](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_017.png)](/images/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/wsus-windows-server-update-services-configuracion-de-politica-parte-ii_017.png)

En la próxima y última entrada veremos como trabajar con los reportes y uso básico de la consola de WSUS.

Saludos,

Gracias.