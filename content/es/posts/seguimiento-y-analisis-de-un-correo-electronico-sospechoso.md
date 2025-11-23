+++
date = "2019-12-23"
title = "Seguimiento y análisis de un correo electrónico sospechoso"
author = "Miguel Garrido"
toc = false
+++

En este post vamos a intentar explicar de manera resumida cómo se puede realizar un seguimiento de un correo electrónico sospechoso (Este caso es real y se han anonimizado todos los datos).

Para llevar a cabo este laboratorio, se han utilizado las siguientes herramientas:

MxToolboox (Analyzer Headers)

Whois

Microsoft Outlook

Buscadores de Internet

Ahora vamos a explicar lo ocurrido.

Un cliente se pone en contacto con nosotros, indicando que ha recibido un correo sospechoso, por lo que le solicitamos que envíe adjunto el correo original (formato .msg).

Antes de nada, es recomendable que cualquier archivo que se vaya a revisar, se analice con diversos antivirus o herramientas online, por ejemplo, [https://www.virustotal.com/](https://www.virustotal.com/). Aunque esto ayudaría a evitar una posible infección, se recomienda que la apertura de este tipo de archivos se haga en máquinas totalmente aisladas.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_001.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_001.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_001.png)

Comprobación del mensaje en virustotal

La primera fase del análisis es realizar una vista previa del correo. La empresa está formada por diversas delegaciones en el mundo y en este caso, el correo está escrito en portugués y dirigido a uno de los usuarios de la empresa. Revisando en detalle el contacto, se puede apreciar que el remitente real era <secure.operation@yyy.com>. Tal y como se muestra en la imagen, la persona que realizó el envío de ese mensaje, intentó suplantar la identidad de Luis xxxxx.<Luis.zzz@xxx.pt> poniendo esos datos como nombre del emisor para intentar engañar al usuario.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_002.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_002.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_002.png)

Detalle del remitente sospechoso

Se revisó el correo electrónico por completo para analizar el resto de los datos y vínculos asociados al mensaje. Los únicos datos aparentemente importantes son la redirección a una web asociada a servicios fiscales y un enlace indicando “hagan clic para reportar si este correo es spam”.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_003.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_003.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_003.png)

Resto del mensaje

La segunda fase del análisis es realizar un análisis completo de la cabecera del correo electrónico. Para ello, se debe copiar todo el código del mensaje y utilizar alguna herramienta online como [https://mxtoolbox.com/EmailHeaders.aspx](https://mxtoolbox.com/EmailHeaders.aspx). Esta herramienta nos ayudará a visualizar que saltos ha realizado el mensaje hasta llegar al destinatario. Esta web también nos devuelve otros valores como SPF, DKIM y DMARC que explicaré en otros posts.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_004.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_004.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_004.png)

Análisis de cabeceras del correo electrónico

Tras el resultado obtenido, se determina que el correo electrónico se envió desde la web del servidor de correo proporcionado por el proveedor de servicio. <https://betawebmail.combell.com>.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_005.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_005.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_005.png)

Webmail del proveedor (origen del mensaje)

Analizando cada uno de los saltos realizados por el correo electrónico, se detectó que uno de los servidores de correo por donde paso dicho mensaje, se encontraba en varias blacklists.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_006.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_006.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_006.png)

Blacklist

Se consultó el Whois del dominio sospechoso que envió el correo.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_007.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_007.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_007.png)

Registro del dominio sospechoso

Tras realizar la consulta, obtenemos los datos del registro del dominio. Utilizando el correo electrónico del registro e investigando un poco más, procedemos a buscar cuantos dominios tenía asociados, encontrando cuatro en total.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_008.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_008.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_008.png)

Dominios vinculados al correo electrónico

Como curiosidad, cabe destacar que el correo que se utilizó para registrar estos dominios era de protonmail, que tiene la característica principal de preservar la privacidad y el anonimato. Otro indicador más de que el emisor intenta ocultar su identidad.

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_009.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_009.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_009.png)

Servicio de correo electrónico cifrado

Buscando más información y utilizando de nuevo técnicas OSINT (fuentes abiertas), todos estos dominios se dieron de alta el mismo día y en una franja horaria muy corta (entre las 9:30 y las 12h).

[![/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_010.png](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_010.png)](/images/seguimiento-y-analisis-de-un-correo-electronico-sospechoso/seguimiento-y-analisis-de-un-correo-electronico-sospechoso_010.png)

Todos los dominios tenían webs relacionadas con temas fiscales para intentar ofrecer más credibilidad al ataque y con nombres similares a webs legítimas que eran “cercanas” a la empresa que recibió el ataque.

Para finalizar este post, hemos enumerado una serie de conclusiones en función de los datos obtenidos.

• Existe un emisor que se presupone que realiza una **suplantación** de **identidad**. El sospechoso usa una cuenta <secure.operation@yyy.com>, para intentar realizar una suplantación de identidad sobre Luis < luis.zzz@xxx.pt> poniendo el nombre junto al correo en el nombre de la cuenta para engañar al usuario. En el correo indica que se ponga en copia Carla <carla.hhh@jjj.com>, la cual forma parte de otro dominio del sospechoso, registrado por la misma persona.

• En el correo se encontró una configuración activa “**Misdirected**”. No se puede determinar si es un problema de configuración de servidores por parte del proveedor, pero sí destacarlo como valor añadido al ataque.

• El sospechoso dio de alta **cuatro dominios a su nombre con una cuenta de correo que tenía como característica el anonimato y la privacidad**. Esto podría tener relación con el ataque, al intentar ocultar su identidad/privacidad. Todos ellos relacionados entre sí por la “actividad” a la que estaba relacionada la empresa que ha recibido el ataque y el asunto del correo.

• Los **dominios** tiene un **nombre similar** a otras **webs** **legítimas** (posiblemente empresas colaboradoras), las cuales realizan trabajos fiscales. Se valora la posibilidad de que se intentase realizar “**typesquatting**” (probabilidad de que un usuario teclee erróneamente una dirección web y abra otra diferente a la original). Es probable que se hiciera con la intención de ofrecer más credibilidad al ataque.

• **Envío del correo electrónico desde la versión web para evitar dejar rastro** de su dispositivo (ordenador y gestor de correo). De esta manera, la única IP de la cual se tiene constancia, es la del servidor de correo del proveedor y no de la IP de su casa o trabajo.

• **Varios servidores de correo se encuentran en blacklist** (listas negras de IPs.) Por lo que se podría relacionar con el posible ataque.

• Los dominios se dieron de alta el mismo día, este dato es meramente informativo, y denota la posibilidad de que el sospechoso iniciase ese día la elaboración del ataque dando de alta diferentes dominios.

•**Envió un correo electrónico personalizado**. El sospechoso buscó información específica de la empresa, para realizar este ataque. Usó el nombre de un alto cargo y tenía un objetivo claro, otro empleado, en este caso, de la delegación ubicada en Portugal.

• También cabe destacar que el correo fue **escrito** en el idioma portugués **sin** **ningún error ortográfico**, por lo que se descarta que fuera spam y si un ataque dirigido.

Tras la recopilación de todas estas evidencias, se determina que no ha sido el típico caso de spam. Se considera, que el usuario sospechoso intentó realizar un ataque llamado “Spearphishing”. Este ataque es una estafa que se transmite a través del correo electrónico con la única finalidad de obtener acceso no autorizado a datos confidenciales. A diferencia de las estafas de phishing, que lanzan amplios ataques masivos, el Spearphishing centra su objetivo en una organización o grupo específico. Su intención es robar datos de propiedad intelectual, información financiera o secretos comerciales o militares u otra información confidencial.

Tal y como se ha indicado, los datos se han anonimizado o modificado para mantener oculta la información original.

Por último, me gustaría agradecer a [Javier Olmedo](https://twitter.com/JJavierOlmedo) el darme la posibilidad de publicar esta entrada.

Esperemos que os haya gustado y hasta la próxima.