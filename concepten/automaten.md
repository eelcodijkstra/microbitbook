# Eindige automaten

Waarom eindige automaten? Waar eindige automaten? Zowel in de theorie (van bijv. programmeertalen) als in de praktijk wordt dit gebruikt: bijvoorbeeld bij het ontwerpen van digitale schakelingen, maar ook bij het maken van software voor "physical computing" (embedded systemen); of voor het verwerken van reguliere expressies.

Een eindige automaat bestaat uit een eindig aantal *toestanden*, en een eindig aantal *toestandsovergangen*.
Een toestand tekenen we als een bolletje.
Een toestandsovergang van toestand A naar toestand B tekenen we als een pijl van A naar B; deze overgang is gelabeld met een invoersymbool en (mogelijk) met een uitvoersymbool.

(Voor de ingewijden: we staan in dit geval geen epsilon-overgangen toe. Bovendien veronderstellen we dat er voor elke toestand voor elk invoersymbool precies 1 overgang is.)

(Een meer formele beschrijving van een eindige automaat vind je bijvoorbeeld op Wikipedia?)

In de microbit-voorbeelden is een inputsymbool altijd een *input-event*, bijvoorbeeld het indrukken van knop A, of het schudden van de microbit. Een outputsymbool is een *output-event*, meestal de besturing van het display, van een geluid, van een output-pin, o.i.d.

Als de automaat in toestand X een input A aangeboden krijgt, en deze toestand heeft een overgang (A/K->Y), dan (i) wordt outputsymbool K gegenereerd, en (ii) wordt Y de nieuwe toestand.

(In ruimere zin volgt ook "React" dit model.)

Je kunt een dergelijke automaat ook in de vorm van tabellen beschrijven:

* huidige toestand
* input
* output
* volgende toestand

(Deze tabel kan bijvoorbeeld in een read-only geheugen van een digitale schakeling opgeslagen worden.)


## Omzetten van een eindige automaat in microPython

We gaan hiervoor uit van het gebruik van events en event-handlers, zie XXX.
In een event-handler, voor een input-event, moet je dan onderscheid maken tussen de verschillende toestanden waarin deze input-event kan optreden.

1. Begin met een getekende eindige automaat;
2. Zet deze om in tabelvorm (zoals hierboven beschreven);
3. Groepeer deze tabel per input-event, in plaats van per toestand;
4. Zet de tabel-regels van eenzelfde input-event om in een IF waarin de toestanden onderscheiden worden.

Het programma moet *in elke toestand elke mogelijke invoer* kunnen verwerken - eventueel door een fout-reactie of een reset.
Dit betekent dat de automaat compleet moet zijn: elke toestand moet voor elk inputsymbool een overgang hebben.
En, omgekeerd, voor elk inputsymbool moet er voor elke toestand een overgang gedefinieerd zijn. Dit kan natuurlijk ook een overgang naar dezelfde toestand zijn. 


Het voorbeeld van de 1-knops dimmer wordt dan:

| input | toestand | output | volgende toestand |
| :---: | :---:    | :---:  | :---:             |
| A     |  0       |  L1    |  1                |
| A     |  1       |  L2    |  2                |
| A     |  2       |  L3    |  3                |
| A     |  3       |  L0    |  0                |

Het programma wordt deze tabel dan:

```Python
def A_handler():
    global state
    if state == 0:
        set_light_level(1)
        state = 1
    elif state == 1:
        set_light_level(2)
        state = 2
    elif state == 2:
        set_light_level(3)
        state = 3
    elif state == 3:
        set_light_level(0)
        state = 0
```

Voor de 2-knops dimmer wordt dit:

| input | toestand | output | volgende toestand |
| :---: | :---:    | :---:  | :---:             |
| A     |  0       |  L1    |  1                |
| A     |  1       |  L2    |  2                |
| A     |  2       |  L3    |  3                |
| A     |  3       |  L3    |  3                |
| B     |  0       |  L0    |  0                |
| B     |  1       |  L0    |  0                |
| B     |  2       |  L1    |  1                |
| B     |  3       |  L2    |  2                |

In het programma wordt dit:

```Python

def A_handler():
    global state
    if state == 0:
        set_light_level(1)
        state = 1
    elif state == 1:
        set_light_level(2)
        state = 2
    elif state == 2:
        set_light_level(3)
        state = 3
    elif state == 3:
        set_light_level(3)
        state = 3

def B_handler():
    global state
    if state == 0:
        set_light_level(0)
        state = 0
    elif state == 1:
        set_light_level(0)
        state = 0
    elif state == 2:
        set_light_level(1)
        state = 1
    elif state == 3:
        set_light_level(2)
        state = 2
```