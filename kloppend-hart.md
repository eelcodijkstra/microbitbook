# Kloppend hart

In deze les maak je een eenvoudige animatie van een kloppend hart op het microbit-display.
Je bouwt dit op in een aantal stappen, te beginnen met het programma om alleen een hart te tonen.

:::{Admonition} Concepten

import, display, image, pixels, herhaling

:::

## Toon een enkele figuur

::::{grid}
:gutter: 2

:::{grid-item}
```{figure} figs/image-heart.png
:width: 200
```
:::
:::{grid-item}

:::
:::{grid-item}

:::
:::{grid-item}

:::

::::

In de eerst les heb je een programma op de microbit geladen om een hart op het display te tonen.
De Python-tekst van dit programma:

```python
from microbit import *

display.show(Image.HEART)
```

Wat betekent dit?

:::{margin}
Over de begrippen *module*, *class* en *object* hoef je je nu nog niet druk te maken. Daar komen we later uitgebreider op terug. Het is voldoende als je deze voorbeelden in grote lijnen begrijpt.
:::

- voordat je de opdrachten van de microbit-module kunt gebruiken, moet je deze eerst *importeren*. De microbit-module is een grote verzameling definities voor het gebruik van de microbit. Zie [microbit](https://microbit-micropython.readthedocs.io/en/v2-docs/microbit.html). Deze module bevat onder andere de `display`-module voor het aansturen van het display.
- de `show`-functie van de `display`-module: `display.show(fig)` toont een figuur op het display. Zie [microbit.display.show](https://microbit-micropython.readthedocs.io/en/v2-docs/display.html#microbit.display.show)
- als figuur gebruiken we het `HEART`-object (een hart) van de `Image` class: zie [images](https://microbit-micropython.readthedocs.io/en/v2-docs/image.html#attributes)


**Stappen**

- zie de stappen bij het laden van een programma op de microbit

**Variaties**

Als het programma werkt kun je dit op allerlei manieren aanpassen, en de aangepaste code op je microbit laden.
Zorg ervoor dat je je programma na elke aanpassing eerst bewaart ("Save") op je computer en daarna naar de microbit stuurt ("Flash").

- kies een andere figuur uit de lijst van ingebouwde figuren ([images](https://microbit-micropython.readthedocs.io/en/v2-docs/image.html#attributes))
- maak zelf een figuur, zoals bijvoorbeeld:

```python
from microbit import *

display.show(Image(
              "90009:"
              "09090:"
              "00900:"
              "09090:"
              "90009"))
```

Het display bestaat uit 5 bij 5 ledjes, de "pixels" van het display. 
In het bovenstaande image geeft elk cijfer de helderheid van het overeenkomstige pixel aan: `0` is uit, en `9` is maximale helderheid. (*Vraag:* wat stelt het bovenstaande `Image` voor?)

## Een-staps animatie

::::{grid}
:gutter: 2

:::{grid-item}
```{figure} figs/image-heart.png
:width: 200
```
:::
:::{grid-item}
```{figure} figs/image-heart-small.png
:width: 200
```
:::
:::{grid-item}

:::
:::{grid-item}

:::

::::

Een animatie bestaan uit een reeks opeenvolgende beelden.
Je kunt een *kloppend hart* maken door het "grote" hart af te wisselen met een kleiner hart.
Probeer als eerste stap het volgende programma om twee figuren na elkaar te tonen:

```python
from microbit import *

display.show(Image.HEART)
display.show(Image.HEART_SMALL)
```

**Uitleg**: je ziet hier twee opdrachten na elkaar: deze opdrachten worden in deze volgorde na elkaar uitgevoerd.

**Vraag**: Wat zie je als je dit programma op de microbit uitvoert? Hoe komt dat? En hoe zou je dat kunnen oplossen?

:::{dropdown} Antwoord
Je ziet alleen de laatste figuur, het kleine hart.
Computers, ook zulke eenvoudige als een microbit, zijn (heel) veel sneller dan mensen: je ziet de eerste figuur niet, deze wordt direct overschreven door de tweede figuur.
:::

Je kunt de microbit tussen twee opdrachten laten wachten met de functie `utime.sleep(x)`. Hierin is `x` het aantal seconden; dit mag ook een decimale fractie zijn, bijvoorbeeld `0.1`.

```python
from microbit import *
import utime

display.show(Image.HEART)
utime.sleep(1)
display.show(Image.HEART_SMALL)
```

**Uitleg**

- we gebruiken (importeren) de module `utime` (zie: [utime](https://microbit-micropython.readthedocs.io/en/v2-docs/utime.html))
- `utime.sleep(1)` zorgt ervoor dat de microbit 1 seconde wacht (”slaapt”; er worden dan geen opdrachten uitgevoerd) en daarna doorgaat.

**Variaties**

- probeer andere wachttijden, ook kleiner dan een seconde
- breid het programma uit zodat dit na het kleine hartje weer het grotere hart toont

:::{dropdown} Uitwerking
```python
from microbit import *
import utime

display.show(Image.HEART)
utime.sleep(0.5)
display.show(Image.HEART_SMALL)
utime.sleep(0.5)
display.show(Image.HEART)
```
:::

## Herhaal de animatie-stappen

::::{grid}
:gutter: 2

:::{grid-item}
```{figure} figs/image-heart.png
:width: 200
```
:::
:::{grid-item}
```{figure} figs/image-heart-small.png
:width: 200
```
:::
:::{grid-item}
```{figure} figs/image-heart.png
:width: 200
```
:::
:::{grid-item}
```{figure} figs/image-heart-small.png
:width: 200
```
:::

::::

In de voorafgaande opdracht heb je gezien dat je met opeenvolgende `show`-opdrachten opeenvolgende figuren op het display kunt laten zien. Je gebruikt dan de `sleep`-functie om de figuur lang genoeg op het display te houden voordat deze overschreven wordt.

In deze opdracht maak je een “oneindige” herhaling van afwisselende figuren. Als je dit in het juiste tempo doet, geeft dit het effect van een animatie.

Code voor *blinking heart*:

```python
from microbit import *
import utime

while True:
    display.show(Image.HEART)
    utime.sleep(0.5)
    display.show(Image.HEART_SMALL)
    utime.sleep(0.5)
```

Uitleg bij dit programma:

- de Python-opdracht `while True: actie` herhaalt `actie` zolang de voorwaarde `True` geldt, met andere woorden: blijft steeds `actie` herhalen (totdat de *power* van de microbit uitvalt).
- `actie` kan uit meerdere opdrachten bestaan: deze schrijf je dan *onder* de `while True:`. Om aan te geven dat deze opdrachten bij de `while True:` horen, laat je deze 4 spaties inspringen. **Deze witruimte heeft betekenis in Python!**

**Vraag**: waarom is de tweede `sleep` nodig? 

:::{dropdown} Antwoord
De tweede `sleep` komt voor het "grote" hart in de volgende herhaling van de lus. Als je deze `sleep` weglaat is er geen tijd tussen het kleine hart en het daarop volgende grote hart, en zie je dus alleen maar het grote hart.
:::

**Variatie(s)**:

- het is niet nodig dat de wachttijden voor beide figuren gelijk zijn. Probeer verschillende tijden voor de `sleep`-aanroepen, om (i) het tempo van de hartslag te verhogen of te verlagen: het gaat dan om de som van de beide `sleep`-argumenten; of om (ii) een meer "dynamische" hartslag te krijgen, met bijvoorbeeld een kortere tijd voor het kleine hart: het gaat dan om de verhouding tussen de beide `sleep`-argumenten.
- probeer een animatie met andere figuren. Zie bijvoorbeeld: [images-animation](https://microbit-micropython.readthedocs.io/en/v2-docs/tutorials/images.html#animation)


**Vraag**: wat zie je op het microbit display bij het volgende programma?

```python
from microbit import *
import utime

while True:
    display.show(Image.HEART)
utime.sleep(0.5)
display.show(Image.HEART_SMALL)
utime.sleep(0.5)
```

:::{dropdown} Antwoord
Je ziet alleen het grote hart: de opdrachten die niet ingesprongen zijn horen niet bij het `while`-statement, en worden na dit statement uitgevoerd. Maar: het `while True: ...` - statement blijft herhalen, en eindigt nooit: de opdrachten daarna worden dus nooit uitgevoerd.
:::


## Opmerkingen

Door het gebruik van herhaling kun je in een kort programma een heel lang proces beschrijven.

De herhaling met `True` als voorwaarde is een speciaal geval. Deze kom je eigenlijk alleen tegen in besturingsprogramma’s zoals voor de microbit. In “normale” programma’s gebruik je een voorwaarde die uiteindelijk (na 0 of meer herhalingen) False oplevert.


---

Vraag: wat kun je zeggen over de opdrachten die je niet-ingesprongen na de `while True:` schrijft, als in het onderstaande programma:

```python
from microbit import *
import utime

while True:
    display.show(Image.HEART)
    utime.sleep(0.5)
    display.show(Image.HEART_SMALL)
    utime.sleep(0.5)

display.show(Image.HAPPY)
```

