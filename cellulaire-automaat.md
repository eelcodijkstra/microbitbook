(cellulaire-automaat)=
# Cellulaire automaat

:::{figure} figs/mb-cellulaire-automaat.png
:width: 250
:align: right

Cellulaire automaat
:::

In deze opdracht gebruik je de microbit voor het simuleren van een 1-dimensionale *cellulaire automaat*.
De automaat is beperkt tot de breedte van de microbit: 5 cellen.
Het display van de microbit geeft (maximaal) 5 opeenvolgende generaties van deze automaat.

## Gebruik

We beschrijven eerst het gebruik van het programma, en daarna leggen we de werking uit.

* door te schudden begin je met een nieuwe toestand: `[0,0,0,0,1]`
* knop A berekent de volgende generatie uit de huidige generatie: deze komt onder de huidige op het display.
* knop B past de huidige generatie aan, door er (binair) 1 bij op te tellen.
    * hiermee kun je na het schudden de begintoestand aanpassen 

Je kunt experimenteren met de verschillende regels voor het bepalen van de volgende generatie.
Hiervoor moet je wel de code aanpassen (aanroep `setrulenr(...)`).

## Programma

Het programma is vrijwel identiek aan het programma in {ref}`cellulaire-automaten`,
met als grootste verschillen de weergave van de toestand op het microbit-display,
en de interactie met de gebruiker.
Zie voor een uitleg van de code aldaar.

```Python
from microbit import *

rule = [0, 0, 0, 0, 0, 0, 0, 0]  # rule represented as 8 bits

size = 5      # number of cells in a state
state = []    # a list of cells
history = []  # a list of states

# create a new cellular automaton
# nr_cells: number of cells
# ones:     positions of living cells
def create_state(nr_cells, ones):
    global size, state, history, gen_nr
    size = nr_cells
    state = []
    for i in range(size):
        state.append(0)
    for cellnr in ones:
        if cellnr >= 0 and cellnr < nr_cells:
            state[cellnr] = 1
    history = [state]

# set rule-list from rule-number
def set_rule(rulenr):
    global rule
    for r in range(8):
        rule[r] = rulenr % 2
        rulenr = rulenr // 2

# cell-value in current state,
# with border-cells with value 0
def cell(i):
    if i < 0 or i >= len(state):
        return 0
    else:
        return state[i]

# compute next state
# and add to history
def step_state():
    global state, history
    next_state = []
    for i in range(size):
        cell_state = cell(i-1) * 4 + cell(i) * 2 + cell(i+1)
        next_state.append(rule[cell_state]) 
    state = next_state
    history.append(state)
    if len(history) > 5:
        history.pop(0)

def inc_state():
    statenr = 0
    for cell in state:
        statenr = statenr * 2 + cell
    statenr = statenr + 1
    for i in range(size):
        state[-i-1] = statenr % 2
        statenr = statenr // 2 
        
def display_row(rownr, row):
    for colnr in range(len(row)):
        display.set_pixel(colnr, rownr, row[colnr] * 9)

def display_history():
    display.clear()
    # history is at most 5 elements long
    for rownr in range(len(history)):
        display_row(rownr, history[rownr])
            
create_state(size, [4])
set_rule(110)
display_history()
while True:
    if accelerometer.was_gesture("shake"):
        create_state(size, [4])
        display_history()
    elif button_a.was_pressed():
        step_state()
        display_history()
    elif button_b.was_pressed():
        inc_state()
        display_history()
    sleep(50)
```