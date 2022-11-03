# Cellulaire automaten

Een lineaire cellulaire automaat bestaat uit een reeks aaneengesloten cellem; 
in ons geval hebben deze de waarden 0 (wit, uit, dood) of 1 (zwart, aan, levend).
De toestand van een automaat op een bepaald moment geeft aan welke cellen levend zijn, en welke dood.
Dit kun je weergeven als een (horizontale) rij zwart/witte cellen (hokjes).

:::{figure} rekenen/automaton-state.png
:width: 300
:align: center

Toestand van een lineaire cellulaire automaat
:::

Gegeven een toestand van een automaat, kun je voor elke cel de volgende toestand bepalen uit de toestand van die cel en die van zijn directe buren.
Je kunt de opeenvolging van toestanden weergeven door deze onder elkaar te tekenen, in een plat vlak.
Bekijk de volgende reeks toestanden. Kun je voorspellen welke waarden je krijgt in rijen 4 en 5?

:::{figure} figs/automaton-sequence.png
:width: 300
:align: center

Openvolging van toestanden
:::


Tip: bepaal voor *elke combinatie* van een cel met zijn directe buren, wat de volgende toestand is voor die cel.
Met andere woorden: bepaal in onderstaande figuur de waarde voor de hokjes onderaan de "T".

:::{figure} figs/automaton-rules-empty.png
:width: 300
:align: center

Welke regels voor de volgende toestand van een cel?
:::


Antwoord: 

:::{figure} figs/automaton-rules.png
:width: 300
:align: center

Regels voor de volgende toestand van een cel
:::

:::{figure} figs/automaton-sequence-complete.png
:width: 300
:align: center

Stappen 4 en 5 ingevuld.
:::



Met de microbit kunnen we een kleine cellulaire automaat simuleren.



We beschrijven eerst het gebruik van het programma, en daarna leggen we de werking uit.



Je kunt experimenteren met de verschillende regels voor het bepalen van de volgende generatie.

## Programma

* door te schudden maak je een nieuwe *seed*: de eerste generatie waaruit je het vervolg uitrekent.
* door indrukken van knop B bepaal je de volgende generatie uit de vorige.
    * (hoe weten we welke rij aan de beurt is?)
* door indrukken van knop A kun je de rijen doorschuiven?

een "generatie" (`gen`) bestaat uit een rij (list) van getallen 0 en 1, van lengte 5.

```Python

rule = (0, 0, 0, 0, 0, 0, 0, 0)

def create_field():
    row = []
    for i in range(5):
        row.append(0)
    field = []
    for i in range(5):
        field.append(row)

def set_rule(x):
    global rule
    for r in range(8):
        rule[r] = x % 2
        x = x // 2

def next_gen(gen):
    old_gen = gen
    old_gen.insert(0, 0)  # add border cells
    old_gen.insert(6, 0)
    next_gen = (0, 0, 0, 0, 0)
    for i in range(5):
        value = old_gen(i-1) * 4 + old_gen(i) * 2 + old_gen(i+1)
        next_gen[i] = rule[value]
    return next_gen    
     
def next_row(rownr):
    prev_row = (0, 0, 0, 0, 0)
    for i in range(r):
        prev_row[i] = display.get

```

