# LED met timer

## Knipperende LED met `sleep`

Bekijk het onderstaande programma:

```Python
from microbit import *
import utime

ledpin = pin0   # LED, digitale output
potpin = pin1   # potmeter, analoge input

period = 500
timer = utime.ticks_add(utime.ticks_ms(), period)
ledpin.write_digital(1)
state = 0

while True:
    sleep(period)
    state = 1 - state
    ledpin.write_digital(state)
            
    period = potpin.read_analog() * 5
```

> `state = 1 - state` zorgt voor de overgang van 0->1 of van 1->0 (ga dit na).

**Vraag.** Wat doet dit programma? (Beschrijf het gedrag van het programma zonder dit uit te voeren. Schrijf dit kort op.)
(Bedenk eerst wat het programma doet zonder de laatste opdracht. Wat zal de invloed van deze laatste opdracht zijn?)

**Voer het programma uit...** en controleer je beschrijving.

**Vraag.** Wat is de langste periode, als je weet dat de maximale waarde voor een analoge input 1023 is? (Deze analoge input is een 10 bits geheel getal).

**Vraag.** Wat gebeurt er als je de potmeter van de grootste waarde van `period` omzet naar de laagste? (Doe dit bijvoorbeeld als de LED net uitgaat, voor een duidelijk effect.) Verklaar dit gedrag.

## Knipperende LED met timer

In het onderstaande voorbeeld gebruiken we een timer (zie [](timer-concept)) voor het knipperen van de LED,
in plaats van de `sleep`-functie.
Dit geeft de mogelijkheid om de timer, en daarmee de knipper-snelheid, in te stellen met een *potentiometer* (draaiknop-sensor).

```Python
from microbit import *
import utime

ledpin = pin0   # LED, digitale output
potpin = pin1   # potmeter, analoge input

period = 500
timer = utime.ticks_add(utime.ticks_ms(), period)
ledpin.write_digital(1)
state = 0

while True:
    now = utime.ticks_ms()
    if utime.ticks_diff(now, timer) >= 0 :
        timer = utime.ticks_add(now, period)
        state = 1 - state
        ledpin.write_digital(state)
            
    period = potpin.read_analog() * 5
    new_timer = utime.ticks_add(now, period)
    if utime.ticks_diff(new_timer, timer) < 0:
        timer = new_timer
```

**Vraag** Wat doet dit programma?

**Vraag** Waarom wordt de deadline aangepast aan het minimum van de oude deadline en de nieuwe deadline?

## Discussie

Het eerste voorbeeld geeft aan dat het gebruik van `sleep` in programma's met invoer heel lastig kan zijn: gedurende de `sleep`-periode wordt er geen invoer verwerkt, en is de computer "doof".
In veel physical computing toepassingen is dat lastig, omdat de buitenwereld zich niet aanpast aan de computer.
Denk aan een robot die even niet de sensor voor een obstakel uitleest...

Door het gebruik van *timers* kun je werken met tijd zonder dat de invoer geblokkeerd wordt. Dit gebruik van timers kun je goed combineren met andere events, bijvoorbeeld voor het aansturen van de overgangen van een eindige automaat.

(Soms is het gebruik van `sleep` met een kleine waarde, bijvoorbeeld enkele milliseconden, wel toegestaan: dat hangt af van de snelheid van de verschijnselen in de omgeving, waarmee de computer rekening moet houden.)

