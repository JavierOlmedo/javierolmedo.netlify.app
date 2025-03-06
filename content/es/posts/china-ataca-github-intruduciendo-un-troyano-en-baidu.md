+++
date = "2015-03-28"
title = "China ataca GitHub introduciendo un troyano en Baidu"
author = "Javier Olmedo"
toc = false
+++

[![/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_banner.jpg](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_banner.jpg)](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_banner.jpg)

Seguro que muchos de los lectores de este blog, teniendo en cuenta cuál es su público más habitual, ya habrán notado que el funcionamiento de **[GitHub](https://github.com/)** esta mañana ha sido intermitente.

Como es habitual, este mal funcionamiento del servicio se debió a un nuevo ataque DDoS, como se puede comprobar en la **[página de estado](https://www.githubstatus.com/messages)** de GitHub.

Siendo los ataques a GitHub tan habituales, ¿por qué prestarle atención a este en concreto? Porque, en primer lugar, según la información disponible, este parece tener un atacante identificado con bastante probabilidad: el Gran Firewall chino. Sin embargo, tampoco es el primer ataque conocido con este origen: recordemos que, a principios de enero, el Gran Firewall hizo que el nombre de dominio de webs censuradas como Facebook resolviese a la dirección IP de la organización ciberactivista francesa La Quadrature du Net, provocando un volumen de tráfico para el que no estaban preparados y que **[provocó una denegación de servicio](https://benjamin.sonntag.fr/en/2015/ddos-on-la-quadrature-du-net-analysis.html)**.

¿Por qué sucedió esto? Entre las hipótesis más probables barajadas se encuentra la de que haya sido una acción intencionada del Gobierno chino para usar el Gran Firewall por el que pasa la conexión de todos sus ciudadanos para convertir a todos los que quisieran visitar Facebook en atacantes de La Quadrature du Net, una organización que defiende los derechos y libertades en Internet, claramente opuesta a la postura del Gobierno chino con respecto a Internet.

Otras hipótesis barajada es la de que no haya sido una orden que proviniese del Gobierno chino, sino que un empleado con acceso al Gran Firewall esté aprovechando su infraestructura para vender servicios de DDoS a terceras personas.

En cualquier caso, tampoco podemos asegurar que lo que haya pasado –y que es otra hipótesis que se ha planteado– no sea simplemente que, cuando el Gran Firewall bloquea algún sitio –como Facebook, en este caso– lo que haga sea que el dominio resuelva a cualquier dirección IP aleatoria y, en este caso, por azar, le ha tocado a una dirección IP de La Quadrature du Net (aunque, de ser esto lo que están haciendo, habría otros **[rangos de direcciones IP mejores](https://datatracker.ietf.org/doc/html/rfc5735)** para hacer esto).

No obstante, habiendo también precedentes de ataques con evidencias de proceder del Gran Firewall chino, ¿qué tiene de especial este ataque para que hayamos decidido hablar de él? Lo que hace este ataque interesante son tres motivos:

- La forma en la que se ha llevado a cabo, que es ciertamente original e inusual y solo posible para alguien con acceso a una infraestructura que pueda interceptar las comunicaciones de millones de personas, como el Gran Firewall chino.
- La respuesta de GitHub, que también ha sido original.
- El atacante chino ha usado también usuarios extranjeros para llevar a cabo su ofensiva, poniendo en evidencia una vez más el diseño de Internet basado en la confianza y suposición de buena fe entre las partes y en cómo nos vemos afectados cuando un gran actor como es China traiciona ese principio de buena fe.

## Cómo se ha llevado a cabo el ataque

Según las **[investigaciones de Insight-labs](https://www.dougwead.com/)**, el modo en el que fue lanzado el ataque fue «secuestrando» el código de tracking de Baidu. Como, sin duda, la mayoría de los lectores sabrán, Baidu es el buscador local chino que tiene prácticamente la hegemonía del mercado, sobre todo tras la retirada de Google al [negarse a seguir colaborando con la censura](https://googleblog.blogspot.com/2010/01/new-approach-to-china.html) tras recibir un ataque con origen en China. Al igual que Google tiene su servicio [Analytics](https://analytics.google.com/analytics/web/#/p291271733/reports/intelligenthome), Baidu tiene un [servicio similar](https://tongji.baidu.com/web5/welcome/login) de tracking para analizar las visitas recibidas por las páginas web que decidan usar este servicio.

El atacante pudo interceptar las peticiones que solicitaban descargar el código de tracking de Baidu detectando las peticiones a `http://hm.baidu.com/h.js` para desviarlo a su propio código malicioso. En la siguiente captura, podemos ver a la izquierda el código de tracking actual de Baidu y a la derecha el código que se descargaba durante el ataque según [Slashdot](https://slashdot.org/submission/4299157/chinas-national-firewall-hijacks-javascript-to-ddos-github):

[![/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_001.png](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_001.png)](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_001.png)

Si bien ambos códigos están ofuscados, se puede ver que son diferentes ya que el método de empaquetado es distinto y la longitud del código difiere bastante, siendo el código original mucho más largo.

Si desofuscamos el código y comparamos ambas versiones, teniendo de nuevo a la izquierda el código actual y legítimo de Baidu y a la derecha el código del ataque, vemos de nuevo que ambos códigos son totalmente distintos y que el código legítimo es mucho más largo:

[![/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_002.png](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_002.png)](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_002.png)

Echando un vistazo al código se puede ver rápidamente que el de la izquierda sí tiene la apariencia de ser un código de tracking, mientras que el de la derecha lo único que hace es activar un temporizador que, cada dos segundos, lanza una petición para descargar el contenido de `https://github.com/greatfire/` y `https://github.com/cn-nytimes/`.

Esto provoca que cada usuario que visite una página que use el código de tracking de Baidu esté cada dos segundos haciendo peticiones a GitHub y, de este modo, está participando sin darse cuenta en un ataque DDoS.

Nótese, además, que esta suplantación de código no solo afecta a ciudadanos chinos, sino también a cualquier ciudadano extranjero que intente descargar el código de Baidu, ya que, al estar Baidu alojado en China, su conexión pasaría del mismo modo por el Gran Firewall y se realizaría la suplantación de código igualmente, como le sucedió al investigador de Insight-labs.

## Cómo mitigó GitHub el ataque

Si bien el ataque tiene una cierta originalidad –aunque no hubiese sido posible sin la gran cantidad de tráfico que se puede manipular teniendo acceso al Gran Firewall– la respuesta de GitHub fue más original todavía, ya que el script malicioso no sólo envía una petición para descargar el contenido de `https://github.com/greatfire/` y `https://github.com/cn-nytimes/`, sino que, debido a la forma en la que está programado, también intenta ejecutar ese código como si fuera JavaScript.

Como ese código que descargaba no era JavaScript, sino HTML, probablemente se generará un error en la consola del navegador y no pasará nada. ¿Cómo aprovechó esto GitHub? Reemplazó el código de las páginas atacadas para incluir en su lugar únicamente la siguiente instrucción JavaScript: `alert("WARNING: malicious javascript detected on this domain")`

Como se puede ver en la siguiente [captura de pantalla](http://public.chuso.fastmail.com.user.fm/1935702586567.17_signed.pdf):

[![/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_003.png](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_003.png)](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_003.png)

Y sirvió ese contenido con `Content-Type` application/javascript:

[![/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_004.png](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_004.png)](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_004.png)

Así, reemplazado el contenido de esas direcciones por esa instrucción JavaScript y sirviéndolo con el `Content-Type` correspondiente, el contenido descargado sí se procesaba correctamente como JavaScript y lo que ejecutaba era la única sentencia que incluía, que lo que hacía era mostrar una advertencia al usuario:

[![/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_005.png](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_005.png)](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_005.png)

De este modo, con esa advertencia se consiguen tres objetivos:

- Advertir al visitante (y probablemente al dueño de la web cuando la visite) de que esa web incluye un scriptmalicioso.
- Retrasar el tiempo entre peticiones: ya que la función `window.alert()` es bloqueante, el resto del script no continuará hasta que el usuario haga clic en aceptar, por lo que se introduce un retraso en esa temporización de dos segundos.
- Dar la opción al usuario de detener el ataque. Esta nueva variante del script (la versión troyanizada por GitHub de la versión troyanizada por el Gran Firewall del script de Baidu) estaría mostrando alertas con un intervalo de dos segundos, algo sin duda muy molesto para el usuario. La mayoría de navegadores modernos, para evitar la ejecución de scripts molestos, a partir de la segunda alerta dan al usuario la opción de abortar la ejecución del script:

[![/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_006.png](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_006.png)](/images/china-ataca-github-intruduciendo-un-troyano-en-baidu/china-ataca-github-intruduciendo-un-troyano-en-baidu_006.png)

Si el usuario, molesto, marca esa casilla antes de hacer clic en aceptar, se detendría el ataque. A base de provocarle una molestia, GitHub puede conseguir que el usuario colabore para detener el ataque.

## Conclusión

Como era de esperar, ni en este caso ni en el de La Quadrature du Net hay información oficial ni ninguna explicación por parte de ningún responsable chino que pueda aclarar qué es lo que ha pasado, por lo que lo único con lo que nos podemos quedar es con conjeturas como estas, pero no podremos saber con certeza qué ha pasado.

Sin embargo, dos conclusiones como mínimo creo que se pueden extraer de todo esto:

- La dependencia de la comunidad de software libre de un servicio centralizado como es GitHub. Si bien hay alternativas a GitHub de código abierto (GitHub no es código abierto), como [GitLab](https://about.gitlab.com/), su hegemonía es indiscutible y, aunque esto puede ser un indicativo de la calidad de su servicio, queda en evidencia lo vulnerables que somos a las caídas del mismo, en las que parece que una parte de la actividad de desarrollo se detiene.
- El diseño de Internet pertenece a otros tiempos y está basado en relaciones de confianza en las que se asume por adelantado la buena fe de todas las partes. Si bien algunos protocolos han ido añadiendo capas y remiendos de seguridad, la arquitectura de Internet todavía es vulnerable a la traición a la buena fe de alguna de las partes, sobre todo al más bajo nivel, como ya se ha [demostrado](https://www.bgpmon.net/chinese-isp-hijacked-10-of-the-internet/) en [repetidas ocasiones](https://en.wikipedia.org/wiki/Room_641A).

Artículo cortesía de [Jesús Perez](https://x.com/chusop)