# Dimmer

In deze opdracht gebruik je de knoppen van de microbit om het display als "lampje" aan- en uit te schakelen. Een volgende stap is om dit display in verschillende lichtsterktes te schakelen, als bij een dimmer.
Je kunt in plaats van het display ook een externe LED als "lampje" gebruiken.


::::{grid}

:::{grid-item-card}
:class-header: bg-light
**Voorkennis**
^^^

* buttons en events
* variabelen en toekenning
* if-statement

:::

:::{grid-item-card} Concepten
:class-header: bg-light
header
^^^

* eindige automaat,
* toestand, toestandsovergang

:::

::::

**Programma**

```Python
from microbit import *

# set brightness level for "lamp"
def lamp (level):
    pixels = bytearray(level for i in range(25))
    display.show(Image(5, 5, pixels))

state = 0
lamp(0)
while True:
    if button_a.was_pressed():
        if state == 0:
            lamp(9)            
            state = 1
        elif state == 1:
            lamp(0)
            state = 0            
```    

**Wat denk je dat dit programma doet?**

**Uitleg**

De functie `lamp(level)` zet de helderheid van de lamp op `level`, een waarde van 0 (uit) tot en met 9 (grootste helderheid). Als "lamp" gebruiken we voorlopig het display van de microbit, maar later zullen we daar andere oplossingen voor gebruiken.

Button A fungeert hier als een schakelaar: als de toestand 0 is, schakelt deze naar toestand 1, en zet daarbij de lamp "vol" aan; als de toestand 1 is, schakelt deze naar toestand 0, en zet daarbij de lamp uit.

In de volgende figuur zie je dit gedrag getekend:

:::{figure} figs/fsm-schakelaar-0.png
:width: 300
Schakelaar als eindige automaat
:::

Dit is een voorbeeld van een *eindige automaat*. Zo'n automaat bestaat uit een eindig aantal toestanden (de "bolletjes" hierboven), met toestandsovergangen tussen deze toestanden (de "pijlen"). Elke overgang is voorzien van een invoersymbool - hier de button-event; en van een uitvoersymbool - hier de actie om de lamp aan of uit te zetten.

**Variaties**

* pas het programma zo aan dat je de lamp uitzet met knop B.
* 

## Lamp met eerdere niveaus: "dimmer"

Sommige lampen kun je met dezelfde schakelaar op verschillende sterktes instellen; na de maximale sterkte schakelt de lamp dan weer uit. Een voorbeeld van dit gedrag zie je in de volgende eindige automaat:

:::{figure} figs/fsm-schakelaar-1.png
:width: 500
Schakelaar met meerdere niveaus
:::

Dit kunnen we als volgt verwerken in een Python programma:

```Python
from microbit import *

# set brightness level for "lamp"
def lamp(level):
    pixels = bytearray(level for i in range(25))
    display.show(Image(5, 5, pixels))

state = 0
lamp(0)
while True:
    if button_a.was_pressed():
        if state == 0:
            lamp(3)            
            state = 1
        elif state == 1:
            lamp(6)
            state = 2
        elif state == 2:
            lamp(9)
            state = 3
        elif state == 3:
            lamp(0)
            state = 0             
```  

**Variaties**

* als je twee knoppen hebt, dan kun je knop A gebruiken om de lichtsterkte te vergroten, en knop B om de lichtsterkte te verminderen. Dit kun je als automaat als volgt weergeven. Maak dit programma in Python.

:::{figure} figs/fsm-schakelaar-2.png
:width: 600
A geeft meer licht, B minder
:::

*Vraag* Wat gebeurt er als een knop soms weigert? In het geval van de automaat met 1 knop? en in het geval van de automaat met 2 knoppen? Welke automaat geeft dan het meest de bedoeling van de gebruiker weer?

## Besturen met een teller

Er is een andere manier om dit programma te maken, met dezelfde functionaliteit.
Gebruik een teller die achtereenvolgens de waarden 0, 1, 2, 3, 0, 1, 2, 3, 0, ... aanneemt.
Dit is een teller die "rond" telt net als een klok (1, 2, ... 11, 12, 1, 2).
We noemen dit soms ook een "modulo" teller, in dit geval "modulo 4". Het ophogen van de teller heeft dan de vorm `teller = (teller + 1) % 4`.  (hierin spreek je `% 4` uit als *modulo 4*: de rest na deling door 4. De rest van 4 gedeeld door 4 is weer 0, en de teller begint dan weer opnieuw.


```Python
from microbit import *

# set brightness level for "lamp"
def lamp(level):
    pixels = bytearray(level for i in range(25))
    display.show(Image(5, 5, pixels))

state = 0
lamp(0)
while True:
    if button_a.was_pressed():
        state = (state + 1) % 4
        lamp(state * 3)            
```  


