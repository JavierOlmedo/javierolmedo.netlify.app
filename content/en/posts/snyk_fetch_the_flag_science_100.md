+++
date = "2025-03-06"
title = "Snyk Fetch The Flag - Science 100"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_science_100/snyk_fetch_the_flag_science_100_banner.png](/images/snyk_fetch_the_flag_science_100/snyk_fetch_the_flag_science_100_banner.png)](/images/snyk_fetch_the_flag_science_100/snyk_fetch_the_flag_science_100_banner.png)

In this CTF challenge, we encounter a **Robco Industries terminal**, similar to the ones found in the Fallout video game series. Our mission is to decipher the correct password using the `Likeness` mechanic, which tells us how many letters match in the exact position with the correct password.

## 🖥️ Connecting to the Challenge

```bash
$ nc challenge.ctf.games 32377
```

Upon connecting, the system welcomes us and presents a list of **potential passwords** hidden among random symbols.

```txt
Welcome to Robco Industries (TM) Termlink

>SET TERMINAL/INQUIRE

RIT-V300

>SET FILE/PROTECTION=OWNER:RWED ACCOUNTS.F
>SET HALT RESTART/MAINT
0xF4F0  !)+}&_]{;;&,  0xF5F0  |(=>,),|;>#^
0xF4FC  =<|){_MOON/@  0xF5FC  {@{;>(&%*>_=
0xF508  )@#<(/,]_@>^  0xF608  ][)<</=.^>>>
0xF514  <+?<CLUB=_)/  0xF614  [,=*.+]<{>,,
0xF520  ><$>{;|][?<$  0xF620  #::.?#:=,*%#
0xF52C  {#,]|):^(%,]  0xF62C  &$+ROCK<&@,#
0xF538  !{{.*]=}}]()  0xF638  -/{)LEFT?!{+
0xF544  [;=#-$%]!%{,  0xF644  {(;+&;#?FIRE
0xF550  )|/[-$*{]%^)  0xF650  %[_#*}){%%()
0xF55C  @/)/(**)=.</  0xF65C  !>=-!{_(.((:
0xF568  ##_&.GOAT<=)  0xF668  =(()<)@&?>:_
0xF574  {^(]?|].[#.]  0xF674  .,&_BARE-%[|
0xF580  !?MOLD]>##}[  0xF680  .}>]!)]:&)(*
0xF58C  {^<%.:|,,[>.  0xF68C  (+![<>])>$?*
0xF598  (|*?]&@=@|#+  0xF698  ,:_;(){[?]}>
0xF5A4  ;;:{,_|?.}+?  0xF6A4  {+*>=)/}$%]|
[!] ATTEMPTS REMAINING: 4
```

## 🔑 Cracking the Password

The challenge allows us to select words as potential access keys. Each failed attempt will indicate how many letters are in the correct position relative to the real password.

- If the result is `0`, the word shares no letters in the correct position with the actual password.
- If the result is `1-3`, some letters are correct and in the right position.
- If the result is `4`, we have found the correct password.

We start with our first guess:

```txt
> GOAT
ENTRY DENIED. LIKENESS: 1/4
[!] ATTEMPTS REMAINING: 3
```

This means that only one letter in `GOAT` is correctly positioned in the actual password.

We try another completely different word:

```txt
> BARE
ENTRY DENIED. LIKENESS: 0/4
[!] ATTEMPTS REMAINING: 2
```

Here, none of the letters in `BARE` are in the password. This allows us to **eliminate any word** containing `B`, `A`, `R`, or `E`.

Finally, we pick a word that does not share letters with the previous one but does have one match with `GOAT`, which is `ROCK`:

```txt
> ROCK  
[!] ACCESS GRANTED  
Robco Industries Termlink (TM) Mail Protocol Initiated  
User: j.hammond@robcoindustries.org  

INBOX  
1) h.hacks@robcoindustries.org SUBJ: new CTF game idea  
2) flag.txt  
3) Paella recipe  
```

We gained access to the system!

```txt
Select an option (1, 2, or 3): 2  
flag{89e575e7272b07a1d33e41e3647b3826} 
```

## 🚩 Flag

```txt
flag{89e575e7272b07a1d33e41e3647b3826}
```