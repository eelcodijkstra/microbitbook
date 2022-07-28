# Hello World!

- eerste programma om te laten zien dat alles werkt - van het invoeren van het programma in de editor tot het uitvoeren van het programma op de microbit.
- voor embedded systems, de gebruikelijke versie is “blink”: het laten knipperen van een LED. In het geval van de microbit zou dat het knipperen van het display zijn (blinking heart is een goede).
- voor microbit: tonen  van een figuur, bijv. een hart (of één van de andere microbit-figuren); en: tonen van tekst
- welke editor? in eerste instantie Mu; ook Thonny proberen.
- Zie ook: MakeCode versie, [https://microbit.org/projects/make-it-code-it/heart/](https://microbit.org/projects/make-it-code-it/heart/)
- Zie ook: Setup guide, [https://microbit.org/get-started/first-steps/set-up/](https://microbit.org/get-started/first-steps/set-up/)
    - hiervan moeten we een Python-versie maken
- Todo: figuur maken van Host en microbit. (gemaakt in excalidraw)

<aside>
⚠️ Wat heb je nodig?
* een computer (”host”) met een microPython editor (Mu, of Thonny)
* een microbit (”target”)
* een USB-kabel voor het verbinden van de host met de target

</aside>

Als eerste programma om uit te proberen of alles werkt - van de editor op de host via het “flashen” van de code tot het uitvoeren van het programma op de microbit -  gebruiken we het weergeven van een figuur op het microbit-display, bijvoorbeeld een hartje.

De Python-tekst voor dit programma is:

```python
from microbit import *

display.show(Image.HEART)
```

Uitleg hierbij:

- we gebruiken (”importeren”) de microbit-module, een grote verzameling definities voor het gebruik van de hardware. Zie voor een beschrijving van deze module: [https://microbit-micropython.readthedocs.io/en/v2-docs/microbit.html](https://microbit-micropython.readthedocs.io/en/v2-docs/microbit.html)
- van deze module gebruiken we de `display`-module, met de functie `show`: `display.show(fig)` toont een figuur op het display. Zie [https://microbit-micropython.readthedocs.io/en/v2-docs/display.html#microbit.display.show](https://microbit-micropython.readthedocs.io/en/v2-docs/display.html#microbit.display.show)
- als figuur gebruiken we het HEART-object (een hartje) van de `Image` class: zie [https://microbit-micropython.readthedocs.io/en/v2-docs/image.html#attributes](https://microbit-micropython.readthedocs.io/en/v2-docs/image.html#attributes)

(Over de begrippen `module`, `class` en `object` hoef je je nu nog niet druk te maken. Daar komen we later uitgebreider op terug. Het is voldoende als je deze voorbeelden begrijpt.)

Stappen

- zie de stappen bij het laden van een programma op de microbit

Variaties

Als het programma werkt, kun je dit op allerlei manieren aanpassen, en de veranderde code op je microbit laden.

- kies een andere figuur uit de lijst van ingebouwde figuren ([https://microbit-micropython.readthedocs.io/en/v2-docs/image.html#attributes](https://microbit-micropython.readthedocs.io/en/v2-docs/image.html#attributes))
- maak zelf een figuur, zie bijvoorbeeld:

```python
from microbit import *

display.show(Image(
              "90009:"
              "09090:"
              "00900:"
              "09090:"
              "90009"))
```

De `9` en de `0` geven de helderheid van de bijbehorende pixel aan.

Twee figuren na elkaar

We willen twee figuren na elkaar laten zien. Probeer eerst het volgende programma:

Je ziet hier twee opdrachten na elkaar: deze opdrachten worden in deze volgorde uitgevoerd.

```python
from microbit import *

display.show(Image.HEART)
display.show(Image.HEART_SMALL)
```

Wat zie je als je dit programma op de microbit uitvoert? Hoe komt dat? En hoe zou je dat kunnen oplossen?

Computers, ook zulke eenvoudige als een microbit, zijn (heel) veel sneller dan mensen: je ziet de eerste figuur niet, deze wordt direct overschreven door de tweede figuur.

Een oplossing hiervoor is om de microbit tussen de twee figuren te laten wachten, met de functie `utime.sleep(x)`. Hierin is `x` het aantal seconden; dit mag ook een decimale fractie zijn, bijvoorbeeld `0.1`.

```python
from microbit import *
import utime

display.show(Image.HEART)
utime.sleep(1)
display.show(Image.HEART_SMALL)
```

Uitleg bij dit programma:

- we gebruiken (importeren) de module `utime` (zie: [https://microbit-micropython.readthedocs.io/en/v2-docs/utime.html](https://microbit-micropython.readthedocs.io/en/v2-docs/utime.html))
- `utime.sleep(1)` zorgt ervoor dat de microbit 1 seconde wacht (”slaapt”; er worden dan geen opdrachten uitgevoerd) en daarna doorgaat.

Variaties

- probeer andere wachttijden, ook kleiner dan een seconde
- breid het programma uit zodat dit na het kleine hartje weer het grotere hart toont

Tekst op het display

In plaats van een figuur kun je ook tekst (letters, cijfers en leestekens) op het display weergeven.  Omdat er maar een enkel teken op het display past moet je de tekst “scrollen”, waarbij deze van rechts naar links over het display schuift. De functie hiervoor is `display.scroll`. 

```python
from microbit import *
import utime

display.scroll('Hello World!')
```

Uitleg bij dit programma:

- Voor de documentatie van display.scroll, zie: [https://microbit-micropython.readthedocs.io/en/v2-docs/display.html#microbit.display.scroll](https://microbit-micropython.readthedocs.io/en/v2-docs/display.html#microbit.display.scroll)
- `'Hello World!'` is een *string*-waarde in Python. Een string is een reeks tekens: letters, cijfers, of leestekens.

Variaties

- pas de tekst aan, bijvoorbeeld naar `‘Hallo Kees!’` , als je Kees heet.
- splits de tekst in twee stukken, en stuur deze met twee opeenvolgende opdrachten naar het scherm. Waarom heb je nu geen `sleep()` nodig?

Tekst sturen naar de host

Je kunt vanuit de microbit ook tekst-uitvoer sturen naar de hostcomputer. Dit vind je dan in het Mu REPL-venster. 

We gebruiken hiervoor het volgende programma:

```python
from microbit import *
import utime

display.scroll('Hello World!')
print('Hello World!!!')
```

Stappen

- voer dit programma in in MU, en laad het in de microbit; als het goed is, zie je de tekst op het microbit-display.
- klik in Mu op de REPL-knop. Hiermee onderbreek je het programma op de microbit. Je kunt in het REPL-venster onderin nu Python-opdrachten uitvoeren, maar dat bewaren we voor een andere les.
- herstart de microbit met de knop achterop. Je ziet nu op het display weer de tekst “Hello World!” verschijnen
- en daarna verschijnt in het REPL-venster de tekst “Hello World!!!”

Variatie:

- splits de tekst in twee stukken, en gebruik opeenvolgende `print`-opdrachten om deze naar de host computer te sturen. Wat zie je nu in het REPL-venster?
-