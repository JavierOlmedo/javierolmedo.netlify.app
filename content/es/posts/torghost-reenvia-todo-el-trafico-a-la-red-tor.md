+++
date = "2018-04-12"
title = "TorGhost - Reenvía todo el tráfico a la red TOR"
author = "Javier Olmedo"
toc = false
+++

La red **TOR** es una **de las mejores opciones a la hora de proteger nuestra identidad** en la red, dado que sus métodos de transferencia de datos son **altamente criptográficos** y cada petición a un mismo servicio **se envía por rutas diferentes utilizando múltiples nodos,** algo que complica mucho trazar a un usuario dentro la red.

[![/images/torghost-reenvia-todo-el-trafico-a-la-red-tor/torghost-reenvia-todo-el-trafico-a-la-red-tor_001.png](/images/torghost-reenvia-todo-el-trafico-a-la-red-tor/torghost-reenvia-todo-el-trafico-a-la-red-tor_001.png)](/images/torghost-reenvia-todo-el-trafico-a-la-red-tor/torghost-reenvia-todo-el-trafico-a-la-red-tor_001.png)

Para navegar por la red TOR, bastaría con 📦[descargar su bundle](https://www.torproject.org/download/download-easy.html.en), pero, tenemos que tener en cuenta, que de esta manera **solo protegeríamos nuestra privacidad usando su navegador, el resto de tráfico no se enviaría por TOR.**

En esta entrada veremos cómo **redirigir todo el tráfico** a la red TOR mediante el uso de un script llamado [TorGhost](https://github.com/susmithHCK/torghost).

## 📄 Características de TorGhost

1. **Redirección de todo el tráfico de red hacía la red TOR**, es decir, cualquier conexión del equipo que intente conectarse a Internet pasará por ella.
2. **No se filtrará ningún ping**, lo que protege nuestra identidad.
3. **Fuerza a las aplicaciones a pasar por ella**, al contrario que proxychain, que es ignorado por algunas aplicaciones que tienden a una conexión más rápida ignorando los proxys.
4. **Rechaza peticiones** entrantes y salientes que puedan contener información sensible o pueda revelar nuestra IP real.
5. **Protección de fuga de DNS**, podemos usar un DNS remoto anónimo.

## 📥 Instalación de TorGhost

1. Clonar el repositorio a nuestro equipo.

```bash
git clone https://github.com/susmithHCK/torghost.git
```

2. Cambiamos al directorio que hemos clonado.

```bash
cd torghost
```

3. Damos permisos de ejecución al archivo de instalación.

```bash
chmod +x install.sh
```

4. Ejecutamos el archivo de instalación.

```bash
./install.sh
```

Todo lo tenemos preparado, para iniciarlo escribimos **torghost** en la terminal, tenemos 3 opciones:

[![/images/torghost-reenvia-todo-el-trafico-a-la-red-tor/torghost-reenvia-todo-el-trafico-a-la-red-tor_002.png](/images/torghost-reenvia-todo-el-trafico-a-la-red-tor/torghost-reenvia-todo-el-trafico-a-la-red-tor_002.png)](/images/torghost-reenvia-todo-el-trafico-a-la-red-tor/torghost-reenvia-todo-el-trafico-a-la-red-tor_002.png)

**Iniciar** TorGhost

```bash
torghost start
```

**Detener** TorGhost

```bash
torghost stop
```

**Cambiar nodo** de salida

```bash
torghost switch
```

Espero os sirva de ayuda.

Saludos!! 👋