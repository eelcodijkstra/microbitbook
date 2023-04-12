(events-en-signalen)=
# Events en signalen

Bij Physical Computing hebben we te maken met verschillende soorten inputs: signalen en events.

* een signaal heeft op elk moment een waarde;
* een event is een gebeurtenis die plaatvindt op een *ondeeelbaar ogenblik*.

Voor de drukknoppen (buttons) zijn er verschillende functies:

* de functie `button.is_pressed()` geeft het signaal van de drukknop weer: is de drukknop op dat moment ingedrukt?
* de functie `button.was_pressed()` geeft de drukknop-events weer: is de drukknop tussen dit moment en de vorige aanroep ingedrukt geweest?

In de voorbeeld-programma's gebruiken we meestal de event-functies, in de event-loop (`while True: ...`).
In het bijzonder gebruiken we deze events als inputs bij de overgangen van een eindige automaat.

Soms is het nodig om zelf de events in een signaal te detecteren; de geven hieronder daarvan enkele voorbeelden.

## Events

Een event is een gebeurtenis die plaatsvindt op een *ondeelbaar ogenblik*. 

Fysisch gezien heeft een event altijd een bepaalde duur, maar voor de software is die duur niet van belang: in de software *abstraheren* we van deze duur, anders gezegd: die duur heeft geen betekenis voor de software, en daarom laten we die weg.

Als de duur voor de software wel van belang is, bijvoorbeeld als je wilt weten hoe lang de knop ingedrukt is, dan kun je werken met een begin- en een eind-event: het indrukken van de knop en het loslaten van de knop.

Het indrukken van een knop begint met de *overgang* van “hoog” (niet ingedrukt) naar “laag” (ingedrukt) van het knop-signaal. microbit-Python detecteert deze overgang als  een *event*. 

`button.is_pressed()` geeft de *actuele toestand* van de knop weer, ofwel het *signaal* van de knop.

---

- er zijn allerlei functies waarmee je events kunt detecteren, bijvoorbeeld: `was_gesture(face_up)` of `was_gesture(face_down)`. Zie [https://microbit-micropython.readthedocs.io/en/v2-docs/tutorials/gestures.html](https://microbit-micropython.readthedocs.io/en/v2-docs/tutorials/gestures.html).
- indrukken van 1 of beide buttons; gestures (zie: xxx); V2: microphone.was_event(loud) of microphone.was_event(quiet).
- er is allerlei uitvoer mogelijk, naast tekst of figuren op het display, ook geluid, of het aansturen van de pinnen (bijvoorbeeld om een motor aan te sturen). Of, als een radio-bericht ontvangen is.
- We kunnen met behulp van timers ook onze eigen “events” maken.

Andere events

- knoppen: `button_a.was_pressed()`, `button_b.was_pressed()`,
- (versnellingsmeter) gebaren (gestures): SHAKE (en een aantal andere)
- microfoon (alleen V2): twee niveau’s detectie,

**Opmerking:** in tegenstelling tot de MakeCode versie is er geen event voor het tegelijk indrukken van beide buttons. Dit betekent dat we voor de operatie (optelling e.d.) een ander event moeten kiezen, bijv. schudden.

## Signalen

**Een signaal heeft op elk moment een waarde.** Voorbeelden van signalen: de output van een temperatuursensor; van een accelerometer; van een digitale input-port.

Een signaal kan analoog zijn of digitaal. Een digitaal signaal heeft *discrete waarden*, op *discrete tijdstippen*. In principe heeft het signaal ook een waarde op de tussenliggende tijdstippen; deze waarden kun je bepalen (als dat nodig is) door *interpolatie*.

Het volgende programma zet het input-signaal van een drukknop (button) om in directe besturing van het display (LEDs): als je knop A indrukt, brandt het display; als je deze knop weer loslaat, gaat het display weer uit.

```Python
from microbit import *

while True:
    if button_a.is_pressed():
        display.show(Images.HEART)
    else:
        display.clear()
```

Voor het aansturen van een LED via een pin is dit nog eenvoudiger:

```Python
from microbit import *

while True:
    pin0.write_digital( button_a.is_pressed() )
```

(Mogelijk moeten we de typering nog aanpassen.) 

In dit voorbeeld zie je dat de waarde van de output direct overeenkomt met de waarde van de input: zodra de input verandert, verandert de output ook. Dit is een voorbeeld van een directe signaal-koppeling. (In dit geval is er geen sprake van events en toestanden.)

(Een ander voorbeeld hiervan zie je bijvoorbeeld bij de snelheidsregeling met een potmeter.)

## Van signaal naar event

Een event als "button pressed" of "shake" is het resultaat van het verwerken van het signaal van de betreffende sensor: je moet de event *detecteren* in het signaal.

### Overgangdetectie

Voor het indrukken van een knop is het detecteren van de event eenvoudig: je herkent deze aan de overgang van "niet ingedrukt" (signaal is laag) naar "ingedrukt" (signaal is hoog). Anders gezegd: als de huidige waarde van het signaal "laag" is, en de vorige waarde was "hoog", dan detecteer je de event "button pressed".

De microbit heeft twee soorten input-functies, voor signalen en voor events:

* `button_a.is_pressed()` - geeft de huidige signaalwaarde van de button
* `button_a.was_pressed()` - geeft aan of er een "pressed" event voor de button gedetecteerd is - tussen de vorige aanroep van `was_pressed` en de huidige aanroep.

Voor het signaal van de accelerometer heb je de functie `accelerometer.get_values()` voor de actuele signaalwaarden; en `accelerometer.get_gestures()` voor de gedetecteerde events tussen de vorige en de huidige aanroep.

Voor het signaal van de microfoon heb je de functie `microphone.sound_level()`; voor de gedetecteerde events tussen de vorige en de huidige aanroep heb je `microphone.get_events()`.

> Opmerking: er kunnen meerdere events optreden tussen twee aanroepen van `was_gesture('shake')`. Als je een analyse wilt doen op basis van de verschillende gestures, dan wil je eigenlijk dat `was_gesture('shake')` alleen de shake-events uit de geschiedenis wist, en niet de andere gestures. Maar werkt dat ook zo? Dat kan ik niet uit de beschrijving halen.

```Python
from microbit import *

presses = 0  # history of H->L transitions
prev_signal = 0

# event detected?
def button_was_pressed():
    prev_presses = presses
    presses = 0   # clear history
    return prev_presses > 0

while True:
    # detect button event:
    button_signal = button_a.is_pressed()  
    if button_signal and not prev_signal:
        presses = presses + 1  # H->L transition detected
    prev_signal = button_signal
    
    if button_was_pressed():
        pass   # handle button event

```

(*We moeten de naamgeving nog aanpassen, zo nog niet erg duidelijk.*)

(De microbit houdt hiervoor een teller bij, voor het aantal malen dat een knop ingedrukt wordt tussen twee aanroepen van `button.was_pressed()`. We kunnen bovenstaande ook aanpassen voor die teller.)


> In de praktijk is dit nog iets ingewikkelder, omdat het signaal bij het indrukken van een knop niet altijd stabiel is: het kan even snel veranderen tussen hoog en laag en weer terug, de zogenaamde *contact dender*. Dit heeft ermee te maken dat een schakelaar een niet-perfect mechanisch instrument is. Zie: Wikipedia contact dender.


Voor andere events, zoals "shake", heb je meer signaalwaarden nodig om de event te kunnen detecteren.

### Drempelwaarden

In het geval van een (tweewaardig) digitaal signaal zoals dat van de drukknop is het verschil tussen "hoog" en "laag" duidelijk. Maar als je met een meerwaardig ("analoog") signaal te maken hebt, zoals het signaal van de accelerometer, dan moet je de drempelwaarden geven waaronder je dit "laag" noemt, en waarboven je dit "hoog" noemt.

> Ook in de digitale electronics moet je vastleggen wat je *laag* en *hoog* noemt. Zo zijn de drempelwaarden voor de 5V TTL-schakelingen: een input onder 0.8V wordt gezien als *laag* (of 0); en boven 2.0V wordt gezien als *hoog* (of 1). De waarden daartussen zijn niet gedefinieerd.

### Piekdetectie

Een wat ingewikkelder event in een signaal is een *piek*. Voor bijvoorbeeld het detecteren van een hartslag in een ECG-signaal, of van een stap in het signaal van een accelerometer, moet je deze pieken kunnen detecteren.

Een piek kenmerkt zich door de opeenvolging "lhl", waarbij de waarde "h" hoger is dan beide waarden "l".

Merk op dat dit een lokale piek kan zijn: als je een piek beter wilt kunnen detectere, moet je meer dan 3 waarden vergelijken.

### Ruis

Een analoog signaal bevat altijd enige ruis: de waarde die je meet (in een sensor, en bij een Analoog/Digitaal omzetting) is nooit precies, maar fluctueert ook als de eigenlijke fysieke input stabiel is.

Het effect van deze ruis kun je verminderen door te werken met het gemiddelde van een aantal waarden, bijvoorbeeld het gemiddelde van de laatste drie meetwaarden.

(Voorbeeld-code van deze middeling; deze verschuift met de metingen mee: je hebt op elk meet-moment een gemiddelde van de laatste 3 metingen. Dit kun je zien als een "laag-doorlaatfilter".)

