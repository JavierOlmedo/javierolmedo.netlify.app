+++
date = "2019-05-17"
title = "Smalisca: Parsing, análisis y gráficos de código Smali"
author = "José María Arenas"
toc = false
+++

En la entrada de hoy, vamos a mostrar la herramienta [Smalisca](https://github.com/dorneanu/smalisca). Esta herramienta es de gran utilidad para el análisis de aplicaciones Android (y otro tipo de aplicaciones), ya que nos permitirá **obtener los datos** de las distintas clases de la aplicación y exportarlos a formato SQLite. También permite realizar una serie de **operaciones de búsqueda** sobre dichos datos atendiendo a los siguientes criterios:

1. Clases
2. Propiedades
3. Métodos
4. Cadenas
5. Llamadas (tanto origen como destino de una llamada)

Finalmente, se pueden **generar distintos diagramas** en los que se puede ver la relación entre clases.

El primer paso para instalar la herramienta es clonar el git e instalar dependencias con pip:

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_001.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_001.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_001.png)

Instalación de la herramineta y las dependencias

Para finalizar la instalación de la herramienta, ejecutaremos el siguiente comando:

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_002.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_002.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_002.png)

Instalación de la herramienta

Finalmente comprobamos que la herramienta está lista para su utilización utilizando la ayuda de la propia herramienta:

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_003.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_003.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_003.png)

Comprobación de que la herramienta está correctamente instalada

Antes de comenzar el análisis de la aplicación, es necesario parsearla. Para ello utilizaremos el siguiente comando:

```bash
smalisca parser -l ../ContactManager/smali/ -s smali -f sqlite -o ContactManager.sqlite
```

Argumentos:

- parser -> comando que se va a utilizar, en este caso es el de parseo
- -l -> ruta a la carpeta raíz que contiene el código Smali de la aplicación (se explica en [esta entrada](https://hackpuntes.com/que-es-smali-y-como-parchear-una-aplicacion-android/) de este mismo blog)
- -s -> extensión de los archivos que se deben parsear. En el caso de smali, el sufijo es smali
- -f -> formato de la salida
- -o -> nombre del archivo que contendrá los resultados del parseo

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_004.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_004.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_004.png)

Ejemplo de parseo de una aplicación

Para más información sobre el proceso de parseo, consultar la [documentación oficial](https://smalisca.readthedocs.io/en/latest/parsing.html).

Una vez realizado el parseo, se puede pasar al análisis. Para entrar al modo interactivo del análisis, ejecutaremos el siguiente comando:

```bash
smalisca analyzer -i ContactManager.sqlite -f sqlite
```

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_005.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_005.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_005.png)

Comando para entrar en el modo de análisis

Una vez dentro del modo interactivo de análisis, se pueden realizar diferentes acciones como por ejemplo, listar todas las clases de la aplicación con el comando `sc`:

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_006.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_006.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_006.png)

Listado de todas las clases de la aplicación

Otra opción es la de búsqueda de métodos. Por ejemplo, podemos buscar los métodos que contengan «Auth» en su nombre con el siguiente comando:

```bash
sm -c method_name -p Auth
```

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_007.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_007.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_007.png)

Búsqueda de los métodos que contienen `Auth` en su nombre

La lista completa de operaciones que se pueden realizar es la siguiente:

- s -> (search) búsqueda en todas las tablas (clases, métodos, etc)
- sc -> (search classes) búsqueda de clases
- sm -> (search methods) búsqueda de métodos
- sp -> (search properties) búsqueda por propiedades
- scs -> (search constant strings) búsqueda por cadenas estáticas
- scl -> (search for calls) búsqueda por llamadas
- sxcl -> (search for cross calls) Como scl, pero permite buscar llamadas que refiere a una clase y/o método o a llamadas invocadas desde una clase y/o método

Para más información sobre el proceso de análisis, consultar la [documentación oficial](https://smalisca.readthedocs.io/en/stable/analysis.html).

Finalmente se pueden usar las opciones de generación de gráficos. Para ello contamos con las opciones dc, dcl y dxcl, equivalentes a sc, scl y sxcl, respectivamente. A modo de ejemplo, vamos a generar un gráfico de los métodos a los que se llaman desde el método getAuthenticatorDescription con el siguiente comando:

```bash
dxcl -m getAuthenticatorDescription -d from --max-depth 1 -f png -o from_getAuth
```

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_008.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_008.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_008.png)

Generación del gráfico asociado a los métodos que se llaman desde el método getAuthenticatorDescription

Los argumentos del comando son los siguientes:

- dxcl ->(draw cross calls) comando para generar gráfico de llamadas cruzadas a métodos
- -m -> nombre del método
- -d -> permite especificar el sentido de la llamada. Los dos valores posibles son **to** y **from**, se refieren a llamadas **hacia** este método o **desde** este método, respectivamente.
- –max-depth -> número máximo de profundidad en la búsqueda de llamadas
- -f -> formato de la imagen que se va a generar
- -o -> nombre del archivo que se va a generar (sin extensión)

Este comando genera la siguiente imagen:

[![/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_009.png](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_009.png)](/images/smalisca-parsing-analisis-y-graficos-de-codigo-smali/smalisca-parsing-analisis-y-graficos-de-codigo-smali_009.png)

Gráfico generado que muestra las llamadas que se hacen desde el método getAuthenticatorDescription

Para más información sobre el proceso de generación de gráficos, consultar la [documentación oficial](https://smalisca.readthedocs.io/en/stable/drawing.html).

Smalisca también cuenta con una API REST que permite consultar toda la información en formato JSON mediante peticiones HTTP. Más información sobre la API en la [documentación oficial](https://smalisca.readthedocs.io/en/stable/web-api.html).

Agradecer a todos vuestra atención y un agradecimiento especial a **Javier Olmedo** por darme de nuevo la oportunidad de publicar en su blog.