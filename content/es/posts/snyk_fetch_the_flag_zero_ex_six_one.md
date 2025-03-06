+++
date = "2025-03-02"
title = "Snyk Fetch The Flag - Zero Ex Six One"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_banner.png](/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_banner.png)](/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_banner.png)

En este desafío obtenemos un archivo llamado `flag.txt.encry`, el cual contiene una cadena en formato hexadecimal.

```bash
$ cat flag.txt.encry
0x070x0d0x000x060x1a0x020x540x510x050x590x530x020x510x000x530x540x070x520x040x570x550x550x050x510x560x510x530x030x550x500x050x030x050x510x590x540x000x1c
```

La 💡 pista en la descripción del reto sugiere que se ha utilizado un cifrado **XOR**, y el título del desafío, **Zero Ex Six One**, podría indicar que la clave de cifrado es `0x61`

```txt
I'm XORta out of ideas for how to get the flag. Does this look like anything to you?
```

## Solución 1 - Usando la clave de cifrado

En esta primera solución, **convertimos la cadena hexadecimal en bytes** y aplicamos un **cifrado XOR** con la clave 🔑 `0x61`.

```python
# Cadena cifrada en hexadecimal proporcionada
hex_data = "0x070x0d0x000x060x1a0x020x540x510x050x590x530x020x510x000x530x540x070x520x040x570x550x550x050x510x560x510x530x030x550x500x050x030x050x510x590x540x000x1c"

# Eliminamos "0x" para poder hacer la conversión correctamente
clean_hex_data = hex_data.replace("0x", "")

# Convertimos la cadena hexadecimal a bytes
ciphertext = bytes.fromhex(clean_hex_data)

# Definimos la clave de cifrado
key = 0x61

# Aplicamos XOR a cada byte con la clave
decrypted = bytes([b ^ key for b in ciphertext])

# Mostramos el resultado en texto legible
print(decrypted.decode('utf-8', errors='ignore'))
```

```bash
$ python zero_ex_six_one.py 
flag{not_really_tho}
```

También se puede hacer uso de la herramienta [Cyberchef](https://gchq.github.io/CyberChef/)

[![/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_001.png](/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_001.png)](/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_001.png)

## Solución 2 - Sin conocer la clave de cifrado

En esta segunda solución, **no utilizamos una clave de cifrado predefinida**. En su lugar, deducimos la clave basándonos en el hecho de que las primeras letras del texto descifrado deben ser `flag` 🤔.

```python
# Cadena cifrada en hexadecimal proporcionada
hex_data = "0x070x0d0x000x060x1a0x020x540x510x050x590x530x020x510x000x530x540x070x520x040x570x550x550x050x510x560x510x530x030x550x500x050x030x050x510x590x540x000x1c"

# Eliminamos "0x" para poder hacer la conversión correctamente
clean_hex_data = hex_data.replace("0x", "")

# Convertimos la cadena hexadecimal a bytes
ciphertext = bytes.fromhex(clean_hex_data)

# Texto en claro conocido ("flag")
known_plaintext = b"flag"

# Calculamos la clave XOR comparando los primeros 4 bytes cifrados con "flag"
key = bytes([ciphertext[i] ^ known_plaintext[i] for i in range(4)])

# Aplicamos XOR a cada byte del texto cifrado utilizando la clave encontrada
decrypted = bytearray()
for i in range(len(ciphertext)):
    decrypted.append(ciphertext[i] ^ key[i % len(key)])

# Mostramos el resultado en texto legible
print(decrypted.decode('utf-8', errors='ignore'))
```

```bash
$ python zero_ex_six_one_no_key.py 
flag{not_really_tho}
```

Ambas soluciones permiten obtener la flag correctamente, ya sea conociendo la clave de cifrado de antemano o deduciéndola a partir de texto en claro conocido.