+++
date = "2017-11-02"
title = "WSUS (Windows Server Update Services): Reportes y uso - Parte III"
author = "Javier Olmedo"
toc = false
+++

Última entrada sobre WSUS, esta vez **hablaremos sobre el uso de la consola y la generación de reportes**, si aún no has leído las entradas anteriores, te dejo por aquí la [primera entrada](https://hackpuntes.com/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/) dónde hablo sobre la instalación y la [segunda entrada](https://hackpuntes.com/wsus-windows-server-update-services-configuracion-de-politica-parte-ii/) sobre la creación de una política.

Lo primero que nos encontramos al intentar generar un reporte con la consola de WSUS, es un error de una característica no disponible, en concreto **“Microsoft Report Viewer 2012 Redistributalbe”**.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_001.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_001.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_001.png)

Si pulsamos sobre el enlace, nos llevará a la **URL de descarga**, procedemos a descargarlo.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_002.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_002.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_002.png)

Al instalar el paquete **“Microsoft Report Viewer 2012 Redistributalbe”**, nos puede aparecer el siguiente error, que nos indica que falta **“Microsoft System CLR Types for SQL Server 2012”**.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_003.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_003.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_003.png)

Para descargar este requisito, visitamos la siguiente URL – https://www.microsoft.com/es-ES/download/details.aspx?id=35580 y descargamos el paquete llamado **“SQLSysClrTypes.msi”**

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_004.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_004.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_004.png)

Ahora sí podemos proceder a la instalación, ya tenemos los **2 instaladores necesarios** para hacer funcionar los reportes.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_005.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_005.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_005.png)

Dentro del panel de WSUS, podemos diferenciar **5 apartados** para organizar las actualizaciones de los equipos, que son **“Actualizaciones”, “Equipos”, “Sincronizaciones”, “Informes” y “Opciones”**, vamos a explicar de manera breve estas opciones.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_006.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_006.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_006.png)

## Actualizaciones

Desde aquí, **podemos comprobar todas las actualizaciones** que existen para nuestros equipos, las actualizaciones **tienen 3 estados**, que son **“Sin aprobar”, “Aprobada” y “Rechazada”**.

Como en nuestro caso, **solo habíamos configurado actualizaciones críticas y de seguridad**, tendríamos que tener todas **«Aprobadas»**, en el apartado **“Opciones”** veremos cómo podemos configurarlo para aprobar estas actualizaciones de manera automática.

Si filtramos por actualizaciones **“Sin aprobar”** veremos que no tenemos ninguna, en el caso que tuviéramos alguna **“Sin aprobar”** o **“Rechazada”**, pulsaríamos con el **“Botón derecho”** y en **“Aprobar”**.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_007.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_007.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_007.png)

Podemos ver actualizaciones rechazadas porque **es posible que hayan salido otras posteriores que solucionen o reemplacen a estas**.

## Equipos

Este panel es importante, puesto que desde aquí **podemos ver todos los equipos que están reportando correctamente** y dividirlos por grupos, por ejemplo, por sistema operativo en caso de querer tenerlos organizados, también podemos ver si disponen de todas las actualizaciones instaladas.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_008.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_008.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_008.png)

## Sincronizaciones

Desde aquí podemos ver **cuándo se ha sincronizado WSUS con los servidores de Windows Update**, podemos hacer una sincronización manual desde el panel derecho.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_009.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_009.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_009.png)

## Informes

**Parte muy importante**, tener informes sobre las actualizaciones que se instalan en los equipos es de vital importancia para la seguridad, **en caso de un incidente**, disponer de informes nos puede ayudar a resolver porque ha ocurrido.

En mi caso, voy a crear una carpeta en la partición de disco que tengo dedicada solamente a WSUS llamada **“Reportes”** y a su vez, esta contendrá informes diarios separados en carpetas por mes y año, ejemplo:

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_010.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_010.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_010.png)

Vamos a generar un reporte de prueba, por ejemplo, un reporte completo de todos las actualizaciones y equipos.

Nos vamos a **Informes -> Estado detallado del equipo**

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_011.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_011.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_011.png)

Las opciones por defecto son **las más recomendadas** para saber que equipos no tienen ciertas actualizaciones, como en mi caso todos los equipos tienen todas las actualizaciones, voy a modificar la parte de **“Incluir equipos que tengan un estado de”** y añadiré **“Instalada/no aplicable”**, después ejecutamos el informe.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_012.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_012.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_012.png)

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_013.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_013.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_013.png)

Ahora podemos ver un informe completo, podemos **guardarlo como PDF**, os recomiendo que siempre tengáis una nomenclatura para guardar archivos, por ejemplo llamadlos **[FECHA][DESCRIPCIONBREVE]**, os adjunto una muestra:

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_014.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_014.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_014.png)

NOTA: Desde nuestro **lector PDF podemos buscar el KB** (Microsoft Knowledge Base)

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_015.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_015.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_015.png)

## Opciones

Desde este panel podemos acceder a la **configuración de WSUS**, por ejemplo, para configurar las actualizaciones de ciertos productos, aprobarlas automáticamente o enviar notificaciones por mail.
**Aprobaciones automaticas**, por defecto, dado que las actualizaciones que tenemos son importantes, nos aparece una regla de auto aprobación.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_016.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_016.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_016.png)

**Productos y clasificaciones**, nos permite configurar de que productos de Microsoft queremos recibir actualizaciones, vamos a configurar para recibir actualizaciones de Microsoft Office 2016 a partir de ahora.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_017.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_017.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_017.png)

**Notificaciones por correo electrónico**, nos permite recibir informes diarios en nuestro mail, está opción es recomendable tenerla configurada.

[![/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_018.png](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_018.png)](/images/wsus-windows-server-update-services-reportes-y-uso-parte-iii/wsus-windows-server-update-services-reportes-y-uso-parte-iii_018.png)

Con esta entrada **terminamos la serie sobre WSUS (Windows Server Update Services)**, nos vemos en la próxima.

Saludos!!