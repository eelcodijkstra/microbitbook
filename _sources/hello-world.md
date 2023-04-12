# Hello World!

Het eerste programma dat je maakt om een nieuwe programmeer-omgeving uit te proberen is meestal "Hello World!". In deze opdracht maak je een programma om deze tekst van de microbit naar de host-computer te sturen. De tekst kunt je lezen in het "REPL" venster.

::::{grid}

:::{grid-item-card}
:class-header: bg-light
**Voorkennis**
^^^

* laden van programma op microbit
* import microbit module

:::

:::{grid-item-card}
:class-header: bg-light
**Concepten**
^^^

* `display.scroll(txt)`
* Python `print` voor text output
* foutmeldingen
* foutzoeken

:::

::::

**Programma**

```
from microbit import *

print('Hello World!')
display.scroll('Hello World!')
```

**Uitleg**

`'Hello World!'` is een *string*-waarde in Python. Een string is een reeks tekens: letters, cijfers, of leestekens. Deze tekens staan tussen twee quotes (`'`) of tussen dubbele quotes (`"`).

Dit programma stuurt de tekst *Hello World!* naar de host, waar je deze via het REPL venster kunt lezen.
Voor het bekijken van de output in het REPL venster moet je de volgende stappen uitvoeren:

* laad het programma naar de microbit; het start automatisch (zoals je op het display ziet);
* klik in de editor op de "REPL" knop;
* dit onderbreekt het programma op de microbit, en start de Python commando-regel (REPL) op - op de microbit;
* herstart de microbit door (a) op de host in het REPL-venster CTRL-D in te toetsen; of (b) op de reset-knop van de microbit te drukken;
* nu komt de tekst-uitvoer van de microbit in het REPL-venster.

Dezelfde tekst is ook te lezen op het display van de microbit zelf.
De opdracht `display.scroll(txt)` toont de tekst `txt` door deze over het display te *scrollen*, in plaats van de tekens achtereenvolgens te tonen. De tekst is dan beter te lezen dan via `display.show(txt)`.
Zie de documentatie:[display.scroll](https://microbit-micropython.readthedocs.io/en/v2-docs/display.html#microbit.display.scroll)


**Variaties**

* Pas de tekst aan, zodat je bijvoorbeeld *Hallo Kees* te zien krijgt (vul voor Kees je eigen naam in).
* Verdeel de tekst "Hello World!" over twee opeenvolgende print-opdrachten. Wat krijg je nu te zien? Wat concludeer je over de print-opdracht?

:::{dropdown} Antwoord
Je ziet deze tekst verdeeld over twee regels. Elke print-opdracht wordt weergegeven op een aparte regel.
:::


## Foutzoeken

Je programma's zullen niet altijd de eerste keer zonder fouten zijn (denk ik).
Soms krijg je een foutmelding van microPython op de microbit. Deze foutmelding komt langs op het display van de microbit - maar is daar vaak slecht te lezen. Je kunt deze foutmelding in het REPL venster te zien krijgen, met de aanpak zoals hierboven: (i) klik op de editor REPL knop; (ii) herstart de microbit.

Soms krijg je geen foutmelding, maar doet het programma gewoon niet wat je verwacht. 
Je kunt problemen opsporen door op een paar handige plekken in je programma `print`-opdrachten te plaatsen. Deze kun je dan via het REPL venster bekijken.
