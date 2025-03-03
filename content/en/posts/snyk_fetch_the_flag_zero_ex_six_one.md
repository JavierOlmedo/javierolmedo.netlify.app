+++
date = "2025-03-03"
title = "Snyk Fetch The Flag - Zero Ex Six One"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_banner.png](/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_banner.png)](/images/snyk_fetch_the_flag_zero_ex_six_one/snyk_fetch_the_flag_zero_ex_six_one_banner.png)

In this challenge, we obtain a file named `flag.txt.encry`, which contains a hexadecimal string.

```bash
$ cat flag.txt.encry
0x070x0d0x000x060x1a0x020x540x510x050x590x530x020x510x000x530x540x070x520x040x570x550x550x050x510x560x510x530x030x550x500x050x030x050x510x590x540x000x1c
```

The 💡 hint in the challenge description suggests that an **XOR encryption** was used, and the challenge title, **Zero Ex Six One**, might indicate that the encryption key is `0x61`.

```txt
I'm XORta out of ideas for how to get the flag. Does this look like anything to you?
```

## Solution 1 - Using the Encryption Key

In this first solution, **we convert the hexadecimal string into bytes** and apply an **XOR decryption** using the key `0x61`.

```python
# Encrypted hexadecimal string provided
hex_data = "0x070x0d0x000x060x1a0x020x540x510x050x590x530x020x510x000x530x540x070x520x040x570x550x550x050x510x560x510x530x030x550x500x050x030x050x510x590x540x000x1c"

# Remove "0x" for proper conversion
clean_hex_data = hex_data.replace("0x", "")

# Convert the hexadecimal string to bytes
ciphertext = bytes.fromhex(clean_hex_data)

# Define the encryption key
key = 0x61

# Apply XOR to each byte using the key
decrypted = bytes([b ^ key for b in ciphertext])

# Display the result in readable text
print(decrypted.decode('utf-8', errors='ignore'))
```

```
$ python zero_ex_six_one.py 
flag{c50d82c0a25f3e644d0702b41dbd085a}
```

## Solution 2 - Without Knowing the Encryption Key

In this solution, **we do not use a predefined encryption key**. Instead, we deduce the key based on the assumption that the first letters of the decrypted text should be `flag`.

```python
# Encrypted hexadecimal string provided
hex_data = "0x070x0d0x000x060x1a0x020x540x510x050x590x530x020x510x000x530x540x070x520x040x570x550x550x050x510x560x510x530x030x550x500x050x030x050x510x590x540x000x1c"

# Remove "0x" for proper conversion
clean_hex_data = hex_data.replace("0x", "")

# Convert the hexadecimal string to bytes
ciphertext = bytes.fromhex(clean_hex_data)

# Known plaintext text ("flag")
known_plaintext = b"flag"

# Calculate the XOR key by comparing the first 4 encrypted bytes with "flag"
key = bytes([ciphertext[i] ^ known_plaintext[i] for i in range(4)])

# Apply XOR to each byte using the discovered key
decrypted = bytearray()
for i in range(len(ciphertext)):
    decrypted.append(ciphertext[i] ^ key[i % len(key)])

# Display the result in readable text
print(decrypted.decode('utf-8', errors='ignore'))
```

```bash
$ python zero_ex_six_one_no_key.py 
flag{c50d82c0a25f3e644d0702b41dbd085a}
```

Both solutions successfully retrieve the flag, either by knowing the encryption key beforehand or by deducing it from known plaintext patterns.