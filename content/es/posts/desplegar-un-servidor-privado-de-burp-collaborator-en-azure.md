+++
date = "2019-06-12"
title = "Desplegar un servidor privado de Burp Collaborator en Azure"
author = "Javier Olmedo"
toc = false
+++

Burp Collaborator es un **servicio externo** de Burp Suite que nos ayuda a detectar algunas vulnerabilidades basadas en interacciones con **servicios externos** o las llamadas **vulnerabilidades ciegas**.

Por defecto, Burp Suite **crea de manera automática** una instancia **pública** de Collaborator en sus servidores de AWS para la realización de estas pruebas, por este motivo, algunos usuarios pueden tener **preocupaciones sobre la seguridad de los datos procesados**, más aún si son de terceros. A continuación, se detallan los pasos para crear nuestro propio Burp Collaborator ayudándonos y complementando la [documentación oficial](https://portswigger.net/burp/documentation/collaborator/deploying) de PortSwigger.

## 1. Requisitos

- Máquina Linux en Azure
- Dominio personalizado en Freenom
- Archivo .jar de Burp Suite Pro

Para la realización de esta entrada, se ha utilizado una máquina virtual de **Ubuntu Server 18.04** y el archivo `.jar` de **Burp Suite Pro v1.7.37**.

## 2. ¿Qué es Burp Collaborator?

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_001.svg](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_001.svg)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_001.svg)

Cuando realizamos un escáner activo con Burp Suite, este envía **payloads diseñados para causar interacciones** entre el servidor de la aplicación que estamos auditando y otro externo. De manera periódica, Burp Suite consultará la instancia de Collaborator para comprobar si ha recibido alguna petición. Un ejemplo de petición con payload sería el siguiente:

```bash
GET /check?url=xxxxxxxx.micollaborator.com HTTP/1.1
Host: target.com
Connection: close`
```

Vemos como realizamos una petición a `target.com` y en el valor del parámetro `url` hemos añadido **nuestra instancia** de Burp Collaborator, en caso de ser vulnerable, en nuestro Collaborator tendríamos una petición con **origen de la aplicación** que estamos auditando.

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_002.svg](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_002.svg)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_002.svg)

En la web de [PortSwigger](https://portswigger.net/burp/documentation/collaborator) puedes ver más ejemplos.

## 3. Preparando la máquina Linux

Una vez creada la máquina virtual en Azure, dirígete al panel de **Redes** y abre los puertos (aquí es buen momento de apuntar la **IP Pública y Privada**, la usaremos más adelante):

- Puerto 25 y 587 -> SMTP
- Puerto 53 -> DNS
- Puerto 80 -> HTTP
- Puerto 443 -> HTTPS
- Puerto 465 -> SMTPS
- Puerto 9090 -> HTTP (Polling)
- Puerto 9443 -> HTTPS (Polling)

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_003.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_003.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_003.png)

Panel de administración de puertos en Azure

La primera regla se encuentra ofuscada, corresponde al puerto **SSH** que he cambiado el predeterminado (22) por otro, después, asegúrate de **cambiarlo en el archivo de configuración** de SSH y establecer una IP de origen, así evitamos ataques de fuerza bruta a nuestra máquina por parte de curiosos.

```bash
sudo nano /etc/ssh/sshd_config
```

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_004.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_004.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_004.png)

Archivo `/etc/ssh/sshd_config`

Al turrón, lo esencial, actualizar.

```bash
sudo apt-get update && sudo apt-get upgrade
```

Instalamos Java Runtime Environment (JRE).

```bash
sudo apt-get install default-jre
```

Creamos una carpeta para guardar todos los archivos necesarios.

```bash
sudo mkdir -p /usr/local/collaborator
```

Copia dentro de la carpeta el archivo .jar de Burp Suite y lo renómbralo a collaborator.jar

```bash
sudo mv /usr/local/collaborator/burpsuite_pro_v1.7.37.jar /usr/local/collaborator/collaborator.jar
```

## 4. Crear archivo de configuración

Para este paso, necesitamos la **IP Privada** y **Pública** que copiamos del paso anterior. Después, **crea un archivo** de configuración y añade el siguiente código (sustituye las cadenas path, serverDomain, hostname, localAddress y publicAddress por tu configuración).

```bash
sudo nano /usr/local/collaborator/collaborator.config
```

```txt
{
  "serverDomain" : "burp.hackpuntes.com",
  "workerThreads" : 10,
  "eventCapture": {
    "localAddress" : ["10.*.*.*"],
    "publicAddress" : "40.*.*.*",
    "http": {
      "ports" : 80
    },
    "https": {
      "ports" : 443
    },
    "smtp": {
      "ports" : [25, 587]
    },
    "smtps": {
      "ports" : 465
    },
    "ssl": {
      "certificateFiles" : [
        "/usr/local/collaborator/keys/privkey.pem",
        "/usr/local/collaborator/keys/cert.pem",
        "/usr/local/collaborator/keys/fullchain.pem" ]
    }
  },
  "polling" : {
    "localAddress" : "10.*.*.*",
    "publicAddress" : "40.*.*.*",
    "http": {
      "port" : 9090
    },
    "https": {
      "port" : 9443
    },
    "ssl": {
      "hostname" : "burp.hackpuntes.com"
    }
  },
  "metrics": {
    "path" : "hackpuntes",
    "addressWhitelist" : ["0.0.0.0/24"]
  },
  "dns": {
    "interfaces" : [{
      "name": "ns1",
      "localAddress" : "10.*.*.*",
      "publicAddress" : "40.*.*.*"
    }],
    "ports" : 53
  },
  "logLevel" : "INFO"
}
```

## 5. Delegar la autoridad del dominio

Primero, debemos de **delegar la autoridad del dominio** a Azure, para ello, debemos de agregar los 4 registros NS en nuestro panel de administración de dominio hacía los servidores de Azure, en mi caso, he utilizado [Freenom](https://www.freenom.com/) para generar un dominio personalizado gratuito.

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_005.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_005.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_005.png)

Panel de administración de dominio de Freenom

A partir de este punto, ya **no es necesario acceder** más al panel de administración de dominio de Freenom, acuérdate que debes de esperar un tiempo prudente hasta que los DNS se propaguen.

## 6. Crear archivos necesarios para generar y mover los certificados

Desde la carpeta **collaborator** creada en el paso 3, ejecutamos el comando wget para descargar el archivo que generará los certificados:

```bash
sudo wget https://dl.eff.org/certbot-auto
```

Asignamos permisos

```bash
sudo chmod a+x ./certbot-auto
```

Creamos otro archivo para mover los certificados (gracias [@fabiopirespt](https://twitter.com/fabiopirespt))

```bash
sudo nano /usr/local/collaborator/configure_certs.sh
```

```bash
#!/bin/bash
 
CERTBOT_DOMAIN=$1
if [ -z $1 ];
then
    echo "Missing mandatory argument. "
    echo " - Usage: $0  &amp;amp;lt;domain&amp;amp;gt; "
    exit 1
fi
CERT_PATH=/etc/letsencrypt/live/$CERTBOT_DOMAIN/
mkdir -p /usr/local/collaborator/keys/
 
if [[ -f $CERT_PATH/privkey.pem &amp;amp;amp;&amp;amp;amp; -f $CERT_PATH/fullchain.pem &amp;amp;amp;&amp;amp;amp; -f $CERT_PATH/cert.pem ]]; then
        cp $CERT_PATH/privkey.pem /usr/local/collaborator/keys/
        cp $CERT_PATH/fullchain.pem /usr/local/collaborator/keys/
        cp $CERT_PATH/cert.pem /usr/local/collaborator/keys/
        echo "Certificates installed successfully"
else
        echo "Unable to find certificates in $CERT_PATH"
fi
```

### 7. Obtener los certificados de Let´s Encrypt

Antes de empezar con este apartado, vamos a **solucionar un problema** (si, antes de empezar ya lo tenemos), si intentáramos obtener los certificados en este momento, tendríamos el siguiente error.

> DNS problem: SERVFAIL looking up CAA for burp.hackpuntes.com

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_006.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_006.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_006.png)

Error SERVFAIL al verificar el dominio

Esto ocurre porque debemos de **añadir la CAA de LetsEncrypt a Azure**, por lo tanto, abrimos la consola en Azure, situada en la barra de la parte superior.

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_007.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_007.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_007.png)

Barra superior de Azure

Prepara un bloc de notas, agrega las siguientes líneas y sustituye los valores de `ZoneName` y `ResourceGroupName` por los tuyos.

```powershell
$caaRecords = @()
$caaRecords += New-AzureRmDnsRecordConfig -CaaFlag "0" -CaaTag "iodef" -CaaValue "mailto:contacto@hackpuntes.com"
$caaRecords += New-AzureRmDnsRecordConfig -CaaFlag "0" -CaaTag "issue" -CaaValue "letsencrypt.org"
New-AzDnsRecordSet -Name "@" -RecordType CAA -ZoneName "hackpuntes.com" -ResourceGroupName [ZONA-DNS-AZURE] -Ttl 3600 -DnsRecords $caaRecords
```

Ejecuta de manera individual cada línea en la consola de Azure

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_008.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_008.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_008.png)

Azure Cloud Shell

Una vez termines, deberás de ver la siguiente entrada en la Zona DNS de Azure.

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_009.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_009.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_009.png)

Zona DNS del Azure

Ahora sí, lanzamos el siguiente comando para realizar la verificación.

```bash
./certbot-auto certonly -d hackpuntes.tk -d *.hackpuntes.tk  --server https://acme-v02.api.letsencrypt.org/directory --manual --agree-tos --no-eff-email --manual-public-ip-logging-ok --preferred-challenges dns-01
```

Debemos de añadir **dos registros TXT** con el prefijo `_acme-challenge` en la Zona DNS de Azure con los valores que nos proporciones el script, confirma que estos valores existen antes de empezar la validación.

```txt
-------------------------------------------------------------------------------
Please deploy a DNS TXT record under the name
_acme-challenge.burp.hackpuntes.com with the following value:
 
wWCRaCvYDRxFda2m3rmffZLO10pCqKZMXIdiFKi
 
Before continuing, verify the record is deployed.
-------------------------------------------------------------------------------
Press Enter to Continue (Press enter here, we are expecting two different TXT records)
 
-------------------------------------------------------------------------------
Please deploy a DNS TXT record under the name
_acme-challenge.burp.hackpuntes.com.com with the following value:
 
nOxlBAHLr_RKF47tNF_HIT0kCh92Elt14LFIhcv6WmM
 
Before continuing, verify the record is deployed.
-------------------------------------------------------------------------------
Press Enter to Continue
```

Aprovechamos este paso, y añadimos además un registro **NS** con el valor `ns1.burp.hackpuntes.com` y otro **A** desde `ns1.burp.hackpuntes.com` hacía la **IP Pública** de nuestra máquina, nos quedaría de la siguiente manera:

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_010.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_010.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_010.png)

Zona DNS de Azure

Si todo va bien, tendremos el siguiente mensaje.

```txt
Waiting for verification...
Cleaning up challenges
 
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   ...
```

Finalmente, movemos los certificados

```bash
chmod +x /usr/local/collaborator/configure_certs.sh && /usr/local/collaborator/configure_certs.sh burp.hackpuntes.com
```

## 8. Lanzar Collaborator

Llegados a este punto, sólo nos queda probar nuestro Burp Collaborator, podemos hacerlo de dos maneras, mediante un **alias** o creando un **servicio** que se arranque en el sistema, si usas esta máquina solamente para el Bup Collaborator, quizás la opción más recomendada será crear un servicio, en caso contrario, te recomiendo crear un alias.

### 8.1 Mediante alias

Edita el archivo `.bashrc` y agrega el alias **collaborator**.

```bash
sudo nano .bashrc
```

```bash
alias collaborator='sudo java -jar /usr/local/collaborator/collaborator.jar --collaborator-server --collaborator-config=/usr/local/collaborator.config'
```

Desde ahora, podremos arrancarlos escribiendo collaborator en al terminal.

### 8.2 Mediante servicio al iniciar el sistema

Creamos un archivo collaborator.service y copiamos el siguiente código

```bash
sudo nano /etc/systemd/system/collaborator.service
```

```txt
[Unit]
Description=Burp Collaborator Server Daemon
After=network.target
 
[Service]
Type=simple
User=$USER
UMask=007
ExecStart=/usr/bin/java -Xms10m -Xmx200m -XX:GCTimeRatio=19 -jar /usr/local/collaborator/collaborator.jar --collaborator-server --collaborator-config=/usr/local/collaborator/collaborator.config
Restart=on-failure
 
# Configures the time to wait before service is stopped forcefully.
TimeoutStopSec=300
 
[Install]
WantedBy=multi-user.target
```

Habilitamos el servicio

```bash
systemctl enable collaborator
```

Arrancamos el servicio

```bash
systemctl start collaborator
```

Dependiendo de la manera que lo hayamos arrancado, para probarlo vamos a la pestaña **Project Options -> Misc** dentro de Burp Suite y pulsamos el botón **Run health check**.

[![/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_011.png](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_011.png)](/images/desplegar-un-servidor-privado-de-burp-collaborator-en-azure/desplegar-un-servidor-privado-de-burp-collaborator-en-azure_011.png)

Test para comprobar el correcto funcionamiento de Burp Collaborator

## 9. Referencias y agradecimientos

Quería terminar la entrada agradeciendo enormemente al trabajo de [Fabio Pires](https://twitter.com/fabiopirespt), su [tutorial](https://blog.fabiopires.pt/running-your-instance-of-burp-collaborator-server/) ha sido de gran ayuda para poder crear esta entrada.

- [https://blog.fabiopires.pt/running-your-instance-of-burp-collaborator-server/](https://blog.fabiopires.pt/running-your-instance-of-burp-collaborator-server/)
- [https://portswigger.net/burp/documentation/collaborator/deploying](https://portswigger.net/burp/documentation/collaborator/deploying)

¡Hasta la próxima!!