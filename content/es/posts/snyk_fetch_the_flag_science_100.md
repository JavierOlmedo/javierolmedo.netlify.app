+++
date = "2025-03-06"
title = "Snyk Fetch The Flag - Science 100"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_science_100/snyk_fetch_the_flag_science_100_banner.png](/images/snyk_fetch_the_flag_science_100/snyk_fetch_the_flag_science_100_banner.png)](/images/snyk_fetch_the_flag_science_100/snyk_fetch_the_flag_science_100_banner.png)

En este desafío de CTF, nos encontramos con un **terminal de Robco Industries** similar al que aparece en la saga del videojuego **Fallout**. Nuestra misión es descifrar la contraseña correcta utilizando la mecánica de `Likeness (similitud)`, que nos indica cuántas letras coinciden en la posición exacta con la palabra clave.

## 🖥️ Conectando al reto

```bash
$ nc challenge.ctf.games 32377
```

Al conectarnos, el sistema nos da la bienvenida y nos presenta una lista de **posibles contraseñas** camufladas entre símbolos aleatorios.

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

## 🔑 Descifrando la contraseña

El reto nos permite seleccionar palabras como posibles claves de acceso. Cada intento fallido nos indicará cuántas letras están en la posición correcta respecto a la contraseña real.

- Si el resultado es `0`, la palabra no comparte ninguna letra en la posición correcta con la contraseña real.
- Si el resultado es `1-3`, las letras existen y están correctas en su posición.
- Si el resultado es `4`, hemos encontrado la contraseña correcta.

Probamos con nuestra primera suposición:

```txt
> GOAT
ENTRY DENIED. LIKENESS: 1/4
[!] ATTEMPTS REMAINING: 3
```

Significa que una sola letra de `GOAT` está en la posición correcta en la palabra clave.

Seguimos con otra palabra completamente diferente:

```txt
> BARE
ENTRY DENIED. LIKENESS: 0/4
[!] ATTEMPTS REMAINING: 2
```

Aquí, ninguna de las letras de `BARE` está en la contraseña. Esto nos permite **descartar cualquier palabra** que contenga `B`, `A`, `R` o `E`.

Finalmente, elegimos una palabra que no comparte letras con las anteriores pero sí tiene una coincidencia con `GOAT`, que es `ROCK`:

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

¡Accedimos al sistema!

```txt
Select an option (1, 2, or 3): 2
flag{89e575e7272b07a1d33e41e3647b3826}
```

## 🚩 Flag

```txt
flag{89e575e7272b07a1d33e41e3647b3826}
```