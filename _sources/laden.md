# Programma laden op de microbit

:::{admonition} Wat heb je nodig?

- een computer met daarop de Mu Python editor geïnstalleerd (”host” computer).
- een microbit
- een USB micro kabel, voor het verbinden van de microbit met de “host” computer

:::

:::{figure} figs/host-usb-microbit-exc.png
:width: 600
:::

Op de "host" computer sla je je Python-bestanden op.
Op de microbit staat -na het laden- je programma en de microPython interpreter voor het uitvoeren van je programma.

:::{figure} figs/mu-knoppenbalk.png
:width: 800
Knoppenbalk Mu editor in microbit-mode
:::

Stappen:

:::{figure} figs/mu-microbit-verbonden.png
:width: 200
:figclass: margin
microbit verbonden
:::

:::{figure} figs/mu-microbit-niet-verbonden.png
:width: 200
:figclass: margin
microbit niet verbonden
:::

- verbind de microbit met de computer (via de USB-kabel)
- start de Mu editor
- zorg dat Mu in “microbit” mode staat. De huidige mode zie je rechts onderin in het Mu venster. Je kunt de mode instellen via de “Mode” knop links boven.
    - rechtsonder in het Mu-venster zie je of de microbit verbonden is met de Mu-editor.
    - als het goed is, vindt Mu de microbit automatisch; als dat niet het geval is, haal dan de USB-kabel even uit de computer, en verbindt deze opnieuw.
- open een nieuw tabblad in het programma-venster in Mu (”New” knop)
- kopieer je programma naar dit venster (Copy/Paste). Gebruik bijvoorbeeld:
    
    ```python
    from microbit import *
    
    display.show(Image.HEART)
    ```
    
- bewaar het programma in dit venster als bestand op je *host* computer (”Save” knop). Hierbij geef je op in welke map op je computer het bestand bewaard moet worden, en onder welke naam. Zorg dat de naam eindigt op `.py`, bijvoorbeeld `myheart.py`.
- laad het programma naar de microbit (”Flash” knop). Tijdens het laden gaat het oranje ledje van de microbit snel knipperen. Het laden duurt enkele seconden.

De microbit start het programma direct na het laden; en als je daarna de microbit herstart.

