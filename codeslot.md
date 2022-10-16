# Codeslot

Je kunt het codeslot ontsluiten met de microbit-knoppen als je de geheime sleutel kent: voor dit voorbeeld is deze BABB (of 1101).
Dit codeslot kun je ook gebruiken als onderdeel van andere programma's, zie de voorbeelden verderop.

::::{grid}

:::{grid-item-card}
:class-header: bg-light
**Voorkennis**
^^^

* buttons en events
* variabelen en toekenning


:::

:::{grid-item-card} Concepten
:class-header: bg-light
header
^^^

* eindige automaat,
* toestand, toestandsovergang
* hexadecimale getalvorm

:::

::::

## Eindige automaat

We beschrijven het codeslot eerst als eindige automaat. Deze automaat zetten we dan volgens de regels om in een Python programma.

Deze versie van het codeslot is beperkt tot 4 bits codes. Je kunt de automaat en het programma eenvoudig uitbreiden voor langere codes.

:::{figure} figs/codeslot-automaat.png
:width: 600

Codeslot als eindige automaat
:::

In deze figuur gebruiken we de afkorting: `x,y/P`: dit staat voor twee overgangen (pijlen): `x/P`, ofwel input event `x` en output `P`; en `y/P`, input event `y` en output `P`. Dit scheelt het tekenen van pijlen. In de tabellen verderop moeten we deze overgangen wel uitschrijven!

### Automaat in tabelvorm

We kunnen de overgangen van deze eindige automaat weergeven in de volgende tabel, met een rij per overgang. (In deze tabelvorm moeten we alle overgangen compleet uitschrijven, we kunnen geen afkortingen gebruiken.)

| state | input | output | state |
| :---  | :---  | :---   | :---  |
| 0     |  a    |        | 0     |
| 0     |  b    |        | 1     |
| 0     |  S    |        | 0     |
| 1     |  a    |        | 2     |
| 1     |  b    |        | 0     |
| 1     |  S    |        | 0     |
| 2     |  a    |        | 0     |
| 2     |  b    |        | 3     |
| 2     |  S    |        | 0     |
| 3     |  a    |        | 0     |
| 3     |  b    | unlock | 4     |
| 3     |  S    |        | 0     |
| 4     |  a    | reset  | 0     |
| 4     |  b    | reset  | 0     |
| 4     |  S    | reset  | 0     |

Groeperen van de rijen volgens de input (sorteren op input, dan op state) geeft:

| state | input | output | state |
| :---  | :---  | :---   | :---  |
| 0     |  a    |        | 0     |
| 1     |  a    |        | 2     |
| 2     |  a    |        | 0     |
| 3     |  a    |        | 0     |
| 4     |  a    | reset  | 0     |
| 0     |  b    |        | 1     |
| 1     |  b    |        | 0     |
| 2     |  b    |        | 3     |
| 3     |  b    | unlock | 4     |
| 4     |  b    | reset  | 0     |
| 0     |  S    |        | 0     |
| 1     |  S    |        | 0     |
| 2     |  S    |        | 0     |
| 3     |  S    |        | 0     |
| 4     |  S    | reset  | 0     |

### Het programma

De bovenstaande tabel zetten we om in het onderstaande Python-programma:

```Python
from microbit import *

state = 0

def unlock():
    display.show(Image.HEART)

def reset():
    display.show(Image.CONFUSED)

def button_a_handler():
    global state
    if state == 0:
        state = 0
    elif state == 1:
        state = 2
    elif state == 2:
        state = 0
    elif state == 3:
        state = 0
    elif state == 4:
        state = 0
        reset()
        
def button_b_handler():
    global state
    if state == 0:
        state = 1
    elif state == 1:
        state = 0
    elif state == 2:
        state = 3
    elif state == 3:
        state = 4
        unlock()
    elif state == 4:
        state = 0
        reset()

def shake_handler():
    global state
    state = 0
    reset()

reset()
while True:
    if button_a.was_pressed():
        button_a_handler()
    elif button_b.was_pressed():
        button_b_handler()
    elif accelerometer.was_gesture("shake"):
        shake_handler()
    microbit.sleep(5) // msec 
```

* de `sleep` is nodig voor het detecteren van gebaren zoals "shake".

## Variaties en uitbreidingen

* verander de code in "1010" (je moet dan 2 overgangen aanpassen!)
* breid de code uit met 1 bit (denk erom dat je automaat volledig moet zijn!)
* blokkeer het codeslot tijdelijk, bijvoorbeeld 10 seconden, na het invoeren van een foute code.

## Voorbeelden

Je kunt dit codeslot ook gebruiken als onderdeel van een ander programma. Enkele suggesties:

**Bewaak je geheime la.** Je kunt de microbit plaatsen in je geheime la. Zodra de microbit beweging detecteert, gaat het pre-alarm af, bijvoorbeeld een knipperend display. Als je dan binnen 15 seconden het codewoord invoert, gaat de microbit weer in rust-toestand. Als het coderwoord niet op tijd ingevoerd wordt, gaat het hoorbare alarm af.
(Met behulp van meerdere microbits die via radio communiceren kun je dit nog veiliger maken.)

**Inschakelen van een apparaat.** Je kunt een programma voor een robot bijvoorbeeld zo maken dat dit pas begint als het juiste codewoord is ingevoerd.

Bedenk zelf andere voorbeelden van het gebruik van het codeslot.

## Langere codes

## Hexadecimale getalvorm

:::{margin}
Je kunt in plaats van A..F ook kleine letters gebruiken, a..f.
:::

Als de code langer wordt, is het lastig om deze te onthouden als een reeks 0-en en 1-en.
Je kunt dan handig gebruik maken van de *hexadecimale getalvorm*: 
een rijtje van 4 bits komt overeen met een hexadecimaal "cijfer": een cijfer (0..9) of een letter (A..F).

In de onderstaande tabel zie je de binaire vorm, de hexadecimale vorm, en de decimale vorm van een rijtje van 4 bits.

| binaire vorm | hexadecimale vorm | decimale vorm |
| :---:        | :---:             | :---:         |
| 0000         | 0                 | 0             |
| 0001         | 1                 | 1             |
| 0010         | 2                 | 2             |
| 0011         | 3                 | 3             |
| 0100         | 4                 | 4             |
| 0101         | 5                 | 5             |
| 0110         | 6                 | 6             |
| 0111         | 7                 | 7             |
| 1000         | 8                 | 8             |
| 1001         | 9                 | 9             |
| 1010         | A                 | 10            |
| 1011         | B                 | 11            |
| 1100         | C                 | 12            |
| 1101         | D                 | 13            |
| 1110         | E                 | 14            |
| 1111         | F                 | 15            |

Het hexadecimale getal A9 staat dan voor het bitpatroon 1010 1001.
Als je dit als getal op zou vatten heeft dit de waarde 169 (decimaal),
maar vaak beschouw je dit gewoon als rij bits, zoals voor het codeslot,
en doet de getalwaarde er niet toe.

