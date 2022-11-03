# Stemmen

::::{grid}

:::{grid-item-card}
:class-header: bg-light
**Voorkennis**
^^^

* buttons en events
* eindige automaten

:::

:::{grid-item-card} 
:class-header: bg-light
**Concepten**
^^^

* radio
* protocol

:::

::::

Je maakt met behulp van de microbit een stemkastje, waarmee je je keuze (A of B) via de microbit-radio verstuurt naar een centrale "stembus", waar de stemmen geteld worden. 

Voor de communicatie tussen de stemkastjes en de stembus gelden de volgende afspraken:

* een stemronde begint met een 'start'-bericht van de stembus naar de stemkastjes;
    * stemmen ontvangen voor dit start-bericht tellen niet mee
* na het ontvangen van het start-bericht wordt het stemkastje actief (`state == ACTIVE`).
* een actief stemkastje mag 1 stem uitbrengen; daarna wordt het afgesloten (`state == SENT`, tot de volgende stemronde)
* de stembus stuurt een 'exit' bericht naar de stemkastjes om de stemronde af te sluiten;
    * de stembus telt de stemmen
* na het ontvangen van het exit-bericht wordt het stemkastje inactief (`state == INACTIVE`) en mag geen stem meer uitbrengen.

Dergelijke afspraken voor de communicatie tussen twee of meer partijen noemen we een (communicatie-)
*protocol*.

:::{figure} figs/fsm-voting.png
:width: 400
Toestanden van het stemkastje. 0: INACTIVE; 1: ACTIVE; 2: SENT
:::

Voor testdoeleinden staan we toe dat het stemkastje meerdere stemmen mag uitbrengen.

Het programma voor het stemkastje:

```Python
import radio

INACTIVE = 0
ACTIVE = 1
SENT = 1  # test! should be 2 in final version

state = INACTIVE
radio.on()
display.show('X')
while True:
    if button_a.was_pressed():
        if state == ACTIVE:
            radio.send('A')
            display.show('A')
            state = SENT
        else:
            pass
    if button_b.was_pressed():
        if state == ACTIVE:
            radio.send('B')
            display.show('B')
            state = SENT
    msg = radio.receive()
    if msg is not None:
        if msg == 'start':
            state = ACTIVE
            display.show('O')
        elif msg == 'stop':
            state = INACTIVE
            display.show('X')
```

Het programma voor de stembus:

```Python
from microbit import *
import radio

a_count = 0
b_count = 0
radio.on()
print('Voting app')
print('s for start vote, x for exit (stop) vote')
while True:
    if uart.any():
        cmd = uart.read()
        if cmd == b's':
            radio.send('start')
            a_count = 0
            b_count = 0
            print('start')
        elif cmd == b'x':
            print('exit')
            radio.send('stop')
            print('total A: {} B: {}'.format(a_count, b_count))
        else:
            print('unknown cmd: ' + cmd)
    msg = radio.receive()
    if msg is not None:
        if msg == 'A':
            a_count = a_count + 1
            print('A')
        elif msg == 'B':
            b_count = b_count + 1
            print('B')
        else:
            print('unknown response: ' + msg)
```

Over het gebruik van radio:

* voordat je de radio kunt gebruiken, moet je (i) de module importeren; (ii) de radio aanzetten, met `radio.on()`.
* met `radio.send(s)` stuur je een string naar de ontvanger(s). Alle radio's met dezelfde kanaal- en groep-instellingen ontvangen dit bericht.
* met `radio.receive()` haal je een ontvangen bericht op als string; als er geen bericht is, is het resultaat `None`. Het ontvangen van een bericht kun je zien als een *event*.

De microbit-radio is een voorbeeld van een *pakketradio*: de communicatie gebeurt in de vorm van kleine pakketten (maximaal 248 bytes; default maximum: 32 bytes). Als een pakket ontvangen is, controleert de hardware via de *checksum* of dit onbeschadigd is. Maar er is geen garantie dat een verzonden pakket ook daarwerkelijk ontvangen wordt: de communicatie is *best effort*. Je kunt deze commnicatie *betrouwbaar* maken door een extra protocol te gebruiken, bij voorbeeld door het versturen van bericht van ontvangst (*acknowledgement*).

**Uitbreidingen**

Er is geen enkele garantie voor de betrouwbaarheid van de communicatie: een radiobericht kan verloren gaan door bijvoorbeeld onderlingen botsingen ("collisions"). Een manier om communicatie betrouwbaar te maken is het versturen van een bericht van ontvangst ("acknowledgement"). Bedenk een manier waarop je de betrouwbaarheid van de verschillende berichten kunt vergroten. Wat is daar voor nodig?

Met twee knoppen kun je meer dan twee verschillende stemmen uitbrengen. Pas het stemkastje aan zodat je door tweemaal drukken uit 4 alternatieven kunt kiezen: A, B, C, of D. Denk erom dat je dan ook de "stembus"-code moet aanpassen.






