+++
date = "2020-10-19"
title = "reNgine – Framework de reconocimiento automatizado para aplicaciones web"
author = "Javier Olmedo"
toc = false
+++

## 1. Sobre reNgine

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_001.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_001.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_001.png)

Dashboard de la aplicación reNgine

reNgine es un **framework de reconocimineto automatizado para aplicaciones web** utilizado para realizar la fase de recopilación de información durante las auditorías, cuenta con varios motores de escaneo personalizables que pueden ayudarnos para:

- Escanear puertos en los servidores de aplicaciones.
- Enumerar subdominios.
- Buscar directorios y archivos.
- Encontrar endpoints.
- Tomar capturas de pantalla.

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_002.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_002.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_002.png)

Diagrama de flujo de reNgine

Lo realmente importante de reNgine, es que nos permite reunir **toda la información en un solo lugar**, además, podemos hacer búsquedas en los resultados para, por ejemplo, **obtener todos los subdominios que usen PHP y su estado sea 200 OK.**

{{< youtube u8_Z2-3-o2M >}}
Demostración sobre el uso de reNgine

reNgine ha sido desarrollado por [Yogesh Ojha](https://twitter.com/ojhayogesh11) y podemos encontrarlo en su repositorio de [GitHub](https://github.com/yogeshojha/rengine).

## 2. Instalación

Antes de realizar la instalación, debemos de asegurarnos de tener **docker** instalado en el equipo.

### 2.1 Prerrequisitos

#### Instalación de docker

Puedes seguir los pasos del siguiente [tutorial](https://hackpuntes.com/instalacion-de-docker-en-kali-linux/) para instalar docker en tu equipo, te recomiendo que instales también **DockStation**.

#### Docker-compose

En linux, la instalación puede realizarse de la siguiente manera:

1) Descargamos la última versión estable.

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2) Damos permiso de ejecución al binario.

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3) Comprobamos si está instalado correctamente mostrando la versión.

```bash
docker-compose --version
```

Para otros sistemas operativos, puedes consultar la documentación oficial [aquí](https://docs.docker.com/compose/install/).

### 2.2 Instalación con docker

#### Descargar desde el repositorio de Github

```bash
git clone https://github.com/yogeshojha/rengine.git
```

```bash
cd rengine
```

#### Edición del archivo `.env`

Con cualquier editor de texto, cambiaremos la configuración por defecto de reNgine para generar nuestros certificados.

```bash
nano .env
```

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_003.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_003.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_003.png)

Configuración del archivo .env

#### Generar certificados

```bash
make certs
```

Una vez los tengamos creados, podemos ejecutar reNgine con https.

#### Compilar reNgine

```bash
make build
```

Esto puede tardar un tiempo.

```bash
make pull
```

Una vez terminado, si hacemos uso de DockStation podremos ver los contenedores en él.

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_004.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_004.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_004.png)

Contenedores de reNgine en DockStation

## 3. Uso

### 3.1 Iniciar reNgine

Si el proceso de instalación no dió ningún error, podemos arrancar reNgine con el siguiente comando (o pulsadon el botón «run» en DockStation).

```bash
make up
```

Accedemos a la aplicación en **[https://127.0.0.1](https://127.0.0.1/)**

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_005.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_005.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_005.png)

Panel de acceso a reNgine

### 3.2 Crear cuenta de usuario

Si ya hemos accedio al panel de autenticación para acceder a la aplicación, nos habremos percatado que no disponemos de usuario, para crear una cuenta de usuario, lanzamos el siguiente comando y seguimos los pasos.

```bash
make username
```

### 3.3 Primer escaneo

Para realizar el primer escaneo, previamente es necesario **añadir el target o lista de dominios**, esto podemos hacerlo desde el menú Targets -> Add Target.

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_006.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_006.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_006.png)

Agregar target

Una vez agregado el host, podremos visualizarlo dentro de **List Targets** y lanzar un escaneo rápido.

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_007.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_007.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_007.png)

Escaner de dominio

Podremos seleccionar entre **3 tipos de escaneos por defecto** (aunque pueden personalizarse dentro del menú **Scan Engine**), son los siguientes:

**Full Scan**

- Enumeración de subdominios
- Escaner de puertos
- Toma de control de subdominio
- Fuzzing de directorios y archivos
- Búsqueda de endpoints

**Passive Scan**

- Enumeración de subdominioos
- Toma de control de subdominios
- Búsqueda de endpoints

**Subdomain Scan**

- Enumeración de subdominios

Al final del escaneo podremos ver un listado como el siguiente, en el cual podemos aplicar búsquedas.

[![/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_008.png](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_008.png)](/images/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web/rengine-framework-de-reconocimiento-automatizado-para-aplicaciones-web_008.png)

Panel de historial de escaneos

Hasta la próxima….