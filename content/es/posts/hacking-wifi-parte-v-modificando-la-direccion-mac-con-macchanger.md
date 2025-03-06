+++
date = "2015-04-09"
title = "Hacking Wifi - Parte V - Modificando la dirección MAC con macchanger"
author = "Javier Olmedo"
toc = false
+++

**La dirección MAC** de la tarjeta de red o dirección física, es un identificador de 48 [bits](https://es.wikipedia.org/wiki/Bit) (6 bloques [hexadecimales](https://es.wikipedia.org/wiki/Sistema_hexadecimal)) que identifica de manera inequívoca a un dispositivo de red, **es una de las maneras más fáciles de** rastrear e **identificar un equipo** al **no existir dos direcciones MAC iguales** en el mundo.

[![/images/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger_001.png](/images/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger_001.png)](/images/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger_001.png)

## ¿Como es posible que no se repitan y sean únicas?

El **Instituto de Ingeniería Eléctrica y Electrónica (IEEE)** es la que le asigna los **primeros 24 bits a los fabricantes de tarjetas de red y los otros 24 bits quedan a elección del fabricante** de tarjetas de red. Por supuesto bajo supervisión y respetando que no se repitan ninguna dirección MAC.

Por lo tanto, viendo mi direccion MAC de mi tarjeta de red inalámbrica:

```bash
00:C0:CA:12:12:12
```

Podemos deducir que el fabricante es ALFA INC, dado que los 3 primeros grupos identifican al fabricante (00:C0:CA) y el resto (12:12:12) la hace **única e irrepetible**, si tienes una tarjeta ALFA, tus 3 primeros grupos serán como la mía.

Os dejo un link donde podéis ver las MAC asociadas a los fabricantes [AQUÍ](https://standards-oui.ieee.org/oui/oui.txt).

Tengo que aclarar que **las direcciones MAC no se pueden cambiar** a nivel de hardware, pero sí de manera lógica, con esto quiero decir, que si te compras una tarjeta de red, **siempre tendrás la misma dirección MAC**, pero podemos cambiarla de manera temporal hasta que el ordenador se apage o reinicie mediante software como por ejemplo utilizando **macchanger**.

Macchanger **está en la mayoría de repositorios** de las distribuciones de Linux, para instalarlo basta con ir a la terminal y con **permisos de superusuario** escribir:

```bash
apt-get install macchanger
```

Los parámetros de macchanger más importantes son los siguientes:

- **-h** –> Muestra la ayuda.
- **-V** –> Muestra la versión.
- **-s** –> Muestra la dirección MAC actual.
- **-r** –> Nos configura una direccion MAC aleatoria.
- **-m** –> Configuramos una MAC personalizada (seguidamente escribiríamos la MAC XX:XX:XX:XX:XX:XX)
- **-l** –> Muestra las MAC de los fabricantes.

Y se utilizaría de la siguiente manera:

```bash
macchanger [parametro] [interfaz]
```

## Cambiando nuestra dirección MAC

Para cambiar la dirección MAC de nuestra tarjeta deberemos de ser **superusuario en nuestra terminal** y lo haremos de la siguiente manera:

1) Comprobamos la direccion MAC real.

```bash
macchanger -s [interfaz]
```

2) Antes de poder cambiar la MAC debemos de deshabilitar nuestra tarjeta.

```bash
ifconfig [interfaz] down
```

3) Indicamos que queremos cambiar la dirección MAC, podemos hacerlo de dos maneras, la primera de forma aleatoria.

```bash
macchanger -r [interfaz]
```

O indicando nosotros una especifica.

```bash
macchanger -m XX:XX:XX:XX:XX:XX [interfaz]
```

4) Habilitamos la tarjeta de nuevo para que esté operativa.

```bash
ifconfig [interfaz] up
```

5) Comprobamos que se ha cambiado correctamente.

```bash
macchanger -s [interfaz]
```

Ahora ya podemos utilizar nuestra tarjeta con la MAC modificada.

## ¿Porqué cambiar la dirección MAC?

Personalmente no suelo cambiar la direccion MAC, a menos que quiera saltarme un filtro de seguridad que incluyen los routers para **denegar** el acceso a Internet a las direcciones MAC que no estén en su **«lista de conocidos»** (MAC Filtering), incluso cuando **conocen la clave o están en red**. La mayoría de la gente no suele trabajar con este filtro, pero es muy importante, pues en el caso que alguien **consiguiera crackear** nuestra clave del router, no podría acceder a Internet (veremos mas adelante como saltarnos el filtrado utilizando **Wireshark y Macchanger** en otra entrada).

Los motivos para querer cambiar la dirección MAC de nuestra tarjeta son por tanto **normalmente “oscuros o malignos”**, aunque está claro que uno de los objetivos es el de **poder realizar tareas de auditoría WiFi de forma “legal”** para saber si nuestra red está realmente segura. Sin embargo, **quien cambia la MAC de su tarjeta normalmente es porque está auditando otras redes…** con o sin permiso (recuerda que puede ser un delito).

[![/images/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger_002.png](/images/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger_002.png)](/images/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger/hacking-wifi-parte-v-modificando-la-direccion-mac-con-macchanger_002.png)

Saludos y no seáis malos!!!!!