(cellulaire-automaten)=
# Cellulaire automaten

:::{margin}
Conway's Game of Life is een voorbeeld van een twee-dimensionale cellulaire automaat.
Je hebt dan een vlak met cellen (witte en zwarte hokjes).
:::

Een 1-dimensionale cellulaire automaat bestaat uit een reeks aaneengesloten cellen 
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



