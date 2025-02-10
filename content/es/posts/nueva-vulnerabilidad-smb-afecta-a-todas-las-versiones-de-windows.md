+++
date = "2015-04-14"
title = "Nueva vulnerabilidad SMB afecta a todas las versiones de Windows"
author = "Javier Olmedo"
+++

[![/images/nueva-vulnerabilidad-smb-afecta-a-todas-las-versiones-de-windows/nueva-vulnerabilidad-smb-afecta-a-todas-las-versiones-de-windows_banner.jpeg](/images/nueva-vulnerabilidad-smb-afecta-a-todas-las-versiones-de-windows/nueva-vulnerabilidad-smb-afecta-a-todas-las-versiones-de-windows_banner.jpeg)](/images/nueva-vulnerabilidad-smb-afecta-a-todas-las-versiones-de-windows/nueva-vulnerabilidad-smb-afecta-a-todas-las-versiones-de-windows_banner.jpeg)

[Nuevo fallo al protocolo SMB](https://www.kb.cert.org/vuls/id/672268) **(Server Message Block)** que afecta a todas las versiones de Windows, esta vulnerabilidad ha sido clasificada como grave. Dicha vulnerabilidad **permite que un atacante pueda robar las credenciales de servicios del usuario**.

El error está relacionado con la forma en que Windows y otro software manejan algunas peticiones HTTP, y los investigadores dicen que **afecta a una amplia gama de aplicaciones, incluyendo iTunes y Adobe Flash, algunos clientes de GitHub, Oracle y AVG Anti-virus**. La vulnerabilidad, divulgada el lunes por los investigadores en Cylance, es una extensión de la investigación realizada por Aaron Spangler hace casi 20 años, conocida como “redirect to SMB”. Esta debilidad puede permitir a un atacante forzar a las víctimas para que se autentiquen mediante [hijacking](http://www.estacion-informatica.com/2013/03/evita-hijacking-en-tu-cuenta.html) en un servidor controlado por el atacante, mediante ataques [man-in-the-middle](http://www.estacion-informatica.com/2015/03/lenovo-no-era-el-unico-con-superfish.html) y luego enviarlos a servidores maliciosos SMB. Con lo que ello supone, robo de credenciales.

Microsoft todavía tiene que lanzar un parche para arreglar la redirección a la vulnerabilidad SMB. **La solución más simple es bloquear el tráfico saliente de TCP 139 y 445 TCP en el firewall**.

Microsoft no resolvió el problema reportado por Aaron Spangler en 1997. Esperamos que esta nueva investigación obligue a Microsoft a reconsiderare esta [vulnerabilidad](http://www.estacion-informatica.com/2015/02/aplicaciones-web-vulnerables.html) y deshabilite la autenticación con servidores SMB no confiables.

Fuentes: https://threatpost.com/new-smb-flaw-affects-all-versions-of-windows/112134?hootPostID=e1a6c6b3834c6b202f75079d9f3c3f56

**No seáis malos.**