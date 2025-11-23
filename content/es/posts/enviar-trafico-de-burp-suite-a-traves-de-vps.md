+++
date = "2020-04-13"
title = "Enviar tráfico de Burp Suite a través de VPS"
author = "Javier Olmedo"
toc = false
+++

Cuando estamos auditando una aplicación externa con Burp Suite, podemos encontrarnos en un entorno con restricciones de red, por ejemplo, un firewall podría interferir en nuestros paquetes y descartarlos o modificarlos, lo que daría como resultado falsos positivos o la anulación de ciertas pruebas de seguridad.

En esta entrada, veremos como **enviar el tráfico del proxy de Burp Suite a través de una máquina linux en la nube (VPS)** para evitar estos problemas u otros similares.

1. En primer lugar, nos conectamos por SSH a nuestro VPS y abrimos el fichero `/etc/ssh/sshd_config`.

```bash
ssh root@[IP-VPS]
sudo nano /etc/ssh/sshd_config
```

2. Nos aseguramos que los valores **AllowTCPForwarding** y **PermitOpen** estén comentados, no establecidos o configurados de la siguiente manera.

```bash
AllowTCPForwarding yes
PermitOpen any
```

3. Guardamos cambios, reiniciamos el servicio SSH y salimos de la sesión.

```bash
sudo service sshd restart
exit
```

4. En nuestra máquina local, configuramos el navegador para que trabaje con el proxy de Burp Suite (puerto 8080).

5. Volvemos a abrir una conexión SSH al VPS, esta vez, estableciendo un puerto (ejemplo 23000). Esto se utilizará para pasar nuestro tráfico local al VPS. (no cerrar esta ventana durante la auditoria, podemos ejecutarla en segundo plano con **&** o hacer uso de **TMUX**).

```bash
ssh -D 23000 root@[IP-VPS]
```

6. En Burp Suite, vamos a la pestaña **Project options** -> **Connections** y habilitamos **SOCKS Proxy** de la siguiente manera:

- Override user options
- SOCKS proxy host: **localhost**
- SOCKS proxy port: **23000**
- Do DNS lookup over SOCKS proxy

[![/images/enviar-trafico-de-burp-suite-a-traves-de-vps/enviar-trafico-de-burp-suite-a-traves-de-vps_001.png](/images/enviar-trafico-de-burp-suite-a-traves-de-vps/enviar-trafico-de-burp-suite-a-traves-de-vps_001.png)](/images/enviar-trafico-de-burp-suite-a-traves-de-vps/enviar-trafico-de-burp-suite-a-traves-de-vps_001.png)

Configuración de SOCKS en Burp Suite

7. Último paso, visitar [https://whatsmyip.com/](https://whatsmyip.com/) y comprobar que salimos con la IP del VPS.

[![/images/enviar-trafico-de-burp-suite-a-traves-de-vps/enviar-trafico-de-burp-suite-a-traves-de-vps_002.png](/images/enviar-trafico-de-burp-suite-a-traves-de-vps/enviar-trafico-de-burp-suite-a-traves-de-vps_002.png)](/images/enviar-trafico-de-burp-suite-a-traves-de-vps/enviar-trafico-de-burp-suite-a-traves-de-vps_002.png)

Visualización de IP pública en Whatsmyip

8. Si tenemos que usar otras herramientas como **sqlmap** o **gobuster**, acordaos de establecer Burp Suite como proxy. Si tenéis problemas de certificados, podéis echar un ojo al siguiente tutorial: [https://hackpuntes.com/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/](https://hackpuntes.com/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/)

Hasta la próxima!!