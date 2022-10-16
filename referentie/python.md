# Python - cheatsheet

We geven hier een uitleg van de verschillende Python-constructies die we gebruiken in de voorbeeld-programma.s
Per constructie geven we een uitleg van de betekenis en een aantal voorbeelden van het gebruiken.

* waarde
* expressie
* variabele
* toekenning
* functie-aanroep
* functie-definitie
* 

De **herhaling**

```python
while <voorwaarde>:
    <opdracht>
```

betekent dat zolang aan de `voorwaarde` voldaan is, (op dat punt in de verwerking van het programma), de `opdracht` uitgevoerd wordt.

De vorm

```python
while True:
    <opdracht>
```

betekent dat de `opdracht` steeds weer herhaald wordt. Het programma stopt pas als je de computer uitzet (of reset). Dit is typisch voor het programma van een besturingscomputer (microcontroller): dit stopt nooit. Het programma in (bijvoorbeeld) een magnetron of wasmachine blijft altijd actief en kan altijd invoer verwerken, zolang het apparaat aanstaat.

(variabelen)=
## Variabelen en toekenning

* waarde
* expressie (uitdrukking)
* variabele: naam gekoppeld aan een waarde
* toekenning: naam wordt gekoppeld aan een nieuwe waarde
    * vorm: `<naam> = <expressie>`
    * je werkt dit van rechts naar links (en zo lees je het ook meestal)
    * reken expressie rechts uit, dit geeft een waarde
    * ken deze waarde toe aan de variabele links
    * bijvoorbeeld: `i = 2 * x` - spreek uit: i *wordt* twee maal x
    * (het `=` teken spreek je uit als *wordt*. Dit is een wat ongelukkig gekozen symbool, in andere talen kom je ook wel `:=` tegen, of `<-`.)
    
    
## is en is not

Om te vergelijken of twee variabelen *identiek* zijn, dat wil zeggen naar hetzelfde object verwijzen, gebruik je de vergelijkingen `is` en `is not`.

Deze vergelijkingen gebruik je ook in combinatie met `None`: `if x is not None:`