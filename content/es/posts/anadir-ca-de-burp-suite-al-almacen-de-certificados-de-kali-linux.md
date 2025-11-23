+++
date = "2020-03-29"
title = "Añadir CA de Burp Suite al almacén de certificados de Kali Linux"
author = "Javier Olmedo"
toc = false
+++

Durante un programa privado de bug bounty, estuve utilizando las herramientas cURL, gobuster y sqlmap a través del proxy de Burp Suite. Como siempre y por vagancia, configuraba las herramientas para que ignoraran el certificado de PortSwigger (por ejemplo, la opción `-k` en cURL).

En caso de no añadir opciones de ignorar certificados a nuestras herramientas, podemos obtener errores del tipo **SSL certificate problem**.

[![/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_001.png](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_001.png)](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_001.png)

_Error de certificado al usar cURL a través del proxy de Burp Suite_

Configurar cada herramienta para ignorar el certificado de Burp Suite no es lo más óptimo. Por tanto, en esta entrada se muestra como **añadir la CA de Burp Suite al almacén de certificados de Kali Linux** para evitar problemas cuando usemos el proxy con otras herramientas.

Esto es aplicable a cualquier certificado y distribución de Linux.

## Obtener certificado de Burp

Podemos obtener el certificado de Burp Suite desde la opción `Proxy -> Options -> Proxy Listeners -> Import / export CA certificate`.

[![/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_002.png](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_002.png)](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_002.png)

_Panel de opciones de proxy_

Finalmente, seleccionamos el formato **DER** y lo guardamos en disco con el nombre **burp.der**.

[![/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_003.png](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_003.png)](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_003.png)

_Exportación de certificado en formato DER_

## Generar clave pública con OpenSSL

Si intentamos visualizar el contenido de burp.der, podemos observar que es un binario.

[![/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_004.png](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_004.png)](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_004.png)

_Visualización del archivo burp.der_

Para poder utilizar la clave pública contenida en dicho certificado (y firmada), podemos hacer uso de OpenSSL para generarla partiendo del certificado DER.

```bash
openssl x509 -in burp.der -inform DER -out burp.crt
```

Si ahora visualizamos el fichero generado con el comando anterior (**burp.crt**), vemos que se trata de una clave pública preparada para utilizar.

[![/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_005.png](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_005.png)](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_005.png)

_Visualización del archivo burp.crt_

## Guardar en almacén de certificados

Ahora que ya tenemos la clave pública de PortSwigger, sólo tenemos que moverla al almacén de certificado de Kali Linux.

```bash
mv burp.crt /usr/local/share/ca-certificates/burp.crt
```

Y actualizar los certificados.

```bash
update-ca-certificates
```

Con el último comando, se puede comprobar que la clave ha sido agregada correctamente.

[![/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_006.png](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_006.png)](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_006.png)

_Clave pública agregada correctamente al almacén de certificados_

Ahora, al utilizar el mismo comando del principio de la entrada, no obtenemos error de certificado.

[![/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_007.png](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_007.png)](/images/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux/anadir-ca-de-burp-suite-al-almacen-de-certificados-de-kali-linux_007.png)

_Petición cURL a través del proxy de Burp Suite_

## Borrar del almacén de certificados

⚠️ Si un futuro decidieras eliminar el certificado del almacén, sería tan simple como eliminarlo de la siguiente manera.

```bash
rm /usr/local/share/ca-certificates/burp.crt
```

Y finalmente actualizar el almacen (la opción `–fresh` indicará al sistema operativo que realice una actualización completa, incluida la eliminación de enlaces simbólicos que pudieran existir en el directorio `/etc/ssl/certs`).

```bash
update-ca-certificates --fresh
```

Un saludo a todos!!