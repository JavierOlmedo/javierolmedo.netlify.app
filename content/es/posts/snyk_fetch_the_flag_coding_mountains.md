+++
date = "2025-03-08"
title = "Snyk Fetch The Flag - Coding Mountains"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_coding_mountains/snyk_fetch_the_flag_coding_mountains_banner.png](/images/snyk_fetch_the_flag_coding_mountains/snyk_fetch_the_flag_coding_mountains_banner.png)](/images/snyk_fetch_the_flag_coding_mountains/snyk_fetch_the_flag_coding_mountains_banner.png)

Los **retos de scripting** son mis favoritos en los CTF, y este, sin duda, ha sido mi preferido en Snyk Fetch The Flag 2025. El desafío consiste en **responder 50 preguntas rápidas sobre las montañas más famosas del mundo**. Debemos responder con su altura (en pies) y el año en el que fueron escaladas por primera vez, utilizando el siguiente formato: `height,year`.

Un ejemplo del inicio del juego sería:

```txt
$ nc challenge.ctf.games 32641

Welcome! I've recently gotten into mountaineering, so I came up with this fun little quiz game.  
Just answer 50 questions for me!

Note: Height is measured in feet with no comma (e.g., 28129). 
If the peak has not been summited, write none; otherwise, specify the year of the first ascent.
Answer with comma-separated values, (e.g., `height,year`)

Do you want to give it a chance? (Y/n): y
Awesome, good luck!
What is the height and first ascent year of Saraghrar: 
```

Para resolverlo, partimos de un archivo llamado `mountains.json`, que contiene las respuestas a las preguntas. El objetivo es claro: automatizar el proceso para responder las **50 preguntas** rápidamente.

📄 Fragmento del archivo `mountains.json`

```json
[
    ...
    {
        "name": "K2",
        "height": "28,251",
        "first": "1954"
    },
    {
        "name": "Kangchenjunga",
        "height": "28,169",
        "first": "1955"
    },
    ...
]
```

💣 Utilizando `coding_mountains_exploit.py`, podemos resolver el desafío de manera eficiente.

```python
import os
import sys
import socket
import json
import argparse

mountains = [
    {"name": "Mount Everest", "height": "29032", "first": "1953"},
    {"name": "K2", "height": "28251", "first": "1954"},
    {"name": "Kangchenjunga", "height": "28169", "first": "1955"},
    {"name": "Lhotse", "height": "27940", "first": "1956"},
    {"name": "Makalu", "height": "27838", "first": "1955"},
    {"name": "Cho Oyu", "height": "26864", "first": "1954"},
    {"name": "Dhaulagiri I", "height": "26795", "first": "1960"},
    {"name": "Manaslu", "height": "26781", "first": "1956"},
    {"name": "Nanga Parbat", "height": "26660", "first": "1953"},
    {"name": "Annapurna I", "height": "26545", "first": "1950"},
    {"name": "Gasherbrum I", "height": "26510", "first": "1958"},
    {"name": "Broad Peak", "height": "26414", "first": "1957"},
    {"name": "Gasherbrum II", "height": "26362", "first": "1956"},
    {"name": "Shishapangma", "height": "26335", "first": "1964"},
    {"name": "Gyachung Kang", "height": "26089", "first": "1964"},
    {"name": "Gasherbrum III", "height": "26070", "first": "1975"},
    {"name": "Annapurna II", "height": "26040", "first": "1960"},
    {"name": "Gasherbrum IV", "height": "26024", "first": "1958"},
    {"name": "Himalchuli", "height": "25896", "first": "1960"},
    {"name": "Distaghil Sar", "height": "25866", "first": "1960"},
    {"name": "Ngadi Chuli", "height": "25823", "first": "1979"},
    {"name": "Nuptse", "height": "25801", "first": "1961"},
    {"name": "Khunyang Chhish", "height": "25666", "first": "1971"},
    {"name": "Masherbrum", "height": "25659", "first": "1960"},
    {"name": "Nanda Devi", "height": "25643", "first": "1936"},
    {"name": "Chomo Lonzo", "height": "25604", "first": "1954"},
    {"name": "Batura Sar", "height": "25574", "first": "1976"},
    {"name": "Rakaposhi", "height": "25551", "first": "1958"},
    {"name": "Namcha Barwa", "height": "25531", "first": "1992"},
    {"name": "Kanjut Sar", "height": "25460", "first": "1959"},
    {"name": "Kamet", "height": "25446", "first": "1931"},
    {"name": "Dhaulagiri II", "height": "25430", "first": "1971"},
    {"name": "Saltoro Kangri", "height": "25400", "first": "1962"},
    {"name": "Kumbhakarna", "height": "25299", "first": "1962"},
    {"name": "Tirich Mir", "height": "25289", "first": "1950"},
    {"name": "Molamenqing", "height": "25272", "first": "1981"},
    {"name": "Gurla Mandhata", "height": "25243", "first": "1985"},
    {"name": "Saser Kangri I", "height": "25171", "first": "1973"},
    {"name": "Chogolisa", "height": "25148", "first": "1975"},
    {"name": "Dhaulagiri IV", "height": "25135", "first": "1975"},
    {"name": "Kongur Tagh", "height": "25095", "first": "1981"},
    {"name": "Dhaulagiri V", "height": "24993", "first": "1975"},
    {"name": "Shispare", "height": "24970", "first": "1974"},
    {"name": "Trivor", "height": "24859", "first": "1960"},
    {"name": "Gangkhar Puensum", "height": "24840", "first": "none"},
    {"name": "Gongga Shan", "height": "24790", "first": "1932"},
    {"name": "Annapurna III", "height": "24787", "first": "1961"},
    {"name": "Skyang Kangri", "height": "24754", "first": "1976"},
    {"name": "Changtse", "height": "24747", "first": "1982"},
    {"name": "Kula Kangri", "height": "24731", "first": "1986"},
    {"name": "Kongur Tiube", "height": "24700", "first": "1956"},
    {"name": "Annapurna IV", "height": "24688", "first": "1955"},
    {"name": "Mamostong Kangri", "height": "24659", "first": "1984"},
    {"name": "Saser Kangri II E", "height": "24649", "first": "2011"},
    {"name": "Muztagh Ata", "height": "24636", "first": "1956"},
    {"name": "Ismoil Somoni Peak", "height": "24590", "first": "1933"},
    {"name": "Saser Kangri III", "height": "24590", "first": "1986"},
    {"name": "Noshaq", "height": "24580", "first": "1960"},
    {"name": "Pumari Chhish", "height": "24580", "first": "1979"},
    {"name": "Passu Sar", "height": "24528", "first": "1994"},
    {"name": "Yukshin Gardan Sar", "height": "24505", "first": "1984"},
    {"name": "Teram Kangri I", "height": "24482", "first": "1975"},
    {"name": "Jongsong Peak", "height": "24482", "first": "1930"},
    {"name": "Malubiting", "height": "24469", "first": "1971"},
    {"name": "Gangapurna", "height": "24459", "first": "1965"},
    {"name": "Jengish Chokusu", "height": "24406", "first": "1956"},
    {"name": "Sunanda Devi", "height": "24390", "first": "1939"},
    {"name": "K12", "height": "24370", "first": "1974"},
    {"name": "Yangra", "height": "24350", "first": "1955"},
    {"name": "Sia Kangri", "height": "24350", "first": "1934"},
    {"name": "Momhil Sar", "height": "24324", "first": "1964"},
    {"name": "Kabru N", "height": "24318", "first": "1994"},
    {"name": "Skil Brum", "height": "24310", "first": "1957"},
    {"name": "Haramosh Peak", "height": "24308", "first": "1958"},
    {"name": "Istor-o-Nal", "height": "24288", "first": "1969"},
    {"name": "Ghent Kangri", "height": "24281", "first": "1961"},
    {"name": "Ultar", "height": "24239", "first": "1996"},
    {"name": "Rimo I", "height": "24229", "first": "1988"},
    {"name": "Churen Himal", "height": "24229", "first": "1970"},
    {"name": "Teram Kangri III", "height": "24219", "first": "1979"},
    {"name": "Sherpi Kangri", "height": "24210", "first": "1976"},
    {"name": "Labuche Kang", "height": "24170", "first": "1987"},
    {"name": "Kirat Chuli", "height": "24154", "first": "1939"},
    {"name": "Abi Gamin", "height": "24131", "first": "1950"},
    {"name": "Gimmigela Chuli", "height": "24110", "first": "1994"},
    {"name": "Nangpai Gosum", "height": "24110", "first": "1986"},
    {"name": "Saraghrar", "height": "24111", "first": "1959"},
    {"name": "Talung", "height": "24111", "first": "1964"},
    {"name": "Jomolhari", "height": "24035", "first": "1937"},
    {"name": "Chamlang", "height": "24019", "first": "1961"},
    {"name": "Chongtar", "height": "23999", "first": "1994"},
    {"name": "Baltoro Kangri", "height": "23990", "first": "1963"},
    {"name": "Siguang Ri", "height": "23980", "first": "1989"},
    {"name": "The Crown", "height": "23934", "first": "1993"},
    {"name": "Gyala Peri", "height": "23930", "first": "1986"},
    {"name": "Porong Ri", "height": "23924", "first": "1982"},
    {"name": "Baintha Brakk", "height": "23901", "first": "1977"},
    {"name": "Yutmaru Sar", "height": "23894", "first": "1980"},
    {"name": "K6", "height": "23891", "first": "1970"},
    {"name": "KangpenqingGang Benchhen", "height": "23888", "first": "1982"},
    {"name": "Muztagh Tower", "height": "23871", "first": "1956"},
    {"name": "Mana Peak", "height": "23858", "first": "1937"},
    {"name": "Dhaulagiri VI", "height": "23845", "first": "1970"},
    {"name": "Diran", "height": "23839", "first": "1968"},
    {"name": "Labuche Kang III", "height": "23790", "first": "none"},
    {"name": "Putha Hiunchuli", "height": "23773", "first": "1954"},
    {"name": "Apsarasas Kangri", "height": "23770", "first": "1976"},
    {"name": "Mukut Parbat", "height": "23760", "first": "1951"},
    {"name": "Rimo III", "height": "23730", "first": "1985"},
    {"name": "Langtang Lirung", "height": "23711", "first": "1978"},
    {"name": "Karjiang", "height": "23691", "first": "none"},
    {"name": "Annapurna Dakshin", "height": "23684", "first": "1964"},
    {"name": "Khartaphu", "height": "23665", "first": "1935"},
    {"name": "Tongshanjiabu", "height": "23645", "first": "none"},
    {"name": "Malangutti Sar", "height": "23645", "first": "1985"},
    {"name": "Noijin Kangsang", "height": "23642", "first": "1986"},
    {"name": "Langtang Ri", "height": "23638", "first": "1981"},
    {"name": "Kangphu Kang", "height": "23635", "first": "2002"},
    {"name": "Singhi Kangri", "height": "23629", "first": "1976"},
    {"name": "Lupghar Sar", "height": "23600", "first": "1979"}
]

def banner():
    os.system("cls||clear")
    print('''
      _________              __     _______________________________
    /   _____/ ____ ___.__.|  | __ \_   ___ \__    ___/\_   _____/
    \_____  \ /    <   |  ||  |/ / /    \  \/ |    |    |    __)  
    /        \   |  \___  ||    <  \     \____|    |    |     \   
    _______  /___|  / ____||__|_ \  \______  /|____|    \___  /   
           \/     \/\/          \/         \/               \/ 2025
                      [CODING MOUNTAINS EXPLOIT]
                            @jjavierolmedo
    ''')

def get_args():
    parser = argparse.ArgumentParser(description='Snyk Fetch the Flag - Coding Mountains Exploit')
    parser.add_argument('-u', '--url', dest='url', required=True, action='store', type=str, help='Insert challenge URL')
    parser.add_argument('-p', '--port', dest="port", required=True, action='store', type=int, help='Insert challenge port')
    args = parser.parse_args()
    return args

def main():
    banner()
    args = get_args()
    mountain_dict = {mountain['name']: (mountain['height'], mountain['first']) for mountain in mountains}

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((args.url, args.port))

        # Read welcome message
        welcome_message = s.recv(4096).decode()

        # Send Y to accept the challenge
        s.sendall(b'Y\n')
        question = s.recv(4096).decode()

        while True:
            question = s.recv(4096).decode()

            if 'flag' in question:
                print(question)
                break

            if "What is the height and first ascent year of" in question:

                # Extract the name of the mountain from the question
                mountain_name = question.split("What is the height and first ascent year of ")[1].strip().replace(':', '')

                # Look up mountain in the dictionary
                if mountain_name in mountain_dict:
                    height, first_ascent = mountain_dict[mountain_name]
                    answer = f"{height},{first_ascent}\n"
                    s.sendall(answer.encode())
                else:
                    print(f"Mountain {mountain_name} not found in the JSON data.")
                    break
            elif "" == question:
                print("Quiz ended.")
                break

if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("[-] User aborted session\n")
        sys.exit(0)
```

Ejecutamos el script de la siguiente manera

```bash
python coding_mountains_exploit.py -u challenge.ctf.games -p 32641
```

Resultado

```txt
      _________              __     _______________________________
    /   _____/ ____ ___.__.|  | __ \_   ___ \__    ___/\_   _____/
    \_____  \ /    <   |  ||  |/ / /    \  \/ |    |    |    __)  
    /        \   |  \___  ||    <  \     \____|    |    |     \   
    _______  /___|  / ____||__|_ \  \______  /|____|    \___  /   
           \/     \/\/          \/         \/               \/ 2025
                      [CODING MOUNTAINS EXPLOIT]
                            @jjavierolmedo
    
Good job! You're like the Alex Honold of coding. Here is the flag: flag{33e043f76c3ba0fe9265749dbe650940}
```

## 🚩 Flag

```txt
flag{33e043f76c3ba0fe9265749dbe650940}
```