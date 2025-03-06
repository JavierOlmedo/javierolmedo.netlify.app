+++
date = "2025-03-04"
title = "Snyk Fetch The Flag - Screaming Crying Throwing up"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_banner.png](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_banner.png)](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_banner.png)

Este reto consiste en descifrar un binario cifrado con [scream](https://en.wikipedia.org/wiki/Scream_(cipher)). Para ello, podemos utilizar la herramienta [Scream Cipher Translator](https://scream-cipher.netlify.app/).

Si abrimos el archivo `screaming.bin` en **VSCode**, veremos el siguiente contenido:

```txt
a̮ăaa̋{áa̲aȧa̮ȧaa̮áa̲a̧ȧȧa̮ȧaa̲a̧aa̮ȧa̲aáa̮a̲aa̲a̮aaa̧}
```

Sin embargo, si usamos `cat` en la terminal (CMD, PowerShell o Bash), obtenemos un resultado diferente:

```txt
$ cat screaming.bin
aăaa{áaaȧaȧaaáaaȧȧaȧaaaaaȧaaáaaaaaaaa}
```

⚠️ ¡Atención! ⚠️

El contenido del archivo puede verse distinto dependiendo del visor utilizado. Esto es clave al momento de analizar el cifrado y evitar confusiones. 🧐

[![/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_001.png](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_001.png)](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_001.png)

## 🚩 Flag

```txt
flag{edabfbafedcbbfbadcafbdaefdadfaac}
```