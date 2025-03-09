+++
date = "2017-10-24"
title = "WSUS (Windows Server Update Services): Instalación y configuración - Parte I"
author = "Javier Olmedo"
toc = false
+++

Buenas a todos, voy a comenzar una **serie de entradas en el blog** dónde explicaré como **implementar WSUS** (Windows Server Update Services) en un dominio con Windows Server 2016 para mantener nuestros equipos de la empresa actualizados y más seguros, en un principio, creo que con **3 entradas podré explicar lo necesario** para tener este rol completamente funcional.

En esta entrada os explicaré un poco en qué consiste WSUS, su **instalación y configuración**.

La segunda entrada consistirá en la **configuración de la política** para los equipos clientes y la tercera sobre **reportes**.

¡Comencemos!

## ¿Qué es WSUS?

**WSUS** (Windows Server Update Services) **es un rol para sistemas operativos Windows Server** que permite a los administradores disponer de un **sistema centralizado de actualizaciones para todos los equipos** dentro de nuestra empresa/dominio.

A través de WSUS es posible gestionar completamente la distribución de las **últimas actualizaciones publicadas por Microsoft** para todos sus productos, así como de los **parches de seguridad más recientes**, algo que muchos administradores pasan por alto y no dan importancia.

Cabe destacar que WSUS **solamente está destinado para productos de Microsoft**, por lo que **no se podrá utilizar para instalar actualizaciones de terceros** (Java, Adobe, Antivirus, etc).

En definitiva, WSUS **nos permitirá ahorrar tiempo y mejorar la seguridad** de la red.

## ¿Cómo funciona WSUS?

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_001.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_001.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_001.png)

Una vez implementado el rol en el servidor, WSUS **descargará desde los servidores de Microsoft todas las actualizaciones que haya disponible** en función de los productos que hayamos configurado en su instalación, en la imagen anterior podemos ver cómo distribuye las actualizaciones entre los equipos vinculados al Active Directory.

## Pasos a seguir para su instalación

Antes de empezar con la instalación, me gustaría comentaros que **Microsoft recomienda no instalar WSUS en el controlador de dominio**, en la práctica puedo deciros que no he tenido ningún problema hasta el momento en este aspecto, por lo tanto, si disponéis de varios servidores intentad instalarlo en uno que no sea controlador de dominio.

Os adjunto un link con más información [aquí]()

1. Abrimos el **Administrador del servidor**.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_002.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_002.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_002.png)

2. Debemos agregar el rol, nos vamos a **Administrar –> Agregar roles y características**.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_003.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_003.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_003.png)

3. Marcamos para instalar el rol **“Windows Server Update Services”**, ubicado en la parte inferior.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_004.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_004.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_004.png)

4. Nos aparecerá una ventana con las características requeridas que debemos de agregar, marcamos también la casilla **“Incluir herramientas de administración (si es aplicable)”**.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_005.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_005.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_005.png)

5. En la siguiente ventana, **añadimos características de .NET Framework 3.5 (Incluye .NET Framework 2.0)**, esto lo usaremos para generar reportes desde la consola de WSUS, lo veremos más tarde en la tercera entrada.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_006.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_006.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_006.png)

6. En los servicios del rol, **marcaremos las 2 primeras**, si disponemos de base de datos SQL, es recomendado marcarla también, pues podremos trabajar mejor desde PowerShell.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_007.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_007.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_007.png)

7. En este paso, indicamos la ruta en la cual descargará y almacenará las actualizaciones, en mi caso, tengo un disco duro de 500GB en la unidad W: para usarlo únicamente para este servicio.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_008.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_008.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_008.png)

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_009.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_009.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_009.png)

8. Instalamos todo lo que hemos configurado hasta ahora, esperamos a que termine.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_010.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_010.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_010.png)

9. Una vez termine de instalar, cerramos la ventana y vamos a **Herramientas –> Windows Server Update Services**, WUSUS nos pedirá realizar el proceso de post-instalación, verificamos que la ruta de acceso es la misma que configuramos en el anterior paso anterior y marcamos la casilla **“Almacenar actualizaciones localmente”**, después ejecutamos.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_011.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_011.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_011.png)

10. **Este paso hay que tenerlo en cuenta**, es posible que haya que configurar alguna regla en el firewall para permitir que los equipos se conecten, de momento no vamos a cambiar nada, pero **si posteriormente tenemos problemas para que los equipos se conecten, sabemos que puede ser por el firewall**.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_012.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_012.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_012.png)

11. Paso de elegir el servidor de actualizaciones, tenemos 2 opciones, sincronizar con los servidores de Microsoft Update o sincronizar con otro WSUS, como es el primero y único que tendremos, **dejamos marcada la primera opción**.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_013.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_013.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_013.png)

12. Todo preparado, iniciamos la conexión, **este paso puede llegar a tarda hasta 30 minutos**, toma un café tranquilamente 😊

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_014.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_014.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_014.png)

13. El paso de elegir idiomas, aquí marcamos los que necesitemos, por ejemplo, si tenemos un Windows o versión de Office con otro idioma, marcamos los que correspondan, por defecto dejaría marcado siempre Inglés.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_015.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_015.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_015.png)

14. **Mucho cuidado en este paso**, es importante **marcar sólo los productos necesarios que queremos actualizar de manera desatendida**, por ejemplo, si marcamos actualización de SQL o drivers, podemos tener después problemas de compatibilidad con software de terceros, **ese tipo de actualizaciones es mejor hacerlas de manera manual**, yo solamente marcaré actualizacions de Windows 10 y Windows 7.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_016.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_016.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_016.png)

15. Nuestro propósito con WSUS es **mantener actualizados los sistemas en cuanto a parches de seguridad y actualizaciones críticas**, sólo dejaré estas opciones marcadas.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_017.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_017.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_017.png)

16. No tiene mucho sentido que tengamos que sincronizar WSUS con los servidores de Microsoft Update de manera manual, ya que nos interesa que este servicio sea lo más desatentido posbile, por lo tanto, **lo configuraré para que actualice todos los días a las 3 de la madrugrada**, esa hora la he elegido para no saturar la red en caso que descargue actualizaciones.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_018.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_018.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_018.png)

17. Hora de la primera sincronización, marcamos la siguiente casilla y **click en siguiente**

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_019.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_019.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_019.png)

18. En el último paso del asistente de configuración, nos indica algunas configuraciones recomendadas, **sería interesante implementar SSL**, pero eso lo dejaremos para más adelante, quizás en una cuarta parte de esta serie de entradas.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_020.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_020.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_020.png)

19. Al iniciar WSUS veremos lo siguiente, vamos a realizar una sincronización manual, **de momento no veremos ningún equipo, ya que falta de aplicar las políticas** que veremos en la siguiente entrada.

[![/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_021.png](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_021.png)](/images/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i/wsus-windows-server-update-services-instalacion-y-configuracion-parte-i_021.png)

Hasta aquí la parte I sobre instalación y configuración de WSUS, en la próxima entrada veremos cómo crear una política para los equipos clientes del dominio.

Un Saludo!!

Gracias.