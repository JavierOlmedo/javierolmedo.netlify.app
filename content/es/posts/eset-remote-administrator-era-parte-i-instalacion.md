+++
date = "2017-11-08"
title = "ESET Remote Administrator (ERA) - Parte I: Instalación"
author = "Javier Olmedo"
toc = false
+++

Ya hemos visto en [entradas anteriores](https://hackpuntes.com/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/) como mantener nuestros equipos del dominio con las últimas actualizaciones críticas y parches de seguridad para mantenerlos más seguros, otra de las tareas prioritarias que debemos de hacer, es **proveer de una solución antivirus a los equipos**, en esta entrada, veremos cómo instalar **ERA (ESET Remote Administrator)** en un controlador de dominio con Windows Server 2016.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_001.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_001.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_001.png)

## ¿Qué es ESET Remote Administrator (ERA)?

Es una **herramienta** que permite a los administradores de sistemas **gestionar los equipos del dominio que ejecutan productos ESET**, con esto conseguimos tener centralizados todos los incidentes de seguridad que ocurren en los equipos y gestionarlos desde un único panel, lo que también se traduce en una mayor **facilidad y agilidad** en la configuración de la seguridad de la empresa.

## Pasos previos a su instalación

Si vamos a proceder a instalarlo en un controlador de dominio, como en mi caso, y ya tenemos SQL Express instalado, tenemos que hacer **2 cambios** en la configuración de SQL Server.

### Cambio 1 - Habilitar TCP/IP y establecer el puerto de escucha 1433

Para ello abrimos **“Administrador de configuración de SQL Server 2016”**, podemos buscarlo en **“Todos los programas”** si no lo encontramos.

Si nos fijamos, aparece deshabilitado por defecto, **“Click”** con el **“botón derecho”**, **“Propiedades”** y en la pestaña **“Protocolo”** lo habilitamos, después tenemos que especificar el puerto en la pestaña **“Direcciones IP”**, estableciendo el **puerto 1433**.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_002.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_002.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_002.png)

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_003.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_003.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_003.png)

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_004.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_004.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_004.png)

Debemos de **reiniciar el servicio** de SQL Server para que se apliquen los cambios.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_005.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_005.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_005.png)

### Cambio 2 - Habilitar autenticación de Windows en SQL Server 2016

Este paso es opcional, aunque lo recomiendo para poder autenticarse con el usuario administrador del dominio, para ello, abrimos **“Microsoft SQL Server Management Studio”**, **“Click”** con el **“botón derecho”** y en **“Propiedades”**.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_006.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_006.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_006.png)

En la pestaña **“Security”** marcamos la opción **“SQL Server and Windows Authentication mode»**.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_007.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_007.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_007.png)

**NOTA:** Debemos de reiniciar de nuevo el servicio de SQL Server para que se apliquen los cambios.

Otra configuración a tener en cuenta antes de proceder con la instalación, es tener **Java instalado en el sistema**, podemos descargarlo desde [aquí](https://www.java.com/es/download/).

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_008.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_008.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_008.png)

## Instalación de ESET Remote Administrator (ERA)

1. Descargamos el paquete **“Todo en uno”** de ERA [desde la web de ESET](https://descargas.eset.es/empresas)

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_009.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_009.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_009.png)

2. Descomprimimos el paquete descargado y ejecutamos el **Setup.exe**, elegimos el idioma y aceptamos el contrato de licencia, en la parte de componentes a instalar **podemos dejarlo por defecto**, pero en mi caso, desmarcaré Microsoft SQL Server Express, ya que lo tengo instalado.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_010.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_010.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_010.png)

3. En la conexión con la base de datos, **habilitaré la autenticación de Windows** y dejaré los demás campos por defecto.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_011.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_011.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_011.png)

4. Puesto que es la primera vez que instalamos ERA, **creamos un usuario nuevo**.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_012.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_012.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_012.png)

5. En esta ventana, configuraremos la **contraseña que usaremos para acceder a ERA** vía web.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_013.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_013.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_013.png)

6. En los siguientes pasos nos pedirá **configurar el certificado y activar con licencia**, en la ventana de certificado existen **2 campos obligatorios** que son **“Nombre común de la autoridad”** y **“Validez del certificado”**, opcionalmente podemos crear una contraseña, nos aseguraremos de no perderla ya que todos los equipos que lo utilicen la requerirán.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_014.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_014.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_014.png)

7. Si todo ha ido bien, al final veremos la **URL** con el acceso a nuestra herramienta.

[![/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_015.png](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_015.png)](/images/eset-remote-administrator-era-parte-i-instalacion/eset-remote-administrator-era-parte-i-instalacion_015.png)

En la próxima entrada veremos cómo **instalar en los equipos el Agente de ERA mediante una GPO** para administrarlos.

Saludos,

Gracias.