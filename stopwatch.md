# Stopwatch

Met de microbit maak je een stopwatch: met knop A start je de tijd, en met knop B stop je deze. De tijd op het display, in tienden van seconden, wordt weergegeven als binair getal. Schudden van de microbit laat de tijd zien in decimale notatie.

(In principe kun je de tijd ook met knop A weer stoppen? We moeten ook nog een mogelijkheid hebben om de tijd weer op 0 te zetten. Bijvoorbeeld: snel achter elkaar B indrukken.)

**Programma**

```Python
from microbit import *
import utime


def show_binary(num):
    pixels = bytearray(25)
    pos = 24
    while pos >= 0:
        pixels[pos] = 9 * (num % 2)
        num = num // 2
        pos = pos - 1
    display.show(Image(5, 5, pixels))


timer = utime.ticks_add(utime.ticks_ms(), 1000000)  # infinity?
running = False
duration = 0
while True:
    if button_a.was_pressed():
        running = True
        now = utime.ticks_ms()
        start_time = now
        timer = utime.ticks_add(now, 100)

    if button_b.was_pressed():
        running = False
        now = utime.ticks_ms()
        duration = utime.ticks_diff(now, start_time)
        show_binary(duration // 100)  # 100 ms units
        print(duratio)
        timer = utime.ticks_add(utime.ticks_ms(), 1000000)  # infinity?

    if accelerometer.was_gesture("shake"):
        display.scroll(duration // 100)
        print(duration)

    now = utime.ticks_ms()
    if running and utime.ticks_diff(now, timer) >= 0:
        time = utime.ticks_diff(now, start_time)
        show_binary(time // 100)
        timer = utime.ticks_add(timer, 100)
```

**Uitleg**

* de timer loopt alleen als knop A ingedrukt is, en starttime ongelijk aan 0 is.
* (het lijkt handiger om een aparte variabele "running" bij te houden.)


Uitbreidingen:

* netjes afronden op ms, in plaats van afbreken.
* het moet mogelijk zijn om de tijd even te stoppen, en daarna door te gaan. Dan wil je met het indrukken van A de timer herstarten met de bestaande tijd, in plaats van met een nieuwe tijd.
    * in dat geval moet je de tijd ook weer op 0 kunnen zetten; bijvoorbeeld door tweemaal binnen korte tijd B in te drukken. (Met andere woorden: ook voor B houden we een timer bij...)
* we hebben de variabele "time" niet echt nodig, omdat we deze kunnen uitrekenen op basis van de starttijd.

