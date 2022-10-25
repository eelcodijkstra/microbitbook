---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

(cellulaire-automaten)=
# Cellulaire automaten

:::{margin}
Conway's Game of Life is een voorbeeld van een 2-dimensionale cellulaire automaat.
Je hebt dan een vlak met cellen (witte en zwarte hokjes).
:::

Een 1-dimensionale cellulaire automaat bestaat uit een reeks aaneengesloten cellen. 
Een cel is "levend" (zwart, 1) of "dood" (wit, 0).
De toestand van een automaat op een bepaald moment geeft aan welke cellen levend zijn, en welke dood.
Dit kun je weergeven als een (horizontale) rij zwart/witte cellen (hokjes).

:::{figure} figs/automaton-state.png
:width: 300
:align: center

Toestand van een cellulaire automaat
:::

Gegeven een toestand van een automaat, kun je voor elke cel de volgende toestand bepalen uit de toestand van die cel en die van zijn directe buren.
Je kunt de opeenvolging van toestanden weergeven door deze onder elkaar te tekenen, in een plat vlak.

**Voorspel de volgende toestanden.**
Bekijk de volgende reeks toestanden, waarbij de rijen 4 en 5 nog niet ingevuld zijn:

:::{figure} figs/automaton-sequence.png
:width: 300
:align: center

Opeenvolging van toestanden
:::

:::::{tab-set}
::::{tab-item} Vraag

Kun je voorspellen welke waarden je krijgt in rijen 4 en 5?

::::

::::{tab-item} Tip: bepaal de regels

Tip: bepaal voor *elke combinatie* van een cel met zijn directe buren, wat de volgende toestand is voor die cel:
welke waarden moeten er staan op de plek van de vraagtekens?
Dit zijn de regels van de cellulaire automaat.

:::{figure} figs/automaton-rules-empty.png
:width: 400
:align: center

Welke regels voor de volgende toestand van een cel?
:::

:::{fillintheblank} 
Het resultaat kun je beschrijven als een reeks 0-en en 1-en, 0 voor wit en 1 voor zwart,
in de volgorde van linksboven (0) naar rechtsonder (7).
Geef hier het resultaat: {blank}`01110110`

* answer list???
:::
::::

::::{tab-item} De regels

Bepaal met de onderstaande regels de toestand in rij 4, en vervolgens in rij 5.

:::{figure} figs/automaton-rules.png
:width: 400
:align: center

Regels voor de volgende toestand van een cel
:::
::::

::::{tab-item} Antwoord

Het resultaat:

:::{figure} figs/automaton-sequence-complete.png
:width: 300
:align: center

Stappen 4 en 5 ingevuld.
:::
::::
:::::

Als je de regels van de cellulaire automaat kent, kun je uit elke toestand de volgende toestand uitrekenen.
Er zijn 8 regels (maximaal): één voor elke combinatie van de toestand van een cel en die van zijn buren.
Deze 8 regels kun je uitdrukken als een reeks van 8 bits, en die reeks kun je weer zien als een getal.

De cellulaire automaat hierboven heet "regel 110". Deze automaat heeft een bijzondere eigenschap:
deze is "Turing complete", dat wil zeggen: je kunt er dezelfde berekeningen mee uitvoeren als met een willekeurige andere  computer. We hebben eerder gezegd: *rekenen is schuiven met vormen, volgens bepaalde regels*. Deze regels kunnen dus echt heel eenvoudig zijn!

De *vertaling* van een rekenprobleem naar deze cellulaire automaat is wel ingewikkeld: 
het is niet een praktische oplossing. Maar dat doet aan de fundamentele rekenkracht van deze automaat niets af.

:::{rubric} Meer weten?
:::

* https://nl.wikipedia.org/wiki/Cellulaire_automaat
* https://nl.wikipedia.org/wiki/Regel_110
* https://mathworld.wolfram.com/Rule110.html
* https://www.wolframscience.com/nks/ (Stephen Wolfram: A New Kind of Science, een heel boek over cellulaire automaten)
    * bekijk eens p. 400 en volgende: https://www.wolframscience.com/nks/p400--growth-of-plants-and-animals/


## Programma

Hieronder geven we een programma voor het uitrekenen van de opeenvolgende toestanden van een cellulaire automaat, in Python.

```{code-cell} ipython3
import random
from typing import List

rule = [0, 0, 0, 0, 0, 0, 0, 0]  # rule represented as 8 bits

size = 10     # number of cells in a state
state = []    # a list of cells
history = []  # a list of states

# create a new cellular automaton
# nr_cells: number of cells
# ones:     positions of living cells
def create_state(nr_cells: int, ones: List[ int ]) -> None:
    global size, state, history
    size = nr_cells
    state = []
    for i in range(size):
        if i in ones:
            state.append(1)
        else:
            state.append(0)
    history = [state]

# set rule-list from rule-number
def set_rule(rulenr: int) -> None:
    global rule
    for r in range(8):
        rule[r] = rulenr % 2
        rulenr = rulenr // 2

# cell-value in current state,
# with border-cells with value 0
def cell(i: int) -> int:
    if i < 0 or i >= len(state):
        return 0
    else:
        return state[i]

# compute next state
# and add to history
def step_state() -> None:
    global state, history
    next_state = []
    for i in range(size):
        cell_state = cell(i-1) * 4 + cell(i) * 2 + cell(i+1)
        next_state.append(rule[cell_state]) 
    state = next_state
    history.append(state)

def display_history() -> None:
    for row in history:
        line = ""
        for x in row:
            line = line + str(x)     
        print(line)
```

```{tip}
Je kunt deze pagina "live" maken, via het menu *raket->Live code* bovenin.
De code-cellen krijgen dan knoppen waarmee je deze uit kunt voeren.
Je kunt de code in de cellen ook aanpassen, voor eigen experimenten.
**Let op:** het opstarten van een "live book" kan even duren; 
als je dan een cel probeert uit te voeren, krijg je de melding "waiting for kernel".
```

Met "Live code" kun je de code op deze pagina aanpassen en opnieuw uitvoeren. Voer eerst de code-cel hierboven uit; daarna kun je de begintoestand of de regel hieronder aanpassen en de cellulaire automaat een aantal stappen laten uitvoeren.

```{code-cell} ipython3
create_state(40, [20,22,24])
set_rule(110)
for i in range(20):
    step_state()
display_history()
```