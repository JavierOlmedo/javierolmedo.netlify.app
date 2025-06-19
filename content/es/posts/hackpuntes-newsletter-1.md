+++
date = "2019-10-07"
title = "Hackpuntes Newsletter #1"
author = "Javier Olmedo"
toc = false
+++

Bienvenidos a la primera entrega de hackpuntes newsletter, semanalmente, iremos publicando todos los **lunes** recursos valiosos para pentesters y bug bounty hunters.

Este newsletter cubre la semana del **30 de septiembre al 6 de octubre de 2019**.

## 🧰 Herramienta

[HRShell](https://github.com/chrispetrou/HRShell)

Es una **shell reversa** capaz de ejecutar servidores HTTP/HTTPS con generación de certificados SSL «sobre la marcha», permite la configuración de proxy en el cliente y es fácilmente extensible. Ha sido desarrollado con Flask y es multiplataforma, muy buena herramienta para nuestras tareas de post-explotación.

## 📝 Writeup

[Archivo GIF en WhatsApp permite la ejecución remota de código](https://awakened1712.github.io/hacking/hacking-whatsapp-gif-rce/)

Un investigador de seguridad llamado **Awakened** ha aprovechado un bug en WhatsApp que permite la ejecución remota de código a través de un archivo GIF, esta vulnerabilidad ha sido asociada al [CVE-2019-11932](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11932) y afecta a todas las versiones de WhatsApp inferiores a la 2.19.244 para la plataforma Android

## 🎬 Video

{{< youtube 8KLCTUxh9YI >}}

@Hahamsec y @TheCyberMentor nos muestran como realizar una **fase de reconocimiento en Yahoo** en directo, algunas técnicas para buscar en crtsh, censys junto con algunos atajos nos pueden venir muy bien cuando ya no sabes que más hacer.

## 🐛 Exploit

[Checkm8](https://github.com/axi0mX/ipwndfu)

Checkm8 es el nombre del exploit que nos permite hacer **jailbreak** a todos los iPhone desde el modelo 4S al X y para el cual no existirá parche debido a que actua sobre el bootrom (una memoria de sólo lectura).

## 💰 Bug bounty

|Divulgación|Vulnerabilidad|Bounty|
|---|---|---|
|01 octubre 2019|[Open redirect protection (https://www.pixiv.net/jump.php) is broken for novels](https://hackerone.com/reports/541862)|$200|
|01 octubre 2019|[Bypassing push rules via MRs created by Email](https://hackerone.com/reports/526570)|$3000|
|01 octubre 2019|[Clientside resource Exhausting by exploiting gitlab math rendering](https://hackerone.com/reports/549040)|$1000|
|01 octubre 2019|[Stored XSS on Zeit.co user profile](https://hackerone.com/reports/541737)|—|
|01 octubre 2019|[Anonymous user login to Nexus Repository Manager](https://hackerone.com/reports/540698)|—|
|01 octubre 2019|[Steal ALL collateral during liquidation by exploiting lack of validation in flip.kick](https://hackerone.com/reports/684092)|$50000|
|01 octubre 2019|[Stealing Users OAuth Tokens through redirect_uri parameter](https://hackerone.com/reports/665651)|$750|
|01 octubre 2019|[Privilege escalation due to insecure use of logrotate](https://hackerone.com/reports/578119)|$1000|
|02 octubre 2019|[Clogin csrf in analytics.mopub.com](https://hackerone.com/reports/577920)|$280|
|02 octubre 2019|[Reports Modal in app.mopub.com Disclose by any user](https://hackerone.com/reports/574639)|$280|
|03 octubre 2019|[Administrator access to staging.railto.com](https://hackerone.com/reports/686015)|—|
|03 octubre 2019|[Server Side JavaScript Code Injection](https://hackerone.com/reports/532667)|—|
|04 octubre 2019|[Signed integer overflow in tool_progress_cb()](https://hackerone.com/reports/591770)|—|
|04 octubre 2019|[RCE Jira (CVE-2019–11581)](https://hackerone.com/reports/706841)|—|
|04 octubre 2019|[Use after free with assign by ref to overloaded objects](https://hackerone.com/reports/210238)|$500|
|04 octubre 2019|[Full Account Takeover](https://hackerone.com/reports/215859)|—|
|04 octubre 2019|[Path traversal](https://hackerone.com/reports/217344)|—|
|04 octubre 2019|[XXE in DoD website that may lead to RCE](https://hackerone.com/reports/227880)|—|
|04 octubre 2019|[RIDOR on DoD Website exposes FTP users and passes linked to all accounts!](https://hackerone.com/reports/228383)|—|
|04 octubre 2019|[Remote Code Execution (RCE) in a DoD website](https://hackerone.com/reports/248116)|—|
|04 octubre 2019|[SQL injections](https://hackerone.com/reports/272506)|—|
|04 octubre 2019|[SQL injection](https://hackerone.com/reports/488795)|—|
|04 octubre 2019|[SQL Injection in the get_publications.php](https://hackerone.com/reports/489483)|—|
|04 octubre 2019|[Full local fylesystem access (LFI/LFD) as admin via Path Traversal in the misconfigured Java servlet](https://hackerone.com/reports/497771)|—|
|04 octubre 2019|[LFI with potential to RCE](https://hackerone.com/reports/538771)|—|
|04 octubre 2019|[Possibility to takeover any user account #2 without interaction](https://hackerone.com/reports/544334)|—|
|04 octubre 2019|[Au](https://hackerone.com/reports/587214)[t](https://hackerone.com/reports/587214)[henticated User Data Disclosure](https://hackerone.com/reports/587214)|—|
|04 octubre 2019|[Vulnerable to CVE-2018-0296 Cisco ASA Path Traversal Authentication Bypass](https://hackerone.com/reports/622864)|—|
|04 octubre 2019|[Root Remote Code Execution](https://hackerone.com/reports/632721)|—|

Un saludos a todos, hasta la próxima!!