(events-en-signalen)=
# Events en signalen

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

- knoppen: button_a.was_pressed(), button_b.was_pressed(),
- (versnellingsmeter) gebaren (gestures): SHAKE (en een aantal andere)
- microfoon (alleen V2): twee niveau’s detectie,

**Opmerking:** in tegenstelling tot de MakeCode versie is er geen event voor het tegelijk indrukken van beide buttons. Dit betekent dat we voor de operatie (optelling e.d.) een ander event moeten kiezen, bijv. schudden.

## Signalen

**Een signaal heeft op elk moment een waarde.** Voorbeelden van signalen: de output van een temperatuursensor; van een accelerometer; van een digitale input-port.

Een signaal kan analoog zijn of digitaal. Een digitaal signaal heeft *discrete waarden*, op *discrete tijdstippen*. In principe heeft het signaal ook een waarde op de tussenliggende tijdstippen; deze waarden kun je bepalen (als dat nodig is) door *interpolatie*.






