# Codeslot

Je kunt het codeslot ontsluiten als je de geheime sleutel kent: deze bestaat uit een aantal 0-en en 1-en, die overeenkomen met het indrukken van knop A en knop B.

::::{grid}

:::{grid-item-card}
:class-header: bg-light
**Voorkennis**
^^^

* buttons en events
* variabelen en toekenning


:::

:::{grid-item-card} Concepten
:class-header: bg-light
header
^^^

* eindige automaat,
* toestand, toestandsovergang
* hexadecimale getalvorm

:::

::::



## Langere codes

## Hexadecimale getalvorm

:::{margin}
Je kunt in plaats van A..F ook kleine letters gebruiken, a..f.
:::

Als de code langer wordt, is het lastig om deze te onthouden als een reeks 0-en en 1-en.
Je kunt dan handig gebruik maken van de *hexadecimale getalvorm*: 
een rijtje van 4 bits komt overeen met een hexadecimaal "cijfer": een cijfer (0..9) of een letter (A..F).

In de onderstaande tabel zie je de binaire vorm, de hexadecimale vorm, en de decimale vorm van een rijtje van 4 bits.

| binaire vorm | hexadecimale vorm | decimale vorm |
| :---:        | :---:             | :---:         |
| 0000         | 0                 | 0             |
| 0001         | 1                 | 1             |
| 0010         | 2                 | 2             |
| 0011         | 3                 | 3             |
| 0100         | 4                 | 4             |
| 0101         | 5                 | 5             |
| 0110         | 6                 | 6             |
| 0111         | 7                 | 7             |
| 1000         | 8                 | 8             |
| 1001         | 9                 | 9             |
| 1010         | A                 | 10            |
| 1011         | B                 | 11            |
| 1100         | C                 | 12            |
| 1101         | D                 | 13            |
| 1110         | E                 | 14            |
| 1111         | F                 | 15            |

Het hexadecimale getal A9 staat dan voor het bitpatroon 1010 1001.
Als je dit als getal op zou vatten heeft dit de waarde 169 (decimaal),
maar vaak beschouw je dit gewoon als rij bits, zoals voor het codeslot,
en doet de getalwaarde er niet toe.

