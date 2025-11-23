+++
date = "2020-11-24"
title = "Crear tu propia VPN sin logs"
author = "Javier Olmedo"
toc = false
+++

Bienvenidos a este tutorial sobre como montar nuestra propia **VPN sin logs** mediante la configuración de un servidor **OpenVPN** en una máquina en la nube.

Antes de empezar con el tutorial, deberemos de crear una máquina en la nube, yo he usado el **VPS de DigitalOcean** en el cual tengo una máquina Ubuntu 20.04 por 5$ al mes, puedes usar el siguiente [enlace](https://m.do.co/c/67dd38080d62) para obtener **100$** gratis.

## Crear una máquina en la nube

Como he comentado al principio, he elegido DigitalOcean como VPS, pero existen otros como Linode, Azure o AWS. Para crear una máquina Ubuntu 20.04 deberemos de ir a la parte superior derecha de nuestro panel y seleccionar **Create -> Droplets**.

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_001.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_001.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_001.png)

Creación de máquina en la nube

Elegiremos la siguiente imagen con el plan más económico (**Ubuntu 20.04 por 5$ al mes**, recuerda que tienes 100$ gratis si usas mi [enlace](https://m.do.co/c/67dd38080d62)). También podremos elegir la **ubicación** de nuestro servidor, que en mi caso usaré **Frankfurt (Alemania)** y crear unas **credenciales** para el usuario root.

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_002.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_002.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_002.png)

Configuración de imagen y plan

## Generar claves SSH

En el paso anterior, creamos una contraseña para el usuario root, pero el uso de contraseñas sin cifrar para iniciar sesión en nuestra máquina no es muy buena idea ya que estas **pueden quedar expuestas en la red al no estar cifradas**. Por lo tanto, **generaremos unas claves** para que solo las máquinas que disponga de ellas (y la contraseña) puedan iniciar sesión.

Podemos generar las claves de la siguiente manera:

**En Windows** (con PowerShell):

```bash
PS C:\> Add-WindowsCapability -Online -Name OpenSSH.Client*
```

**En Linux y Mac**

```bash
ssh-keygen -t rsa -b 4096
```

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_003.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_003.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_003.png)

Generación de claves SSH en Linux

## Iniciar sesión en el servidor y actualizar

Iniciamos sesión en nuestro servidor, recuerda que puedes ver la IP de tu máquina en el panel principal de DigitalOcean.

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_004.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_004.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_004.png)

Panel principal en DigitalOcean

```bash
ssh root@[IP]
```

Escriba la contraseña que especificó a la hora de crear la máquina y ejecute los comandos de actualización.

```bash
apt-get update && apt-get upgrade
```

## Crear un usuario

Si bien el usuario root nos permite realizar cualquier acción sobre la máquina, **no es recomendable que esté habilitado para SSH**, por lo tanto, crearemos un usuario con permisos para el uso de **sudo y bash** como shell predeterminado.

```bash
useradd -G sudo -m jolmedo -s /bin/bash
```

Después crearemos una contraseña.

```bash
passwd jolmedo
```

## Copiar las claves SSH al servidor

En este paso, vamos a copiar las claves SSH generadas del paso anterior a nuestra máquina en la nube.

**En Windows** (con PowerShell):

```bash
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh IP "cat >> .ssh/authorized_keys"
```

**En Linux y Mac**:

```bash
ssh-copy-id jolmedo@[IP]
```

## Deshabilitar autenticación por contraseña y configuración de seguridad para SSH

Una vez tenemos las claves SSH en el servidor, procederemos a deshabilitar la autenticación por contraseña y a habilitar la autenticación por clave pública. Editamos el archivo sshd

```bash
nano /etc/ssh/sshd_config
```

Antes de nada, vamos a cambiar el puerto por defecto de SSH, puedes usar cualquier puerto (yo usaré el 22022), esto ayudará a que los escáneres no puedan intentar iniciar sesión en nuestro servidor con credenciales por defecto.

```bash
# Port 22
Port 22022
```

Deshabilitamos autenticación por contraseña (solo se podrá iniciar sesión con clave pública).

```bash
PasswordAuthentication no
```

Deshabilitamos el inicio de sesión con root.

```bash
PermitRootLogin no
```

Por último, reiniciamos SSH para aplicar cambios.

```bash
systemctl restart sshd
```

## Configuración de OpenVPN

Aquí viene el plato fuerte del tutorial, configurar OpenVPN puede llevarnos algún tiempo (instalación de paquetes, generar las claves, configurar IPTables, generar archivos de configuración, etc), pero gracias al trabajo de un usuario de GitHub, podremos hacerlo de una manera muy sencilla.

En primer lugar, descarga el repositorio.

```bash
git clone https://github.com/Nyr/openvpn-install.git
```

Nos posicionamos en el directorio.

```bash
cd openvpn-install/
```

Ejecutamos el script.

```bash
sudo bash openvpn-install.sh
```

**NOTA:** Siempre que descargue cualquier script, asegúrate que no haya nada sospechoso en él.

Una vez ejecutado, tan solo deberemos de responder a unas pocas preguntas.

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_005.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_005.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_005.png)

Ejecución de script de instalación de OpenVPN

Si os habéis fijado en la captura anterior, he cambiado el puerto por defecto de OpenVPN al 443, esto es debido a que algunas redes pueden **bloquear dicho puerto**. He decidido usar **443** (el mismo que HTTPS) y os preguntareis que esto puede dar algún problema pero, mientras HTTPS usa TCP, OpenVPN usa UDP, por lo tanto, no habrá ningún conflicto entre ellos.

Después de contestar a las preguntas, el proceso de instalación comenzará. Cuando termine, podremos ver nuestro archivo de configuración **.ovpn** en la carpeta raíz del usuario root, vamos a moverlo a la carpeta del usuario jolmedo y a asignarlo como propietario.

```bash
sudo mv /root/hackpuntes-vps.ovpn ~
```

```bash
sudo chown jolmedo hackpuntes-vps.ovpn
```

## Deshabilitar registros en nuestra VPN

Con todo ya preparado, solo nos queda hacer una cosa muy importante en la parte del servidor, y es deshabilitar los registros, para ello, vamos a modificar el siguiente archivo.

```bash
sudo nano /etc/openvpn/server/server.conf
```

Buscamos la línea **verb 3** y lo cambiamos a **verb 0**. Ahora reiniciamos OpenVPN.

```bash
systemctl restart openvpn-server@server.service
```

Ahora ya tenemos una VPN que **realmente no mantiene los registros**.

## Probar la VPN

Descargamos el fichero **hackpuntes-vps.ovpn** a nuestra máquina local y creamos una nueva conexión VPN.

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_006.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_006.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_006.png)

Creación de VPN en Kali Linux

Elegimos **Import a saved VPN configuration…**

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_007.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_007.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_007.png)

Importación de fichero ovpn

Seleccionamos el fichero **hackpuntes-vps.ovpn**

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_008.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_008.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_008.png)

Selección de fichero ovpn

Y veremos algo similar a lo siguiente:

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_009.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_009.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_009.png)

Configuración de VPN con fichero ovpn

Guardamos los cambios y ya tenemos nuestra VPN totalmente funcional y sin registro de logs.

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_010.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_010.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_010.png)

Activación de VPN

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_011.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_011.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_011.png)

Mensaje de confirmación de conexión VPN

Podemos hacer uso de [WHOER](https://whoer.net/) para conocer información y confirmar el uso de nuestra VPN.

[![/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_012.png](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_012.png)](/images/crear-tu-propia-vpn-sin-logs/crear-tu-propia-vpn-sin-logs_012.png)

Uso del servicio WHOER

Muchas gracias por seguir el tutorial, compártelo si te fue útil y no dudes en preguntar en los comentarios si necesitas ayuda.