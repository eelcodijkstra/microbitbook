# Teller

::::{grid}

:::{grid-item-card} Voorkennis
:class-header: bg-light
header
^^^

* `display.show`
* button-events

+++
footer
:::

:::{grid-item-card} Concepten
:class-body: bg-light

* variabele
* toekenning

:::

::::

:::{figure} figs/handteller.png
:align: right
:width: 120

Een handteller
:::

Je maakt een *teller* met de microbit: als je knop A indrukt, wordt de teller met 1 opgehoogd. Als je knop B indrukt, met 1 afgelaagd. Het display geeft steeds het actuele aantal weer.

Zulke handtellers ("clickers") worden bijvoorbeeld gebruikt voor het tellen van de passagiers in een bus; van deelnemers bij een event; van ronden bij wedstrijden; van waarnemingen bij biologie; enz.


**Het programma**

```Python
from microbit import *

totaal = 0
while True:
    if button_a.was_pressed():
        totaal = totaal + 1
    if button_b.was_pressed():
        totaal = totaal - 1
    if was_gesture('face down'):
        display.show(0)
        display.show(totaal)

```

**Voer het programma uit**

**Uitleg**

* `totaal` is een *variable*, met als waarde een geheel getal (`int`).
* `totaal` krijgt 0 als *initiële waarde*
* bij indrukken van A wordt de nieuwe waarde van `totaal`, de vorige waarde + 1.
* bij indrukken van B wordt de nieuwe waarde van `totaal`, de vorige waarde - 1.

Zie voor meer uitleg over variabelen en toekenning: {ref}`variabelen`.

**Variaties**

* soms hoef je alleen maar omhoog te tellen: dan is het handig als je ook met twee omhoog kunt tellen. Verander de actie voor button B zodat deze de teller met 2 ophoogt.
* voeg een event toe, bijvoorbeeld het schudden van de microbit, voor het terugzetten van het totaal (`totaal = 0`).

**Een probleem...** Als het totaal boven de 9 komt, kun je dat niet meer goed op het display weergeven. Je ziet de cijfers nog wel voorbijkomen, maar je ziet dan bijvoorbeeld geen verschil tussen 1 en 11, of tussen 12 en 21. Daar komt nog een ander probleem bij: het weergeven van meer dan 1 cijfer kost tijd: dat betekent dat je in de tijd die het kost om `totaal` weer te geven op het display, geen events kunt verwerken van het indrukken van de knoppen.

We kunnen dit probleem op verschillende manieren oplossen. We kunnen een (groot) getal op het display op andere manieren weergeven. We gebruiken twee voorbeelden:

* het weergeven van een getal in "unaire representatie": de som van het aantal pixels dat "aan" is geeft het getal weer;
* het weergeven van een getal in "binaire representatie": we gebruiken de pixels voor de binaire representatie van het getal.

(Variatie: gebruik een speciaal gebaar (schudden?) om het getal decimaal weer te geven. Dat kun je dan op een "rustig" moment doen, als er niet meer geteld hoeft te worden. Je kunt het probleem natuurlijk ook oplossen door het getal helemaal niet meer te tonen tijdens het tellen, maar alleen op verzoek.)


## Unaire representatie

```Python
from microbit import *
import utime

pixels = bytearray(25)

totaal = 0
while True:
    if button_a.was_pressed():
        pixels[totaal] = 9
        totaal = totaal + 1
    if button_b.was_pressed():
        totaal = totaal - 1
        pixels[totaal] = 0
    display.show(Image(5, 5, pixels))
```

* probleem: geeft index error boven 25

## Binaire representatie

```Python
from microbit import *

pixels = bytearray(25)

def binary_display(num):
    pos = 24
    while pos >= 0:
        if num % 2 == 1:
            pixels[pos] = 9
        else:
            pixels[pos] = 0
        num = num // 2
        pos = pos - 1 

totaal = 0
while True:
    if button_a.was_pressed():
        totaal = totaal + 1
    if button_b.was_pressed():
        totaal = totaal - 1
    binary_display(totaal)    
    display.show(Image(5, 5, pixels))
```

(Nog betere namen kiezen. We kunnen dit "binary display" ook gebruiken voor de rekenmachine, straks.)

## Vragen

* wat gebeurt er met negatieve getallen (kan door aftellen via B)?
* wat gebeurt er bij overflow (alleen voor unaire vorm)
* waarom hoef je bij de binaire vorm niet bang te zijn voor overflow?
* kun je in de binaire vorm ook negatieve getallen weergeven?

