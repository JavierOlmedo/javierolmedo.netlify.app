+++
date = "2025-03-07"
title = "Snyk Fetch The Flag - ClickityClack"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_banner.png](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_banner.png)](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_banner.png)

In this challenge, we started with a `pcapng` file that contains the hidden flag inside. Initially, I tried to analyze it using **Wireshark**, but couldn't find anything relevant. That's when a simple Google search gave me the key clue:

```txt
pcapng usb ctf
```

[![/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_001.png](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_001.png)](/images/snyk_fetch_the_flag_clickityclack/snyk_fetch_the_flag_clickityclack_001.png)

This search led me to discover a tool called [Usb_Keyboard_Parser.py](https://raw.githubusercontent.com/5h4rrk/CTF-Usb_Keyboard_Parser/refs/heads/main/Usb_Keyboard_Parser.py) that parses USB traffic packets, such as keypresses.

All it takes is to pass the file to the script to retrieve the flag at the end of the keypresses.

```bash
python Usb_Keyboard_Parser.py click.pcapng
```

```txt
[-] Found Modifier in 10 packets [-]

[+] Using filter "usbhid.data" Retrived HID Data is : 

aaaaacaaaaaaaaaabaaaaaababadaaaabaaaaaacaaaabaaaaaaadabbacbabccabcccabcgccccccgccdcdcgccdfccdccdfccddccfccfdbccaccbacbbcbbcbbbcbbcbbbbbcbbcbbcgbcbbbcbbbccbacbcbbbcbbcbbbbbbcbbafbbbababaaeabaaaaaaacaaaaaaaacaaaaaaeabaaaaaaabaaababbbbacbbabbbbbbbbbbbebbbabbbcbebbcbbcbbebbcacbbbbbfbbbcacbgcababbdbbdbababbabbcbbababbbaebabaabbbaeabbaababbabaebabbabbaeabaaabaaaabaabaabaaabaaaaebaaaaaaabacaabaaaaaaaaaaaaeaaaaaaaaaaaaaaaaaaaaaaacaaaaaaaaaaabaabaaafaabababbabaaaacaababaaacabaababbbbbbbebcbdcccccccfdceddceddcddefeeheeefeaeeeeiefeefefnefefeiefeeiefefefeeieefaeeeejeedfedhdedbdegddcdgdddcdcdacdfdccccdfcccbbbcbbbbbbbbbadaaaaaaaaaaflag{a3ce310e9a0dc53bc030847192e2f585}

↑↑Thhis iis a ppreetty ccool thhing tthatt I hhave ffigguredd ooutt.
Yoou can capturre thhe chharacteers yyouuu type in PCAPs.aa↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmm↑↑
mmmmmmmmmmaaaaaaabdbaaaaaaaaabaaaaaa
```

## 🚩 Flag

```txt
flag{a3ce310e9a0dc53bc030847192e2f585}
```