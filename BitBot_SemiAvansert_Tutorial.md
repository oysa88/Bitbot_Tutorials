# BitBot SemiAvansert - Tutorial

## Steg 1 @unplugged

### SemiAvansert Bitbot med hastighetsjustering

I denne oppgaven skal dere lage et program som skal brukes på to micro:biter: En som skal brukes som fjernkontroll og en som skal styre bil:bit-bilen.

Følg instruksjonen videre for å løse oppgaven.

![microbit-bitbot-radio-800px.jpg](https://i.postimg.cc/hG6BTbSZ/microbit-bitbot-radio-800px.jpg)


## Steg 2 @unplugged

### Lage fjernkontroll for å styre bit:BitBot

Vi skal starte å lage fjernkontrollen som skal styre bilen. Vi skal bruke helningsvinkelen til micro:biten som gasspedal for bilen. Dvs. at jo mer vi heller micro:biten, jo mer fart skal bilen ha. Vi skal også bruke knapp A og B for å svinge bit:bot.

![microbit-pitch.png](https://i.postimg.cc/prxFZftV/microbit-pitch.png)


## Steg 3

### Sette opp egen radiogruppe

Inni ``||basic: ved start||``: Sett opp egen radiogruppe``||radio: radio sett gruppe||`` slik at fjernkontroll og bil kan snakke sammen.

```blocks
radio.setGroup(1)
```

## Steg 4

### Funksjonen "Fjernkontroll"

Gå til Avansert, under ``||functions: Funksjoner||``, lag en ny funksjon du kaller "Fjernkontroll". 

Inni her skal vi lag en ny ``||variables: variabel||`` som skal brukes til å huske hva ``||input: helningsvinkel fremover-bakover||`` er. 

- Kall ``||variables: variabelen||`` "Hastighet" og sett den til å lese hva ``||input: helningsvinkel fremover-bakover||`` er. 
- Kall opp ``||functions: Fjernkontroll||`` fra blokken ``||basic: gjenta for alltid||``.

```blocks
let Hastighet = 0
function Fjernkontroll () {
    Hastighet = input.rotation(Rotation.Pitch)
}
basic.forever(function () {
    Fjernkontroll()
})
```