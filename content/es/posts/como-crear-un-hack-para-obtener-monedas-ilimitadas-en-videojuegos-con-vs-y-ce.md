+++
date = "2018-07-15"
title = "Como crear un hack para obtener monedas ilimitadas en videojuegos con Visual Studio y Cheat Engine"
author = "Javier Olmedo"
toc = false
+++

En esta entrada, vamos a aprender como **crearnos nuestro propio hack** para videojuegos con el objetivo de obtener 💰 monedas ilimitadas, para el ejemplo, voy a crear un hack para el videojuego  **«El Señor del Olimpo Zeus»**, uno de mis juegos favoritos. El hack, lo desarrollaremos en **Visual Basic** con **Visual Studio** y **Cheat Engine** para encontrar la dirección de memoria dónde el videojuego almacena las monedas. Comentaros, que he elegido este videojuego por lo simple que resulta buscar las direcciones de memoria (son estáticas), en otros videojuegos, podemos encontrarnos direcciones de memoria dinámicas (cambian cada vez que se ejecuta el videojuego), las cuales se precisa **encontrar los punteros** previamente para que el hack funcione, aunque los pasos son exactamente los mismos para encontrarlas, demora demasiado tiempo, quizás, en otra ocasión me anime a hacer un vídeo cuando tenga más tiempo. Os dejo este [enlace](https://es.wikipedia.org/wiki/Asignaci%C3%B3n_de_memoria) por si queréis mirar más a fondo.

## 🛠️ 1. Herramientas necesarias

1.1 Instalar el Entorno de Desarrollo Integrado (IDE) [Visual Studio Community 2107](https://visualstudio.microsoft.com/es/downloads/)

1.2 [Instalar Cheat Engine](https://www.cheatengine.org/)

## 🔍 2. Encontrar la dirección de memoria que almacena las monedas con Cheat Engine

2.1 Ejecutamos el juego (recomendado abrirlo en modo ventana) y localizamos el valor a modificar (5000).

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_001.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_001.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_001.png)

2.2 Abrimos Cheat Engine y buscamos el proceso en ejecución del juego.

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_002.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_002.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_002.png)

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_003.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_003.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_003.png)

2.3 Vamos a proceder a buscar la dirección de memoria que almacena el valor 5000, en la parte **Value:** agregamos el valor **5000** y damos **click** en **First Scan**.

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_004.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_004.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_004.png)

2.4 Nos ha encontrado **19 direcciones de memoria** que usa el proceso del juego para guardar variables con el valor **5000**, estas direcciones, tenemos que ir descartándolas hasta obtener la dirección real que guarda el valor ¿Que como se descartan? Si nos fijamos ahora, ya no aparece el botón **First Scan**, aparece otro llamado **Next Scan** (ahora os explico).

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_005.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_005.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_005.png)

2.5 El objetivo en este punto, es **modificar el valor en el juego**, por ejemplo, vamos a gastar monedas (Me quedo ahora con **4910**).

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_006.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_006.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_006.png)

2.6 La finalidad del botón **Next Scan**, es indicarle a Cheat Engine que me busque aquella dirección de memoria que, en un principio tenía el valor **5000** y ahora tiene el valor **4910**, por lo tanto, cambiamos el **Value:** a **4910** y damos click en el boton **Next Scan** (dependiendo del juego, este proceso tendremos que seguirlo varias veces hasta dar con la dirección de memoria). Ahora, ya he encontrado 3 direcciones que guardan ese valor, voy a quedarme con la primera (**00EF2E74**), aunque me valdría cualquiera de las 3 ya que he probado a cambiar el valor en el juego y cambian su **Value** en todas.

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_007.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_007.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_007.png)

## 👨‍💻 3. Desarrollo del Hack en Visual Basic con Visual Studio 2017

3.1 En esta parte, vamos a programar el hack, abrimos Visual Studio 2017 y vamos a **Archivo, Nuevo, Proyecto.**

3.2 Elegimos un proyecto de **Visual Basic**, **Aplicación de Windows Forms (.NET Framework)**.

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_008.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_008.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_008.png)

3.3 Una vez creado el proyecto, vamos a **crear una clase** que nos **permita leer y escribir en las direcciones de memoria**, **click derecho** sobre la solución (panel de la derecha) y elegimos **Agregar, Nuevo elemento.**

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_009.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_009.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_009.png)

3.4 Elegimos **Clase de Visual Basic** y ponemos un nombre, por ejemplo, **ReadWritingMemory.vb** y pegamos el código de [aquí.](https://raw.githubusercontent.com/JavierOlmedo/Zeus-Hack/master/ZeusCheats/ReadWritingMemory.vb)

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_010.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_010.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_010.png)

3.5 Modificamos el form a nuestro gusto, yo he creado dos botones (uno que lanza una ventana sobre el hack y otro para modificar las monedas), un label que me indica si el proceso está ejecutándose y un TextBox para añadir la cantidad de monedas a hackear, también he añadido algunos recursos como la imagen de fondo del form y un Dracma, el aspecto que queda es este:

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_011.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_011.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_011.png)

3.6 Agregar el [código](https://raw.githubusercontent.com/JavierOlmedo/Zeus-Hack/master/ZeusCheats/Form1.vb) principal del programa, en `Form1.vb`, **click derecho** y **Ver código** (o pulsando F7) lo tengo comentado para que echéis un vistazo, cualquier duda, comentadme.

3.8 Iniciamos y probamos el hack.

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_012.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_012.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_012.png)

## 👁️‍🗨️ 4. Código fuente del proyecto

[![/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_013.png](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_013.png)](/images/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce/como-crear-un-hack-para-obtener-monedas-ilimitadas-en-videojuegos-con-vs-y-ce_013.png)

https://github.com/JavierOlmedo/Zeus-Hack

Hasta la próxima👋