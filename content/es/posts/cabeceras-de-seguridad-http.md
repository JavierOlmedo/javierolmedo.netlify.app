+++
date = "2019-12-11"
title = "Cabeceras de seguridad HTTP"
author = "Javier Olmedo"
toc = false
+++

Las **Cabeceras HTTP** son parámetros que se envían en una petición o respuesta HTTP entre el cliente y el servidor para proporcionar información esencial sobre la transacción que se esté llevando a cabo. Estas cabeceras siempre utilizan la sintaxis **«Cabecera: Valor»** y a continuación veremos cuales son las **cabeceras de respuesta** más importantes enfocándonos en la seguridad.

## HTTP Strict Transport Security (HSTS)

Esta cabecera permite indicar a un navegador que **siempre** se utilice el protocolo HTTPS para establecer la comunicación entre el cliente y servidor (en lugar de HTTP). Esto significa que cualquier marcador, enlace, recurso o dirección forzará a los usuarios a hacer uso de HTTPS, inclusive si el propio usuario especifica el uso de HTTP.

**Ejemplo:**

`Strict-Transport-Security: max-age=31536000 ; includeSubDomains`

|Valor|Descripción|
|---|---|
|max-age=SECONDS|Tiempo, en segundos, que el navegador debe de recordar que solo debe de acceder a la aplicación utilizando HTTPS|
|includeSubDomains|Si se especifica, esta regla también se aplica a todos los subdominios|

## X-XSS-Protection

Se utiliza para configurar la protección contra ataques del tipo Cross-Site Scripting (XSS).

**Ejemplo:**

`X-XSS-Protection: 1; mode=block`

|Valor|Descripción|
|---|---|
|0|Desactiva la protección|
|1|Activa la protección|
|1; mode=block|Activa la protección y, además, cuando se detecte un ataque XSS en lugar de desinfectar la página se evitará su representación|
|1; report=http://[DOMINIO]/mi-reporte|Activa la protección y, además, el navegador desinfectará la página e informará del intento de ataque|

## X-Content-Type-Options

Sólo tiene cun valor válido **nosniff**. Impide que Google Chrome e Internet Explorer intenten detectar el tipo de contenido de una respuesta distinta de la que el servidor declara.

**Ejemplo:**

`X-Content-Type-Options: nosniff`

|Valor|Descripción|
|---|---|
|nosniff|Evita que el navegador detecte una respuesta MIME distinta del tipo de contenido declarado.|

## X-Frame-Options

Protege a los visitantes de ataques del tipo **Clickjacking**. Un usuario malintencionado podría cargar un iFrame malicioso en su su servidor y establecer el sitio legítimo como fuente, de esta manera, podría obligar al usuario a hacer click en partes de la aplicación sin ser cosciente de ello.

**Ejemplo:**

`X-Frame-Options: deny`

|Valor|Descripción|
|---|---|
|deny|La más restrictiva, impide que la página pueda ser incluida en un iFrame, incluso si se encuentra en la misma aplicación o dominio|
|sameorigin|Impide que la página pueda ser incluida en un iFrame salvo que se encuentre en el mismo dominio|
|allow-from: DOMINIO|Permite cargar la página en iFrames de una única URL permitida|

## Content Security Policy

El encabezado CSP (Content Security Policy) permite definir una **lista blanca** de fuentes de contenido aprobadas para su sitio. Al restringir los activos que un navegador puede cargar para su sitio, como JS y CSS, CSP puede actuar como una contramedida efectiva para los ataques XSS.

**Ejemplo:**

`Content-Security-Policy: script-src 'self'`

|Valor|Descripción|
|---|---|
|base-uri|Defina la base uri para la uri relativa.|
|default-src|Defina la política de carga para todos los tipos de recursos en caso de que una directiva dedicada de tipo de recurso no esté definida (reserva).|
|script-src|Defina qué scripts puede ejecutar el recurso protegido.|
|object-src|Defina desde dónde el recurso protegido puede cargar complementos.|
|style-src|Defina qué estilos (CSS) aplica el usuario al recurso protegido.|
|img-src|Defina desde dónde el recurso protegido puede cargar imágenes.|
|media-src|Defina desde dónde el recurso protegido puede cargar video y audio.|
|frame-src|En desuso y reemplazado por child-src. Defina desde dónde el recurso protegido puede incrustar marcos.|
|child-src|Defina desde dónde el recurso protegido puede incrustar marcos.|
|antepasados|Defina desde dónde se puede incrustar el recurso protegido en marcos.|
|font-src|Defina desde dónde el recurso protegido puede cargar fuentes.|
|connect-src|Defina qué URI puede cargar el recurso protegido mediante interfaces de script.|
|manifest-src|Defina desde dónde el recurso protegido puede cargar el manifiesto.|
|forma-acción|Defina qué URI se pueden usar como acción de elementos de formulario HTML.|
|salvadera|Especifica una política de espacio aislado HTML que el agente de usuario aplica al recurso protegido.|
|script-nonce|Defina la ejecución del script al requerir la presencia del nonce especificado en los elementos del script.|
|tipos de plugin|Defina el conjunto de complementos que puede invocar el recurso protegido limitando los tipos de recursos que se pueden incrustar.|
|reflejado-xss|Indica a un agente de usuario que active o desactive cualquier heurística utilizada para filtrar o bloquear ataques de secuencias de comandos entre sitios reflejados, equivalente a los efectos del encabezado X-XSS-Protection no estándar.|
|bloquear todo el contenido mixto|Evitar que el agente de usuario cargue contenido mixto.|
|actualizaciones-inseguras-solicitudes|Indica al agente de usuario que descargue recursos inseguros utilizando HTTPS.|
|referente|Definir la información que el agente de usuario debe enviar en el encabezado Referer.|
|report-uri|Especifica un URI al que el agente de usuario envía informes sobre infracciones de políticas.|
|Reportar a|Especifica un grupo (definido en el encabezado Report-To) al que el agente de usuario envía informes sobre infracciones de políticas.|

## HTTP Public-Key-Pins

HTTP Public Key Pinning (HPKP) es un mecanismo de seguridad que permite a los sitios web HTTPS **desconfiar** en la identidad por parte de usuarios malintencionados que utilizan certificados emitidos por CAs que no son de confianza o fraudulentas.

**Ejemplo:**


`Public-Key-Pins: pin-sha256="d6qzRu9zOECb90Uez27xWltNsj0e1Md7GkYYkVoZWmM="; pin-sha256="E9CZ9INDbd+2eRQozYqqbQ2yXLVKB9+xcprMF+44U1g="; report-uri="http://example.com/pkp-report"; max-age=10000; includeSubDomains`

|Valor|Descripción|
|---|---|
|pin-sha256=»SHA256″|Cadena entre comillas con clave pública (SPKI) codificada en Base64|
|max-age=SEGUNDOS|Tiempo, en segundos, que el navegador debe de recordar que solo debe de acceder a la aplicación utilizando la configuración establecida|
|includeSubDomains|Si se especifica, esta regla también se aplica a todos los subdominios|
|report-uri=»URL»|Si se especifica, esta regla informa de los errores de validación|

## X-Permitted-Cross-Domain-Policies

Un archivo de política entre dominios es un documento XML que otorga a un cliente web, como Adobe Flash Player o Adobe Acrobat (aunque no necesariamente se limita a estos), permiso para manejar datos a través de dominios. Cuando los clientes solicitan contenido alojado en un dominio de origen particular y ese contenido realiza solicitudes dirigidas a un dominio diferente al suyo, el dominio remoto debe alojar un archivo de política de dominio cruzado que otorgue acceso al dominio de origen, permitiendo que el cliente continúe la transacción. Normalmente, una metapolítica se declara en el archivo de política maestra, pero para aquellos que no pueden escribir en el directorio raíz, también pueden declarar una metapolítica utilizando el encabezado de respuesta HTTP X-Permitted-Cross-Domain-Policies.

**Ejemplo:**

`X-Permitted-Cross-Domain-Policies: none`

|Valor|Descripción|
|---|---|
|none|No se permiten archivos de política en ningún lugar del servidor de destino, incluido el archivo de política maestro|
|master-only|Solo se permite el archivo de política maestra|
|by-content-type|Solo se permiten los archivos de política que se sirven con Content-Type: text/x-cross-domain-policy|
|by-ftp-filename|Solo se permiten archivos de política cuyos nombres de archivo sean crossdomain.xml|
|all|Se permiten todos los archivos de políticas en el dominio|

## Referrer-Policy

El encabezado HTTP de la Política de referencia regula qué información de referencia, enviada en el encabezado Referer, debe incluirse con las solicitudes realizadas.

**Ejemplo:**

`Referrer-Policy: no-referrer`

[https://www.w3.org/TR/referrer-policy/](https://www.w3.org/TR/referrer-policy/)

## Expect-CT

Un servidor utiliza el encabezado Expect-CT para indicar que los navegadores deben evaluar las conexiones al host que emite el encabezado para el cumplimiento de la transparencia del certificado.

**Ejemplo:**


`Expect-CT: max-age=86400, enforce, report-uri="https://foo.example/report"`

|Valor|Descripción|
|---|---|
|report-uri|La directiva opcional report-uri indica la URL a la cual el navegador debe informar las fallas de Expect-CT|
|enforce|La directiva opcional de cumplimiento es una directiva sin valor que, si está presente, le indica al navegador que se debe hacer cumplir la Política de CT (en lugar de solo el informe) y que el navegador debe rechazar conexiones futuras que violen su Política de CT. Cuando tanto la directiva de cumplimiento como la directiva de informe-uri están presentes, la configuración se conoce como una configuración de «exigir e informar», lo que indica al navegador que se debe hacer cumplir la política de CT y que se deben informar las violaciones|
|max-age|La directiva de edad máxima especifica el número de segundos después de la recepción del campo de encabezado Expect-CT durante el cual el navegador debe considerar el host del que se recibió el mensaje como un host Expect-CT conocido|

## Feature-Policy

El encabezado Feature-Policy permite a los desarrolladores habilitar y deshabilitar selectivamente el uso de varias funciones del navegador y API.

**Ejemplo:**

`Feature-Policy: vibrate 'none'; geolocation 'none'`

[https://w3c.github.io/webappsec-feature-policy/](https://w3c.github.io/webappsec-feature-policy/)

Hasta la próxima!!