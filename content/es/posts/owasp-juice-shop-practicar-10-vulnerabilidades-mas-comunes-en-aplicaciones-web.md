+++
date = "2018-10-08"
title = "OWASP Juice Shop Project - Laboratorio para practicar las 10 vulnerabilidades más comunes en aplicaciones web"
author = "Javier Olmedo"
toc = false
+++

OWASP Juice Shop es una aplicación web desarrollada completamente en JavaScript con el objetivo de reproducir de manera intencionada las [10 vulnerabilidades más comunes](https://www.owasp.org/index.php/Top_10-2017_Top_10) en aplicaciones web (Top Ten de OWASP) para que el usuario pueda explotarlas y aprender sobre la seguridad en aplicaciones web.

## TOP Ten de OWASP (2017)

Estas son las vulnerabilidades más comunes en aplicaciones web en el 2017

### 1) Inyección

Las fallas de inyección, como SQL, NoSQL, OS o LDAP ocurren cuando se envían datos no confiables a un intérprete, como parte de un comando o consulta. Los datos dañinos del atacante pueden engañar al intérprete para que ejecute comandos involuntarios o acceda a los datos sin la debida autorización.

### 2) Pérdida de Autenticación

Las funciones de la aplicación relacionadas a autenticación y gestión de sesiones son implementadas incorrectamente, permitiendo a los atacantes comprometer usuarios y contraseñas, token de sesiones, o explotar otras fallas de implementación para asumir la identidad de otros usuarios (temporal o permanentemente).

### 3) Exposición de datos sensibles

Muchas aplicaciones web y APIs no protegen adecuadamente datos sensibles, tales como información financiera, de salud o Información Personalmente Identificable (PII). Los atacantes pueden robar o modificar estos datos protegidos inadecuadamente para llevar a cabo fraudes con tarjetas de crédito, robos de identidad u otros delitos. Los datos sensibles requieren métodos de protección adicionales, como el cifrado en almacenamiento y tránsito.

### 4) Entidades Externas XML (XXE)

Muchos procesadores XML antiguos o mal configurados evalúan referencias a entidades externas en documentos XML. Las entidades externas pueden utilizarse para revelar archivos internos mediante la URI o archivos internos en servidores no actualizados, escanear puertos de la LAN, ejecutar código de forma remota y realizar ataques de denegación de servicio (DoS).

### 5) Pérdida de Control de Acceso

Las restricciones sobre lo que los usuarios autenticados pueden hacer no se aplican correctamente. Los atacantes pueden explotar estos defectos para acceder, de forma no autorizada, a funcionalidades y/o datos, cuentas de otros usuarios, ver archivos sensibles, modificar datos, cambiar derechos de acceso y permisos, etc.

### 6) Configuración de Seguridad Incorrecta

La configuración de seguridad incorrecta es un problema muy común y se debe en parte a establecer la configuración de forma manual, ad hoc o por omisión (o directamente por la falta de configuración). Son ejemplos: S3 buckets abiertos, cabeceras HTTP mal configuradas, mensajes de error con contenido sensible, falta de parches y actualizaciones, frameworks, dependencias y componentes desactualizados, etc.

### 7) Secuencia de Comandos en Sitios Cruzados (XSS)

Los XSS ocurren cuando una aplicación toma datos no confiables y los envía al navegador web sin una validación y codificación apropiada; o actualiza una página web existente con datos suministrados por el usuario utilizando una API que ejecuta JavaScript en el navegador. Permiten ejecutar comandos en el navegador de la víctima y el atacante puede secuestrar una sesión, modificar (defacement) los sitios web, o redireccionar al usuario hacia un sitio malicioso.

### 8) Deserialización Insegura

Estos defectos ocurren cuando una aplicación recibe objetos serializados dañinos y estos objetos pueden ser manipulados o borrados por el atacante para realizar ataques de repetición, inyecciones o elevar sus privilegios de ejecución. En el peor de los casos, la deserialización insegura puede conducir a la ejecución remota de código en el servidor.

### 9) Componentes con vulnerabilidades conocidas

Los componentes como bibliotecas, frameworks y otros módulos se ejecutan con los mismos privilegios que la aplicación. Si se explota un componente vulnerable, el ataque puede provocar una pérdida de datos o tomar el control del servidor. Las aplicaciones y API que utilizan componentes con vulnerabilidades conocidas pueden debilitar las defensas de las aplicaciones y permitir diversos ataques e impactos.

### 10) Registro y Monitoreo Insuficientes

El registro y monitoreo insuficiente, junto a la falta de respuesta ante incidentes permiten a los atacantes mantener el ataque en el tiempo, pivotear a otros sistemas y manipular, extraer o destruir datos. Los estudios muestran que el tiempo de detección de una brecha de seguridad es mayor a 200 días, siendo típicamente detectado por terceros en lugar de por procesos internos.

## 📥 Instalación de OWASP Juice Shop Project

La manera más rapida y sencilla de levantar esta aplicación para empezar a auditarla, es mediante [Heroku](https://heroku.com/).

**1.** Para levantarla, vamos al proyecto de la aplicación en [GitHub](https://github.com/bkimminich/juice-shop).

**2.** Click en el **botón de Heroku.**

**3.** Añadimos un nombre a nuestra aplicación y Click en el botón **Deploy app.**

[![/images/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web_001.png](/images/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web_001.png)](/images/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web_001.png)

**4.** Obtendremos una URL con la aplicación lista para probar las vulnerabilidades.

[![/images/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web_002.gif](/images/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web_002.gif)](/images/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web/owasp-juice-shop-practicar-10-vulnerabilidades-mas-comunes-en-aplicaciones-web_002.gif)

## 📽️ Vídeo de introducción a OWASP Juice Shop Project

{{< youtube 62Mj0ZgZvXc >}}

## 🕮 Guía oficial de OWASP Juice Shop Project

- [https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/)

Hasta la próxima ! 👋