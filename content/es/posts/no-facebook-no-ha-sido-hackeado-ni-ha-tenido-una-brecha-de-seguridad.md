+++
date = "2021-04-05"
title = "No, Facebook no ha sido hackeado ni ha tenido una brecha de seguridad"
author = "Javier Olmedo"
toc = false
+++

Recientemente, ha saltado una noticia en diversos medios de comunicación y RRSS sobre una supuesta brecha de seguridad en Facebook que expondría la información de **533 millones de personas** de todo el mundo, en concreto, sus números telefónicos y direcciones.

Esta entrada tiene como finalidad **dar a conocer el impacto real** de dicha brecha de seguridad (desde mi punto de vista) a aquellos usuarios que no tienen la posibilidad de acceder a ella o han sido mal informados por ciertos medios de comunicación o «Gurús» de la ciberseguridad de este país.

## La supuesta filtración de seguridad

El tema de la ciberseguridad da mucho que hablar, y más si hace apenas un mes nos hackearon nuestro **Servicio Público de Empleo Estatal** (esta si fue buena brecha), por lo tanto, una noticia como esta puede entenderse como muy alarmante cuando realmente puede que no lo sea tanto.

Después de ver algunas capturas por foros del supuesto «leak», he podido averiguar que consta de **108 archivos comprimidos** (uno por cada país filtrado).

Una vez descargados los ficheros comprimidos, puede obtenerse un fichero en **texto plano** que contiene la información estructurada de la siguiente manera:

**Teléfono**:**ID_Facebook**:**Nombre**:**Apellidos**:**Sexo**:**Localidad**:**Provincia**:**País**:**Email**:**Profesión**:**Fecha_nacimiento**

[![/images/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad_001.png](/images/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad_001.png)](/images/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad_001.png)

Ejemplo de datos obtenidos del fichero spain.txt

## ¿Por qué entiendo que no es una brecha de seguridad?

Según la AEPD (Agencia Española de Protección de Datos), una brecha de seguridad es un **incidente de seguridad** que afecta a **datos de carácter personal**. Este incidente puede tener un origen accidental o intencionado y además puede afectar a datos tratados digitalmente o en formato papel. En general, se trata de un suceso que ocasione destrucción, pérdida, alteración, comunicación o **acceso no autorizado a datos personales**.

Como se ha podido comprobar en la imagen anterior de la brecha, existen datos como el **número de teléfono** que es de carácter personal pero, esto no implica que dicho número **se haya obtenido debido a un incidente de seguridad**.

Facebook permite que las personas puedan ser encontradas a través de su número de teléfono, esta configuración, por defecto viene desactivada (aunque hace un tiempo se establecía como activada si creabas tu cuenta desde un dispositivo móvil) y podemos configurarla dentro del menú **Configuración –> Privacidad**.

[![/images/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad_002.png](/images/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad_002.png)](/images/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad/no-facebook-no-ha-sido-hackeado-ni-ha-tenido-una-brecha-de-seguridad_002.png)

Configuración de búsqueda por número de teléfono

Auque **no es posible obtener un número de teléfono partiendo de una cuenta de Facebook**, si es posible obtener una cuenta de Facebook partiendo de un número de teléfono. Entendido esto, un **usuario o grupo de usuarios** podrían haber utilizado el buscador de Facebook para conocer de quién es el número de teléfono 666999000, 666999001, 666999002 y así sucesivamente. De esta manera, sería posible extraer todos los datos que aparecen en el leak, **sin comprometer la seguridad** de Facebook.

## ¿Qué debo de conocer si comparto mis datos en RRSS?

Si bien he aclarado que el leak de Facebook no es realmente un leak, si debemos de tener en cuenta que los datos que proporcionamos a las RRSS u otros servicios **pueden ser utilizados de forma maliciosa contra nosotros**. Por ejemplo, si recibimos un SMS con una URL sospechosa, es más probable que accedamos a él si este contiene alguna información sobre nosotros (email, fecha de nacimiento, nombre, apellidos), y aquí es donde viene el verdadero impacto de esta brecha, que **dichos datos puedan ser utilizados en campañas de phising**.

## Conclusión

Elimina la búsqueda por número de teléfono dentro de la configuración de tu perfil y evita proporcionar datos personales en RRSS.