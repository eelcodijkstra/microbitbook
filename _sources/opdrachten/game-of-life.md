# Game of Life

:::{Admonition} Laad het programma naar de microbit
Kopieer het programma hieronder naar de editor die je gebruikt, en laad het op de microbit (of gebruik de simulator).

Gebruik:

* knop A geeft een vast patroon (kruisje) als begin-generatie.
* knop B geeft steeds de volgende generatie.
* schudden geeft een nieuwe (random) begin-generatie.

:::

Het 2-dimensionale speelveld van Conway's Game of Life bestaat uit cellen die levend (zwart) of dood (wit) kunnen zijn.
Of een cel overleeft naar de volgende generatie, afsterft, of juist levend wordt, hangt af van die cel en van het *aantal levende buren* (van de 8 direct omliggende buren).

De microbit heeft een klein (5x5) Life-veld dat je op het display ziet; dit is omgeven door een rand met cellen die altijd dood zijn. 

**Opdracht.** Teken in de bijgaande figuur 4 opeenvolgende generaties van Life, aan de hand van het programma op de microbit.

:::{figure} figs/game-of-life-4.png
:width: 600

Game of Life
:::

**Opdracht.** Probeer aan de hand van deze opeenvolgende generaties uit te vinden wat de regels van het spel zijn.

(Je kunt dit controleren op Wikipedia: https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life; of in het programma hieronder.)


## Programma

```Python
from microbit import *
import random

def create_random_field():
    field = []
    for i in range(5):
        row = []
        for j in range(5):
            row.append(random.randrange(2))
        field.append(row)
    return field

def create_field():
    field = []
    for i in range(5):
        row = []
        for j in range(5):
            row.append(0)
        field.append(row)
    return field

def create_cross_field():
    field = create_field()
    field[1][2] = 1
    field[2][1] = 1
    field[2][2] = 1
    field[2][3] = 1
    field[3][2] = 1
    return field

def cell_value(r, c):
    if r < 0 or r > 4 or c < 0 or c > 4:
        return 0  # border cells
    else:
        return cells[r][c]

# count living neighbours
def count_neighbours(r, c):
    total = - cell_value(r, c)
    for i in range(3):
        for j in range(3):    
            total = total + cell_value(r + i - 1, c + j - 1)
    return total

def next_cell(cell, cnt):
    if cell == 1 and (cnt == 2 or cnt ==3):
        return 1
    elif cell == 0 and cnt == 3:
        return 1
    else:
        return 0
    
def next_generation():
    new_cells = create_field()
    for i in range(5):
        for j in range(5):
            cnt = count_neighbours(i, j)
            new_cells[i][j] = next_cell(cells[i][j], cnt)
    return new_cells

def display_field(field):
    for x in range(5):
        for y in range(5):
            display.set_pixel(x, y, 9 * field[y][x])

cells = create_random_field()
display_field(cells)
while True:
    if button_a.was_pressed():
        cells = create_cross_field()
        display_field(cells)
    if button_b.was_pressed():
        cells = next_generation()        
        display_field(cells)
    if accelerometer.was_gesture('shake'):
        cells = create_random_field()
        display_field(cells)
    sleep(100)    
```