+++
date = "2025-03-04"
title = "Snyk Fetch The Flag - Screaming Crying Throwing up"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_banner.png](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_banner.png)](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_banner.png)

This challenge consists of decrypting a binary file encrypted with [scream](https://en.wikipedia.org/wiki/Scream_(cipher)). To do this, we can use the [Scream Cipher Translator](https://scream-cipher.netlify.app/).

If we open the `screaming.bin` file in **VSCode**, we will see the following content:

```txt
a̮ăaa̋{áa̲aȧa̮ȧaa̮áa̲a̧ȧȧa̮ȧaa̲a̧aa̮ȧa̲aáa̮a̲aa̲a̮aaa̧}
```

However, if we use `cat` in the terminal (CMD, PowerShell, or Bash), we get a different result:

```txt
$ cat screaming.bin
aăaa{áaaȧaȧaaáaaȧȧaȧaaaaaȧaaáaaaaaaaa}
```

⚠️ Attention! ⚠️

The file content may appear different depending on the viewer used. This is crucial when analyzing the encryption to avoid confusion. 🧐

[![/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_001.png](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_001.png)](/images/snyk_fetch_the_flag_screaming_crying_throwing_up/snyk_fetch_the_flag_screaming_crying_throwing_up_001.png)

## 🚩 Flag

```txt
flag{edabfbafedcbbfbadcafbdaefdadfaac}
```