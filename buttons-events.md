# Buttons en events

::::{grid}

:::{grid-item-card} Voorkennis
:class-header: bg-light
header
^^^

* `display.show`

+++
footer
:::

:::{grid-item-card} Concepten
:class-body: bg-light

* button
* event, event-handler, event-loop
* if-statement, voorwaarde

:::

::::

In deze les maak je kennis met *events*, zoals het indrukken van een knop. Aan een event kun je een actie koppelen: de *event-handler*.

:::{admonition} Event
Een **event** is een gebeurtenis dit plaatsvindt op een ondeelbaar moment. Alleen op dat moment heeft de event een waarde (zie {ref}`events-en-signalen`). Voorbeelden: het indrukken van een knop; het ontvangen van een radio-bericht; het schudden van de microbit. 
:::

Gebruik als voorbeeld het onderstaande programma

```python
from microbit import *

while True:
    if button_a.was_pressed():
        display.show(Image.HAPPY)
    if button_b.was_pressed():
        display.show(Image.SAD)

```

**Wat verwacht je** dat dit programma doet? Probeer dit te beantwoorden door het programma te lezen. Straks controleer je dat door het programma uit te voeren.

**Voer het programma uit** 

Maak een nieuwe tab aan in de editor("New"). Kopieer het programma naar die lege tab, en bewaar dit ("Save") als "events.py". Laad het programma naar de microbit ("Flash"), en test het door de buttons A en B in te drukken. Klopt je verwachting?

**Uitleg** bij dit programma:

* De aanroep `button_a.was_pressed()` geeft aan of `button_a` sinds de vorige aanroep is ingedrukt. Zie: [button](https://microbit-micropython.readthedocs.io/en/v2-docs/button.html). Deze functie heeft geen parameters: er staan geen waarden tussen de haakjes `()` van de functie-aanroep.
* Met het *if-statement* koppel je het detecteren van een event aan een actie.

Het **if-statement (conditionele statement)**

```python
if <voorwaarde>:
    <opdracht>
```

betekent dat als de `voorwaarde` op dat punt in de verwerking van het programma `True` is, de `opdracht` uitgevoerd wordt.

:::{Admonition} Let op!
:class: warning
Om aan te geven dat de opdracht onderdeel is het van if-statement, staat deze 4 spaties ingesprongen. *Deze witruimte heeft betekenis in Python*: als je die niet goed gebruikt, kan je programma iets anders doen dan je bedoelt. Overigens helpt de editor bij het juiste aantal spaties.
:::

**Event loop** We hebben eerder gezien dat `while True` de “eindeloze” herhaling is van het besturingsprogramma, waarin de inputs gelezen worden en omgezet worden in acties. De vorm

```python
while True:
    if event_x():
        handle_x()
    if event_y():
        handle_y()
    if event_z():
        handle_z()
```

is een voorbeeld van een *event loop*: in de eindeloos herhalende lus ga je voor elke relevante event na of deze heeft plaatsgevonden; zo ja, dan zorg je ervoor dat de bijbehorende *event handler* uitgevoerd wordt.

**Variaties**

- draai de rollen van de knoppen A en B om: A geeft “sad”, B geeft “happy”.
- zorg ervoor dat als de microbit geschud wordt, de figuur `Image.CONFUSED` getoond wordt.
    - voeg een if-statement toe met een test voor de “schudden” event;
    - event-detectie: `accelerometer.was_gesture('shake') (zie: [gesture](https://microbit-micropython.readthedocs.io/en/v2-docs/accelerometer.html#microbit.accelerometer.was_gesture))
- zoek in de documentatie nog een andere event of gebaar, en voeg daarvoor een test en een actie toe.

Je kunt deze events en event-handlers in veel toepassingen gebruiken, zoals je in de volgende opdrachten zult zien.

