# Afstandssensor

We gebruiken in dit voorbeeld een ultrasone afstandssensor.
Dit soort sensoren wordt bijvoorbeeld gebruikt als parkeersensor, 
om de afstand tussen de auto en de omliggende obstakels te meten.

Deze ultrasonse sensor werkt net als de echo-locatie van een vleermuis:
de sensor stuurt een (ultrasone) geluidsgolf van 40kHz uit en meet de tijd totdat de reflectie daarvan ontvangen wordt.
Uit de tijd en de snelheid van het geluid kun je dan de afstand uitrekenen.
In formule: afstand (m) = tijd (s) * snelheid (m/s).
Omdat de gereflecteerde geluidspuls tweemaal de afstand aflegt naar het doel, heen en terug, 
moeten we het resultaat door 2 delen.

De snelheid van het geluid is 343 m/s, bij 20'C.
Als we dit omrekenen met mm (millimeter) en us (microseconde), 
dan krijgen we als netto afstand tot het doel:

:::{math}
t * 10^{-6} * 343 * 10^{3} / 2 (mm) = t * 343 / 2000 (mm).
:::

De snelheid van het geluid in lucht hangt af van de temperatuur en van de luchtvochtigheid.
Je kunt de afstandsmeting nog preciezer maken door deze waarden te meten en daarvoor te corrigeren.

## Bedrading van de sensor

:::{figure} figs/ultrasone-afstandssensor.png
:width: 300
Ultrasone afstandssensor met aparte Trigger en Echo pinnen
:::

(Er is een speciale Octopus-versie van deze sensor; daarbij zijn de trigger-output en de echo-input gecombineerd op 1 pin.)

We beschrijven hier hoe je de sensor verbindt met het Octopus:bit bordje.
8V staat hier voor de rode (voedings)pin bij pin8, enz..

(https://www.elecfreaks.com/learn-en/microbitKit/Tinker_Kit/octopus_bit.html)

* sensor Vcc-pin aan pin 8V (rood)
* sensor GND-pin aan pin 8G (zwart)
* sensor trigger-pin aan pin 8S (blauw)
* sensor echo-pin aan pin 9S (blauw)

De schakelaar op het bordje staat daarbij op 5V. Bovenstaande pinnen werken dan met die hogere spanning.

**Opmerking** De meest voorkomende versie van deze sensor werkt op 5V; tegenwoordig werken sommige versies ook op 3V3, meestal is het bereik dan iets kleiner.

## Werking en gebruik van de sensor

Als de sensor een (positieve) puls van 10 microseconden ontvang op de "trigger" input, stuurt deze een geluidspuls van 8 periodes van 40 kHz uit.
Daarna verschijnt op de "echo" output een (positieve) puls die duurt van het moment van versturen van deze puls, tot het moment dan de puls weer ontvangen is. Uit de duur van deze echo-puls bepaal je dan de afstand.

:::{figure} figs/ultrasone-afstandssensor-timing.png
:width: 500
Tijdsdiagram van de signalen van de ultrasone afstandssensor
:::

Voor het meten van de lengte van een puls kun je de volgende bibliotheekfunctie gebruiken:

```
machine.time_pulse_us(pin, pulse_level, timeout_us=1000000, /)
```

Deze functie meet de lengte van een puls (in microseconden) op de gegeven pin.
Je kunt aan deze functie een time-out meegeven: als de puls langer duurt, dan keert de functie terug met een fout-waarde (-1 als er helemaal geen pulse gesignaleerd is, -2 als de puls te lang duurde).

**timeout: maximale afstand.** De maximale afstand is ca. 4 m. De duur van de echo-puls is dan 2 * 4 / 343 (s) of 8000 / 343 (ms) = ca. 23 ms.
We gebuiken voor de timeout van deze functie als benadering 25 ms.
Merk op dat de functie voor het meten van de echo-puls gedurende deze meetperiode blokkeert.
Dat is in ons geval geen probleem, omdat we geen processen hebben met een kortere reactietijd dan 25 ms.
Voor andere systemen kan dat een probleem zijn, dan moet je de duur van de echo-puls op een andere manier meten.

Met deze functie maken we dan het volgende programma:

```Python
from microbit import *
import machine
import utime

trigger_pin = pin8
echo_pin = pin9
trigger_pin.write_digital(0)

while True:
    trigger_pin.write_digital(1)
    utime.sleep_us(10)
    trigger_pin.write_digital(0)
    
    duration = machine.time_pulse_us(echo_pin, 1, 25000)  # time-out 25 ms
    if duration > 0:
        distance = duration * 343 // 2000        # in mm
        print("afstand (mm): " + str(distance))
    else:
        print("te ver")
    sleep(1000)    
```

## Parkeersensor

De parkeersensor combineert de afstandssensor met een alarm:
hoe kleiner de gemeten afstand, hoe sneller het alarm knippert (of klinkt).

We gebruik een timer voor de knipperende LED.
De periode hiervan maken we evenredig met de afstand.
(Bijvoorbeeld: een afstand van 100 mm (10 cm) komt overeen met een periode van 50ms; - afstand / 2
een afstand van 2000 mm (2 m) komt overeen met een periode van 1s)
We laten de LED in deze periode 10 ms aanstaan: dat is voldoende zichtbaar.


```Python
from microbit import *
import machine
import utime

trigger_pin = pin8
echo_pin = pin9
trigger_pin.write_digital(0)

ledpin = pin0
ledpin.write_digital(0)
period = 100000        # disable timer 
timer = 0              # not relevant, timer disabled

while True:
    
    trigger_pin.write_digital(1)
    utime.sleep_us(10)
    trigger_pin.write_digital(0)
    
    duration = machine.time_pulse_us(echo_pin, 1, 25000)  # time-out 25 ms
    if duration > 0:
        distance = duration * 343 // 2000        # in mm
        print("afstand (mm): " + str(distance))
        period = distance // 2  ## 1 m (1000mm): 500ms
        
        timer_new = utime.ticks_add(utime.ticks_ms(), period)
        if utime.ticks_diff(timer_new, timer) < 0:
            timer = timer_new
    else:
        print("te ver")
        period = 100000 # disable timer
        
    if period < 10000 and utime.ticks_diff(utime.ticks_ms(), timer) > 0:
        ledpin.write_digital(1)
        sleep(10)
        ledpin.write_digital(0)
        timer =  utime.ticks_add(utime.ticks_ms(), period)
        
    sleep(10)
```

**Opmerkingen**

* we hebben dezelfde methode gebruikt voor het aanpassen van de timer: deze wordt alleen aangepast als de nieuwe timerwaarde kleiner is dan de al lopende timer. (zie led-met-timer).