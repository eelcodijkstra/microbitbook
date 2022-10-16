# microbit cheat sheet

Radio

* `import radio`
* `radio.on()` - zet de radio aan
* `radio.send(s)` - stuur string `s` naar de ontvanger(s)
* `radio.receive()` - haal ontvangen bericht op als string; als er geen bericht is, is het resultaat `None`

`radio.receive()` is geen blokkerende actie; dit kun je gebruiken om te detecteren of er een bericht ontvangen is (als een event).
