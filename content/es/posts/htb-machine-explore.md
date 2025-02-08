+++
date = '2025-02-08T01:47:24+01:00'
title = 'Htb Machine Explore'
+++


Welcome to the writeup of the **explore machine** of the Hack The Box platform. Explore is an **easy** difficulty machine on android.

# Before starting
Connect to **Hack The Box VPN** in background with `TMUX`
```bash
tmux new -s htb
sudo openvpn ~/.vpn/htb.ovpn
```

**Add** machine to `/etc/hosts` file, **check connection** with `ping` and **create work folders**
```bash
echo '10.10.10.247 explore.htb' >> /etc/hosts
ping -c 4 explore.htb
mkdir -p ~/htb/explorer.htb/{exploits,fuzz,http,nmap}
```

# Recon
Check **all open ports**, detect the service and its version with `nmap`
```bash
nmap -sC -sV -p- -T4 --min-rate=1000 -v -oA all explore.htb
```