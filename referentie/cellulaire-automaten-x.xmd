(cellulaire-automaten)=
# Cellulaire automaten

:::{admonition} Begrippen

* ééndimensionale cellulaire automaat, rule 110, weergave van proces
:::

Het Game of Life is een voorbeeld van een tweedimensionale cellulaire automaat:
het spel speelt zich af in het platte vlak.
In dit gedeelte gaan we verder in op *eendimensionale cellulaire automaten*:
een toestand bestaat uit een reeks cellen op één lijn.
Je kunt de opeenvolgende toestanden dan onder elkaar plaatsen:
in het platte vlak zie je dan in één oogopslag het proces: de ontwikkeling van het algoritme in de tijd.

In een eendimensionale cellulaire automaat heeft een cel 2 buren: links en rechts.
De volgende toestand van een cel hangt af van de huidige toestand van de cel en zijn twee buren.
Voor 3 cellen heb je 8 mogelijke configuraties.
De regels van de automaat beschrijven voor elke configuratie welke kleur cel deze oplevert.

Een voorbeeld van een automaat: (rule 110 = 0110 1110)

:::{figure} Rules.png
:width: 400px
:align: center
  
Regels voor de automaat (rule 110)
:::

Met behulp van deze regels kun je vanuit een begintoestand de volgende toestanden uitrekenen.
We tekenen de opeenvolgende toestanden (of generaties) onder elkaar.
In de figuur hieronder is dan gedaan voor een begintoestand met één zwarte cel,
met de bovenstaande regels.

:::{figure} Field.png
:width: 400px
:align: center

1-dimensionele cellulaire automaat: de eerste regel is de begintoestand,
de volgende regels zijn de opeenvolgende toestanden.
:::

In plaats van met de "vormen" zwart en wit kunnen we ook met symbolen werken,
bijvoorbeeld 0 en 1.
(We kunnen allerlei symbolen gebruiken, als we maar twee waarden kunnen onderscheiden.)

In tabelvorm worden de regels dan:

:::{table} Rule 110
:align: center

| 0    | 000  | 0    |
| :--: | :--: | :--: |
| 1    | 001  | 1    |
| 2    | 010  | 1    |
| 3    | 011  | 1    |
| 4    | 100  | 0    |
| 5    | 101  | 1    |
| 6    | 110  | 1    |
| 7    | 111  | 0    |

:::

Dit voorbeeld is uitgewerkt in een `spreadsheet <https://docs.google.com/spreadsheets/d/1vW5YHQMnERHqh9mjamJiN_tSWtZc9RtfsdraQ5LsH5E/edit?usp=sharing>`_.
Deze kun je ook downloaden via :download:`download cellulaire automaten spreadsheet <Cellular-automaton.ods>`.

:::{admonition} Weergave van het proces in een spreadsheet

  In de spreadsheet staan de opeenvolgende stappen van het proces onder elkaar.
  Zo kun je de loop van het proces volgen.
  Deze aanpak zullen we ook in volgende opdrachten gebruiken.
  De eerste rij bevat de begintoestand, soms met de invoer.
  Elke volgende rij bevat de volgende stap in het algoritme - waarvan het resultaat dan in cellen van de rij zichtbaar wordt.

    Voor de spreadsheet met de cellulaire automaat hebben we ook tussenresultaten,
    namelijk een codering van het patroon van de cel en de buren;
    voor de overzichtelijkheid hebben we deze in een apart vel (sheet, tab) geplaatst.
:::

## Links en verdieping

In het Wikipedia-artikel over `Rule 110 <https://en.wikipedia.org/wiki/Rule_110>`_ vind je een beschrijving van het gedrag van deze cellulaire automaat.
Een bijzondere eigenschap is dat deze automaat *Turing-complete* is:
je kunt daarmee in principe alle mogelijke berekeningen uitvoeren.
Voor zover bekend is dit het eenvoudigste systeem waarmee dit kan.

Meer over cellulaire automaten vind je in het boek van Stephen Wolfram, A New Kind of Science.
Dit boek is online beschikbaar, via https://www.wolframscience.com.
In dit boek vind je veel fraaie figuren die ontstaan uit zulke eenvoudige regels en automaten.
Je vindt er ook figuren uit de natuur die soms verrassend sterk lijken op deze figuren,
zoals in hoofdstuk 8 (bijv. p. 423, p. 426).

## Opdrachten


1. Maak de figuur van de cellulaire automaat hierboven af,
   door de toestanden 5 tot en met 8 te berekenen met de gegeven regels.
   Controleer je uitwerking met de `spreadsheet <https://docs.google.com/spreadsheets/d/1vW5YHQMnERHqh9mjamJiN_tSWtZc9RtfsdraQ5LsH5E/edit?usp=sharing>`_.
   Om zelf met deze spreadsheet te werken, moet je eerst een kopie maken,
   via Google Drive, of via :download:`rule 110 spreadsheet <Cellular-automaton.ods>`.
   In deze spreadsheet vul je de begintoestand (eerste regel) in:
   de volgende toestanden worden dan uitgerekend.

2. Je kunt in de spreadsheet ook experimenteren met andere regels.
   In de sheet (tab) "rules" kun je het nummer van een andere regel invoeren,
   in *omgekeerde* binaire notatie.
   Verander het regelnummer van "110" in "18" (0100 1000).
   Welk effect heeft dit op het resultaat?
