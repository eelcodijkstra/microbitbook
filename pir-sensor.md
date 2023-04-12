# Energiebesparende verlichting

In dit voorbeeld behandelen we verschillende manieren om energie te besparen met verlichting.
Het basisidee is dat verlichting alleen nodig is als er mensen in de buurt zijn.
We gebruiken twee manieren om de aanwezigheid van mensen te detecteren: 
(i) een schakelaar; en (ii) een bewegingsdetector.

## Tijdschakelaar

### Schakeling

We gebruiken een LED aangesloten op pin 0 van de microbit,
bijvoorbeeld een Octopus-LED aangesloten op de GVS-pinnen van pin1 van een Octopus:bit bordje.

### Programma

Bekijk het onderstaande programma:

```Python
from microbit import *
import utime

ledpin = pin0
ledpin.write_digital(0) # led off

period = 3000 # 3 seconds, for demo
timer = 0
timer_running = False

while True:
    if button_a.was_pressed():
        ledpin.write_digital(1)
        timer_running = True
        timer = utime.ticks_add(utime.ticks_ms(), period)
        
    if timer_running and utime.ticks_diff(utime.ticks_ms(), timer) > 0:
        ledpin.write_digital(0)
        timer_running = False
```

**Beschrijf kort** wat je denkt dat dit programma doet.

**Voer het programma uit** en ga na of dit klopt met je voorspelling.

**Beantwoord de volgende vragen:**

* Wat gebeurt er als je de knop indrukt terwijl de LED niet brandt?
* Wat gebeurt er als je de knop indrukt terwijl de LED brandt?
* Wat is de functie van de Boolean variabele `timer_running`? Wat kun je zeggen over die variabele en de toestand van de LED?
    * Is deze variabele strikt gezien nodig?

## Bewegingsdetector 

:::::{grid} 2
::::{grid-item}
:::{figure} figs/octopus-pir-sensor.png
:width: 250

Octopus PIR sensor
:::
::::

::::{grid-item}
:::{figure} figs/instelbare-pir-sensor.png
:width: 200

Generieke instelbare PIR sensor
:::
::::
:::::

Als bewegingsdetector gebruiken we een PIR sensor ("passive infrared" sensor).

### Werking van de sensor

De PIR-sensor maakt gebruik van de infraroodstraling van warme lichamen, bijvoorbeeld van mensen.
Op het moment dat de sensor beweging detecteert, wordt de uitgang *hoog*.
Deze blijft *hoog* gedurende een bepaalde periode (de "delay").
Als beweging gedetecteerd wordt tijdens deze *hoog*-periode, dan gaat de delay-tijd opnieuw in.

Sommige sensoren zijn instelbaar: je kunt dan meestal de delay-periode instellen en de gevoeligdheid.
Als je de gevoeligheid groter maakt, dan detecteert de sensor de beweging van zwakker stralende lichamen,
of van lichamen op grotere afstand. (Je kunt alleen de gevoeligheid instellen, niet de afstand niet instellen)

De Octopus-PIR-sensor bevat een LED die brandt als de uitgang *hoog* is. (Dat is handig om de werking van je programma te controleren.)

### Functie van het programma

Als de PIR-sensor beweging detecteert, schakelt de LED aan.
Deze blijft branden gedurende een bepaalde periode (bijvoorbeeld 2 minuten; in de demo, 5 seconden).
Als er in die periode beweging gedetecteerd wordt, gaat een nieuwe periode in.
Met andere woorden: als er voldoende beweging is, blijft de LED branden.

(Merk op dat de *delay* van de PIR-sensor hierbij geen rol speelt: deze wordt vervangen door de vertragingsperiode van het programma.)

### De schakeling

* De LED is aangesloten op pin 0 van de micro:bit.
* De uitgang van de PIR-sensor is aangesloten op pin 2 van de micro:bit.

### Het programma

Het programma detecteert het begin van de positieve puls van de PIR-sensor:
dan wordt de LED ingeschakeld.
Het programma detecteert ook het einde van de positieve puls van de PIR-sensor:
dan wordt de timer gezet om de LED uit te schakelen.
De LED wordt uitgeschakeld als de timer afloopt en het PIR-level *laag* is.
(*vraag*: waarom is die laatste conditie nodig?)

* Voor timers: zie [](timer-concept).

```Python
from microbit import *
import utime

ledpin = pin0
pirpin = pin2

ledpin.write_digital(0) # led off
pirlevel = 0

period = 5000 # 5 seconds, for demo
timer = 0

while True:
    oldlevel = pirlevel
    pirlevel = pirpin.read_digital()
    if oldlevel == 0 and pirlevel == 1:
        ledpin.write_digital(1)
    
    if oldlevel == 1 and pirlevel == 0:
        timer = utime.ticks_add(utime.ticks_ms(), period)
        
    if pirlevel == 0 and utime.ticks_diff(utime.ticks_ms(), timer) > 0:
        ledpin.write_digital(0)
```

**Voer het programma uit,** en controleer dat de LED blijft branden als er voldoende beweging is, 
ook als de PIR-sensor even uit is.

**Vragen**

* Wat is de totale tijd tussen de laatste beweging en het moment dat de LED uit gaat?

**Variatie**

* voeg een potmeter (analoge input) toe om de periode van de LED in te stellen.
