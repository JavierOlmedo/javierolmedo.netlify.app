+++
date = "2015-03-11"
title = "Detectando aplicaciones maliciosas para Android en Google Play"
author = "Javier Olmedo"
toc = false
+++

[![/images/detectando-aplicaciones-maliciosas-para-android-en-google-play/detectando-aplicaciones-maliciosas-para-android-en-google-play_banner.jpg](/images/detectando-aplicaciones-maliciosas-para-android-en-google-play/detectando-aplicaciones-maliciosas-para-android-en-google-play_banner.jpg)](/images/detectando-aplicaciones-maliciosas-para-android-en-google-play/detectando-aplicaciones-maliciosas-para-android-en-google-play_banner.jpg)

Una de las principales formas de prevenir una infección con malware en dispositivos con sistema operativo Android es no descargar e instalar aplicaciones que no sean de Google Play, es decir, no utilizar repositorios no oficiales, como solemos decir. Pero, ¿es esta medida suficiente?

En el día de ayer estuvimos cubriendo la décima edición de [SEGURINFO](https://www.welivesecurity.com/la-es/tag/segurinfo/) 2015 en Argentina, donde la exposición de cierre quedó en manos de un carismático disertante como es **Chema Alonso**, con su presentación “Can I Play with madness?” (en honor a Iron Maiden). En ella abordó temas referidos a aplicaciones maliciosas dentro de Google Play, combinados con divertidas ironías que hicieron reír a la totalidad de la sala -tan colmada que hasta había gente sentada en el piso.

## Identificar apps maliciosas Google Play, ¿encontrar la aguja en el pajar?

Debido al gran volumen de aplicaciones subidas a diario, los controles en la detección de códigos maliciosos deben realizarse de forma automatizada para que los desarrolladores puedan subir su contenido de una forma sencilla y rápida. Este tipo **auditoría** es explotada por ciberdelincuentes para **saltear** de forma sencilla las protecciones y distribuir eficientemente sus programas maliciosos.

Muchas de las aplicaciones mostradas en la presentación están ligadas a algunas ya conocidas de redes sociales utilizando nombres muy similares en busca de usuarios distraídos, aplicaciones como **falsos antivirus,** o inclusive aplicaciones que imitan la visión de rayos X, entre otras. Porque, como dijo Chema Alonso, ¿quién quiere pagar en un hospital si puede bajarse una app para hacer sus propias radiografías? Ni hablar de aquellos “exigentes” que quieren que los antivirus no tengan falsos positivos y los protejan de absolutamente todas las amenazas… en forma gratuita. Para su fortuna, ironizó, siempre habrá **cibercriminales** que les ofrecerán su antivirus gratuito (con algunos defectos, claro).segurinfo_chema_alonso (2)

También se demostró cómo es posible volver a una aplicación maliciosa luego de una actualización de la misma, es decir, los delincuentes compran un desarrollo que ya se encuentra instalado en miles de dispositivos ycambian el código, logrando que en la posterior actualización, todos estos usuarios sean víctimas de un contenido malicioso.

[![/images/detectando-aplicaciones-maliciosas-para-android-en-google-play/detectando-aplicaciones-maliciosas-para-android-en-google-play_001.jpg](/images/detectando-aplicaciones-maliciosas-para-android-en-google-play/detectando-aplicaciones-maliciosas-para-android-en-google-play_001.jpg)](/images/detectando-aplicaciones-maliciosas-para-android-en-google-play/detectando-aplicaciones-maliciosas-para-android-en-google-play_001.jpg)

Para darle más credibilidad a estas aplicaciones, los ciberdelincuentes poseen redes del tipo [botnet](https://www.welivesecurity.com/2017/12/04/eset-takes-part-global-operation-disrupt-gamarue/) con usuarios capturados y tokens de seguridad que utilizan para dar **calificaciones positivas** en la tienda, e inclusive generar comentarios favorables.

Sumergiéndose en este ecosistema, Chema Alonso investigó la detección de **anomalías** y singularidades dentro de las aplicaciones que integran la Google Play Store, mediante un esquema de [Big Data](https://www.welivesecurity.com/la-es/tag/big-data-la-es/). Con esta herramienta, descargó todas las existentes día a día para la correlación de datos y la producción de distintos patrones entre sus componentes y características.

Al diseñar **crawlers** que diariamente indexen las alteraciones sufridas por las aplicaciones disponibles en esta plataforma, es posible construir una base de datos capaz para sostener el seguimiento de su evolución a lo largo del tiempo, prestando principalmente atención a aquellas que por algún motivo han desaparecido de la tienda.

El principal beneficio de soluciones como la propuesta se sintetiza en la posibilidad de tomar un enfoque proactivo en lo que respecta a defensa de usuarios de **Android**, evaluando aplicaciones maliciosas aún activas en mercados de aplicaciones legítimos.

Según se necesite, los ingenieros de seguridad generan **filtros** a partir de estas singularidades encontradas, dando pie a la localización de todas las aplicaciones que pertenezcan a una operación de cibercrimen. Estos filtros pueden ser la utilización de determinados **permisos**, las cuentas de desarrollador vinculadas, fechas y nombres similares de ficheros o cualquier otra característica especial que pueda servir para juzgar estas apps como maliciosas o sospechosas.

A medida que van cayendo dentro de las categorías asignadas por los filtros, se separan para un análisis minucioso en manos de los analistas de malware. Esta metodología permite dejar la **ingeniería reversa** de código como último recurso, ahorrando tiempo en el análisis masivo de potenciales amenazas.

Si bien existe una gran cantidad de aplicaciones maliciosas, esto no significa que realmente sean del tipo troyano; esto significa que entre muchos posibles comportamientos maliciosos, a modo de ejemplo, simplemente podrían robar ancho de banda de un teléfono generando publicidades y terminando con el crédito de transferencia de Internet.

Por lo expuesto anteriormente, debemos mencionar la importancia de tener una solución de seguridad en los dispositivos móviles como es el caso del [ESET Mobile Security](https://www.eset.com/mx/hogar/mobile-antivirus-para-android/), y de tener especial cuidado con las aplicaciones desconocidas que se instalan y que solicitan permisos excesivos. En este sentido, será de utilidad tener en cuenta estos [8 consejos para determinar si una aplicación para Android es legítima](https://www.welivesecurity.com/la-es/2013/07/24/8-consejos-determinar-aplicacion-android-legitima/).

Por otra parte, si los dispositivos se encuentran en un entorno corporativo, es recomendable utilizar un Mobile Device Management (MDM), un software de control que podrá bloquear la instalación de cualquier tipo de aplicación sospechosa o con las características anteriormente mencionadas.

Autor [Lucas Paus](https://www.welivesecurity.com/es/author/lpaus/), ESET