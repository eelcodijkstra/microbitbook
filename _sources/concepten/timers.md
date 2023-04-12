(timer-concept)=
# Timers en timer-events (Python)

Het gebruik van `sleep` in microbit-toepassingen levert soms problemen op:
dit is een blokkerende opdracht, dat wil zeggen, tijdens deze sleep zijn er geen andere activiteiten mogelijk.
In het bijzonder kun je geen input-events detecteren: de microbit is tijdelijk "doof" (want slapend).

Een oplossing hiervoor is het gebruik van timers en timer-events.
Een timer kun je zien als een kookwekker die na een bepaalde periode afloopt.
Het aflopen van een timer is een *timer-event*, die je kunt behandelen als andere events.
Je kunt aan het aflopen van een timer een actie verbinden (event-handler).

Voor deze timers maak je gebruik van de ingebouwde timer van de microbit.
De functie `utime.ticks_ms()` geeft het aantal milliseconden vanaf een willekeurig moment.
Het resultaat, een "ticks" waarde, kun je alleen gebruiken in de volgende functies.

* `utime.ticks_add(ticks, delta)`: `ticks + delta`
    * resultaat is een "ticks" waarde
* `utime.ticks_diff(ticks1, ticks2)`: `ticks1 - ticks2`
    * resultaat is een `int` waarde

De gewone int-operaties als `+` en `-` geven voor "ticks" waarden een onvoorspelbaar resultaat.

> De reden hiervoor is dat "ticks" een eindig bereik heeft: als de maximale waarde bereikt is, begint deze weer bij 0 ("wrap around"), net als een gewone klok. Door de genoemde functies te gebruiken, met verschillen (`delta`) die kleiner zijn dan de helft van het bereik, krijg je altijd een voorspelbaar resultaat.
> Zie: https://microbit-micropython.readthedocs.io/en/v2-docs/utime.html

Er is ook een functie `utime.ticks_us()` die het aantal microseconden geeft vanaf een willekeurig moment.

Eem timer is niets anders dan een getal ("ticks"-waarde) dat het tijdstip voorstelt waarop de timer afloopt.
Door dit getal te vergelijken met de actuele waarde van de microbit-timer zie je of deze timer afgelopen is of niet.

* zetten van de timer die over 500 milliseconden afloopt : 
    * `deadline = utime.ticks_add(utime.ticks_ms(), 500)`
* is de timer afgelopen? 
    * `if ticks_diff((utime.ticks_ms(), deadline) >= 0: ...action...`
    * dit lees je als `now - deadline >= 0` ofwel `now >= deadline`;      
    * `now` is: `utime.ticks_ms()`;
    * het aflopen van een timer is een *timer-event*
    
Aan een timer-event koppelen we gewoonlijk een actie,
met bijvoorbeeld een overgang naar een andere toestand.

:::{figure} figs/timer-expiring-exc.png
:width: 500

Timer: (i) nog niet afgelopen; even later, (ii) timer afgelopen
:::

Hieronder het "kloppend hard" programma maar nu met een timer:

```Python
from microbit import *
import utime

state = 0
display.show(Image.HEART)
period = 500
timer = utime.ticks_add(utime.ticks_ms(), period)

while True:
    now = utime.ticks_ms()
    if utime.ticks_diff(now, timer) >= 0 :
        timer = utime.ticks_add(now, period)
        if state == 0:
            display.show(Image.HEART_SMALL)
            state = 1
        elif state == 1:
            display.show(Image.HEART)
            state = 0
```

Uitleg bij dit programma:

* als de timer afloopt (timer event), dan
    * wordt de timer direct weer gestart voor de volgende periode
    * vindt er een toestandsovergang plaats - afhankelijk van de huidige toestand.
