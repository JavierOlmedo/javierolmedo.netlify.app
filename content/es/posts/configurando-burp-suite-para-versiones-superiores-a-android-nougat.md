+++
date = "2019-02-24"
title = "Configurando Burp Suite para versiones superiores a Android Nougat"
author = "Javier Olmedo"
toc = false
+++

En las últimas semanas, estoy auditando varias aplicaciones de Android con Burp Suite, después de unos días de trabajo con mi vieja versión de Android 5.0 pensé que ya era hora de actualizar a una superior, y quizás, utilizar nuevas herramientas, aquí me tope con [Genymotion](https://www.genymotion.com/) gracias a un compañero de trabajo.

Una vez instalado y configurado Burp Suite con Android tal y como nos recomiendan en la web de [Portswigger](https://support.portswigger.net/customer/portal/articles/1841102-installing-burp-s-ca-certificate-in-an-android-device), me encontré con el siguiente problema:

> The client failed to negotiate an SSL connection to [DOMAIN:443] Received a fatal alert: certificate_unknown

[![/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_001.png](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_001.png)](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_001.png)

Errores mostrados por Burp Suite

Android, parecía no aceptar el certificado de Portswigger e impedía la navegación SSL, después de un rato buscando por Internet una solución al problema, me dí cuenta que las últimas versiones de Android, en concreto [a partir de la API >= 24 (Nougat)](https://android-developers.googleblog.com/2016/07/changes-to-trusted-certificate.html), sólo aceptan un certificado si este se encuentra en las **CA a nivel de sistema** o si se ha declarado expresamente en el fichero `AndroidManifest.xml` de configuración de la APK.

Para solucionar este problema, puedes seguir los siguientes pasos:

## Visualizar los certificados

Para visualizar los certificados a nivel de sistema, necesitaremos descargarnos las  
[Platform Tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) y conectarnos por `ADB`.

```bash
adb devices
adb connect [IP:PORT]
adb shell
```

Las CA de confianza para Android, se almacenan en un **formato especial** en la ruta: `/system/etc/security/cacerts`

[![/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_002.png](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_002.png)](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_002.png)

Visualización de los certificados del sistema en Android

## Exportar y convertir la CA de Burp Suite

Como hemos visto al listar los certificados, Android requiere que el certificado esté en formato `PEM` y que tenga el nombre igual al `subject_hash_old` añadiendo al final `.0`

El primer paso, sería exportar el certificado de Burp Suite en formato `DER`

[![/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_003.png](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_003.png)](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_003.png)

Exportación de CA Burp Suite

Seguidamente, necesitamos **convertir** la CA de Burp (en formato `DER`) a un formato `PEM`, esto podemos hacerlo con **OpenSSL**

```bash
openssl x509 -inform DER -in cacert.der -out cacert.pem
```

Obtenemos el `subject_hash_old` y **renombramos** el archivo

```bash
openssl x509 -inform PEM -subject_hash_old -in cacert.pem |head -1
mv cacert.pem [SUBJECT-HASH-OLD].0
```

[![/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_004.png](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_004.png)](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_004.png)

Conversión de formato `DER` a `PEM` son `subject_hash_old`

## Añadir el certificado al dispositvo

Volvemos a hacer uso de ADB, al tener que copiar el archivo dentro del sistema de archivos (`/system`) previamente debemos de montarlo.

```bash
adb root
adb remount
adb push [SUBJECT-HASH-OLD].0 /sdcard/
```  

[![/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_005.png](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_005.png)](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_005.png)

Copia del certificado desde PC a dispositivo Android

Por último, **abrimos una shell** en el dispositivo, movemos el certificado a la carpeta `/system/etc/security/cacerts` y cambiamos los permisos a **644**

```bash
adb shell
mv /sdcard/[SUBJECT-HASH-OLD].0 /system/etc/security/cacerts/
chmod 644 /system/etc/security/cacerts/[SUBJECT-HASH-OLD].0 
```

[![/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_006.png](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_006.png)](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_006.png)

Copia del certificado a system y establecimiento de permisos

Finalmente, reiniciamos el dispositivo

```bash
reboot
```

Y podemos ver la CA de Burp Suite a nivel de sistema en el disposivito

[![/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_007.png](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_007.png)](/images/configurando-burp-suite-para-versiones-superiores-a-android-nougat/configurando-burp-suite-para-versiones-superiores-a-android-nougat_007.png)

Certificado de Burp Suite a nivel de sistema

Espero que os haya sido de ayuda!!