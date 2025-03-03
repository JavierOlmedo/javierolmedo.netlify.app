+++
date = "2025-03-03"
title = "Snyk Fetch The Flag - Unfurl"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_banner.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_banner.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_banner.png)

En este reto nos enfrentamos a una aplicación web que **obtiene los metadatos** de cualquier URL ingresada en el formulario.

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_001.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_001.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_001.png)

Un análisis rápido del código fuente revela que la aplicación está desarrollada en **NodeJS**, y el uso del módulo `child_process` sugiere la posibilidad de una **ejecución remota de comandos (RCE)**.

📄 Archivo `adminRoutes.js`

```javascript
const { exec } = require('child_process');

router.get('/execute', (req, res) => {
    // This isn't terribly secure, but we're only going to bind this app to the localhost so you'd need to be on the actual host to run any commands.
    // So I think we're good!
    const clientIp = req.ip;

    // Definitely making sure to lock this down to the localhost
    if (clientIp !== '127.0.0.1' && clientIp !== '::1') {
        console.warn(`[WARN] Unauthorized access attempt from ${clientIp}`);
        return res.status(403).send('Forbidden: Access is restricted to localhost.');
    }

    const cmd = req.query.cmd;

    if (!cmd) {
        return res.status(400).send('No command provided!');
    }

    exec(cmd, (error, stdout, stderr) => {
        if (error) {
            console.error(`[ERROR] Command execution failed: ${error.message}`);
            return res.status(500).send(`Error: ${error.message}`);
        }

        console.log(`[INFO] Command executed: ${cmd}`);
        res.send(`
            <h1>Command Output</h1>
            <pre>${stdout || stderr}</pre>
            <a href="/admin">Back to Admin Panel</a>
        `);
    });
});
```

El código indica que la aplicación permite ejecutar comandos en el endpoint `/execute` a través del parámetro `cmd`, pero solo **localmente**.

Revisando el código, encontramos una función que genera un **puerto aleatorio entre 1024 y 4999**, excluyendo el puerto **5000**.

📄 Archivo `admin.js`

```javascript
// This should keep people away from the admin panel!
function getRandomPort() {
    const MIN_PORT = 1024;
    const MAX_PORT = 4999;
    let port;
    do {
        port = Math.floor(Math.random() * (MAX_PORT - MIN_PORT + 1)) + MIN_PORT;
    } while (port === 5000);
    return port;
}

const adminPort = getRandomPort();
adminApp.listen(adminPort, '127.0.0.1', () => {
    console.log(`[INFO] Admin app running on http://127.0.0.1:${adminPort}`);
});
```

Intentamos obtener los metadatos con `http://127.0.0.1:5000` y funciona, pero al ejecutar comandos en `http://127.0.0.1:5000/execute?cmd=whoami`, obtenemos el error 🔴 **"Failed to unfurl the URL"**.

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_002.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_002.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_002.png)

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_003.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_003.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_003.png)

La clave está en descubrir el puerto aleatorio generado. Para ello, utilizamos **Burp Suite** con `Intruder` para realizar **fuzzing**.

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_004.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_004.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_004.png)

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_005.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_005.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_005.png)

El escaneo revela que el puerto **1064** responde con un código `200 OK` y devuelve el HTML del panel de administración.

Finalmente, probamos la ejecución de comandos en este puerto y logramos obtener la bandera.

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_006.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_006.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_006.png)

[![/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_007.png](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_007.png)](/images/snyk_fetch_the_flag_unfurl/snyk_fetch_the_flag_unfurl_007.png)