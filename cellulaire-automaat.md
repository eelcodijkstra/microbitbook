(cellulaire-automaat)=
# Cellulaire automaat

In deze opdracht gebruik je de microbit voor het simuleren van een 1-dimensionale *cellulaire automaat*.
De automaat is beperkt tot de breedte van de microbit: 5 cellen.
Het display van de microbit geeft (maximaal) 5 opeenvolgende generaties van deze automaat.

## Gebruik

We beschrijven eerst het gebruik van het programma, en daarna leggen we de werking uit.

* door te schudden maak je een nieuwe *seed*: dit is de eerste generatie, die bovenin het display staat.
* knop B berekent de volgende generatie uit de huidige generatie: deze komt onder de huidige op het display.

Je kunt experimenteren met de verschillende regels voor het bepalen van de volgende generatie.

## Programma


een "generatie" (`gen`) bestaat uit een rij (list) van getallen 0 en 1, van lengte 5.

```Python
import random

rule = (0, 0, 0, 0, 0, 0, 0, 0)  # a rule is given by a number, or 8 bits
gen_nr = 0
field = []

def new_seed():
    global gen_nr
    gen_nr = 0
    seed = random.randbytes(1)
    create_field()
    for i in range(5):
        field[0][i] = seed % 2
        seed = seed // 2

def create_field():
    global field
    row = []
    for i in range(5):
        row.append(0)
    field = []
    for i in range(5):
        field.append(row)

def set_rule(rulenr):
    global rule
    for r in range(8):
        rule[r] = rulenr % 2
        rulenr = rulenr // 2

def next_gen(gen):
    cur_gen = gen
    cur_gen.insert(0, 0)  # add border cells
    cur_gen.insert(6, 0)
    next_gen = (0, 0, 0, 0, 0)
    for i in range(5):
        cell_state = cur_gen(i-1) * 4 + cur_gen(i) * 2 + cur_gen(i+1)
        next_gen[i] = rule[cell_state]
    return next_gen    
     
def next_row(rownr):
    prev_row = (0, 0, 0, 0, 0)
    for i in range(r):
        prev_row[i] = display.get

def next_row():
    global gen_nr
    if gen_nr < 4:
        field[gen_nr + 1] = nex_gen(field[gen_nr])
        gen_nr = gen_nr + 1
    else:
        field.pop(0)
        field.append(next_gen(field[3]))
        gen_nr = gen_nr + 1

def display_field():
    display.clear()
    for r in range(len(field)):
        row = field[r]
        for c in range(len(row)):
            display.set_pixel(r, c, 9 * row[c])

while True:
    if accelerometer.was_gesture("shake"):
        new_seed()
        display_field()
    if button_a.was_pressed():
        next_row()
        display_field()
    sleep(10)
```

* we moeten het "field" weergeven op het display, na elke verandering

NB: dit kan gemakkelijker met `for i in list`.