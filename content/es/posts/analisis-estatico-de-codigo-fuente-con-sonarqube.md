+++
date = "2019-01-21"
title = "Análisis estático de código fuente con SonarQube"
author = "Javier Olmedo"
toc = false
+++

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_banner.png](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_banner.png)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_banner.png)

SonarQube es un conjunto de herramientas de código abierto con interfaz web que nos ayudará a realizar **análisis estáticos** a nuestro código fuente con el objetivo de mejorar la calidad, solucionar errores y buscar vulnerabilidades.

Actualmente, la [última versión es la 7.5](https://www.sonarqube.org/sonarqube-7-5/), y soporta más de 25 lenguajes de programación.

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_001.png](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_001.png)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_001.png)

## 🖥 1. Instalación

1.1 Descargar [SonarQube Community Edition 7.5](https://www.sonarqube.org/downloads/)

1.2 Crea una carpeta en `C:/sonarqube` y copia aquí los archivos descomprimidos.

1.3 Descarga e instala [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

1.4 Crear un archivo `sonarqube.bat` en el escritorio y añade lo siguiente (este archivo nos ayudará a arrancar sonarqube):

_Para sistemas 32bits_

```powershell
C:\sonarqube\bin\windows-x86-32\StartSonar.bat
```

_Para sistemas 64bits_

```powershell
C:\sonarqube\bin\windows-x86-64\StartSonar.bat
```

1.5 Descarga [SonarQube Scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner#AnalyzingwithSonarQubeScanner-Installation)

1.6 Crea una carpeta en `C:/sonarqube/scanner` y copia aquí los archivos.

1.7 Añade el directorio `C:/sonarqube/scanner/bin` a las variables del sistema, esto puedes hacerlo pulsando las teclas `Windows+R` y escribiendo:

```powershell
rundll32.exe sysdm.cpl,EditEnvironmentVariables
```

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_002.png](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_002.png)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_002.png)

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_003.gif](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_003.gif)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_003.gif)

1.8 Accede a [http://localhost:9000/sessions/new](http://localhost:9000/sessions/new) con las credenciales `admin/admin`

## 📊 2. Análisis estático de proyecto

2.1 Crea una carpeta en `C:/sonarqube/projects` y pega aquí el código fuente de tu software.

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_004.png](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_004.png)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_004.png)

2.3 Dentro del panel de SonarQube, en la parte superior derecha, haz click en el botón **+** para **analizar un nuevo proyecto.**

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_005.png](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_005.png)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_005.png)

2.4 Genera el **comando de ejecución del scanner** de la siguiente manera:

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_006.gif](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_006.gif)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_006.gif)

2.5 Dentro de la carpeta del proyecto, **ejecuta el comando copiado del paso anterior** y deja que termine de escanear el proyecto.

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_007.gif](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_007.gif)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_007.gif)

2.6 Una vez finalice, puedes **ver los resultados** en el panel de SonarQube.

[![/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_008.png](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_008.png)](/images/analisis-estatico-de-codigo-fuente-con-sonarqube/analisis-estatico-de-codigo-fuente-con-sonarqube_008.png)

Saludos!!