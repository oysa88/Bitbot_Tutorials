# BitBot Avansert - Tutorial

### @diffs true

## Steg 1 @unplugged

### Avansert Bitbot som styres med helningsvinkel i to retninger

I denne veiledningen skal dere lage et program som skal brukes på to micro:biter: En som skal brukes som fjernkontroll og en som skal styre bil:bit-bilen.

**Følg instruksjonen videre for å løse oppgaven.**

![microbit-bitbot-radio-800px.jpg](https://i.postimg.cc/hG6BTbSZ/microbit-bitbot-radio-800px.jpg)


## Steg 2 @unplugged

### Lage fjernkontroll for å styre bit:BitBot

Vi skal starte å lage fjernkontrollen som skal styre bilen. Vi skal bruke helningsvinkelene til en micro:bit for å justere farten og svingingen til bilen. Dvs. at jo mer vi heller micro:biten fremover/bakover, jo mer fart skal bilen ha, og jo mer vi heller microbiten til høyre/venstre, jo mer vil bilen svinge.

![microbit-pitch-roll-500px.png](https://i.postimg.cc/Lsp2hNks/microbit-pitch-roll-500px.png)

## Steg 3

### Sette opp egen radiogruppe

Inni ``||basic: ved start||``: Sett opp egen ``||radio: radiogruppe||`` slik at fjernkontroll og bil kan snakke sammen.

*(Du får utdelt hvilken ``||radio: radiogruppe||`` du skal bruke fra læreren.)*

```blocks
radio.setGroup()
```

## Steg 4

### Funksjonen "Fjernkontroll"

Gå til Avansert, under ``||functions: Funksjoner||``, lag en ny funksjon du kaller "Fjernkontroll". 

Inni her skal vi sett inn to nye ``||variables: variabler||``:
-  Sett ``||variables: Pitch||`` til å lese av ``||input: helningsvinkel forover-bakover||`` 
-  Sett ``||variables: Roll||`` til å lese av ``||input: helningsvinkel høyre-venstre||``

*Kall opp ``||functions: Fjernkontroll||`` fra blokken ``||basic: gjenta for alltid||``.*

```blocks
let Pitch = 0
let Roll = 0
function Fjernkontroll () {
    Pitch = input.rotation(Rotation.Pitch)
    Roll = input.rotation(Rotation.Roll)
}
basic.forever(function () {
    Fjernkontroll()
})
```

## Steg 5

### Skru bilen Av og På

Vi skal lage en funksjon som skrur bilen AV eller PÅ hver gang ``||input:knapp A+B trykkes ||`` på fjernkontrollen.

Lag en ``||variables: variabel||`` som du kaller "AvPå": Sett ``||variables: AvPå||`` til å være 1 hvis ``||variables: AvPå||`` = 0, ellers sett ``||variables: AvPå||`` = 0.

|   Status Bil   ||||||   Verdi   |
| :------------: |||||| :------------: |
| PÅ |||||| 1 |
| AV |||||| 0 |


```blocks
let AvPå = 0
input.onButtonPressed(Button.AB, function () {
    if (AvPå == 0) {
        AvPå = 1
    } else {
        AvPå = 0
    }
})
```

## Steg 6

### Sende data til bit:bot-bilen (PAR)

Videre inni ``||functions: Fjernkontroll||`` skal vi sende radiomeldingene med informasjon fra fjernkontrollen til bit:bot-bilen.

Bruk ``||radio: radio send verdi||`` for å sende verdien til ``||variables: AvPå||`` og helningsvinkelene ``||variables: Pitch||`` og ``||variables: Roll||`` over til bilen. Gi hver av dem en bokstav, og sende med variabelen som hører til.

|   Variabel (verdi)   ||||||   Bokstav (navn)   |
| :------------: |||||| :------------: |
| Pitch |||||| P |
| AvPå |||||| A |
| Roll |||||| R |


```blocks
let Pitch = 0
let Roll = 0
function Fjernkontroll () {
    Pitch = input.rotation(Rotation.Pitch)
    Roll = input.rotation(Rotation.Roll)
    radio.sendValue("P", Pitch)
    radio.sendValue("A", AvPå)
    radio.sendValue("R", Roll)
}
basic.forever(function () {
    Fjernkontroll()
})
```

## Steg 7 @unplugged

### Programmere bit:bot-bilen

**Gratulerer!** Du er nå ferdig med den delen av koden som skal være på fjernkontrollen. 

Vi er nå klare for å lage den delen av koden som skal styre bit:bot-bilen!

![Bitbot-bil.png](https://i.postimg.cc/fWqWwHTG/Bitbot-bil.png)


## Steg 8

### Motta radio fra fjernkontrollen

Inne i blokken ``||radio: når radio mottar "name" value||``: Lag en hvis-betingelse som sjekker opp bokstavene vi sendte fra fjernkontrollen. Vi skal lage 3 nye variabler som kan huske verdiene vi sendte. Disse må koble sammen med riktig navn og verdi.

- ``||logic: Hvis||`` "name" = P, sett ``||variables: Pitch||`` til "``||variables: value||``.
- ``||logic: Hvis||`` "name" = A, sett ``||variables: AvPå||`` til "``||variables: value||``.
- ``||logic: Hvis||`` "name" = R, sett ``||variables: Roll||`` til "``||variables: value||``.


```blocks
let AvPå_Bil = 0
let Kjør = 0
let Svinge = 0
radio.onReceivedValue(function (name, value) {
    if (name == "P") {
        Kjør = value
    } else if (name == "A") {
        AvPå_Bil = value
    } else if (name == "R") {
        Svinge = value
    }
})
```
## Steg 9

### Omregning mellom fjernkontroll og bit:bot

``||input: Helningsvinklene||`` fra fjernkontrollen går fra -45 til +45 grader, mens farten til motorene går fra +1023 til -1023.

For variablene ``||variables: Kjør||`` og ``||variables: Svinge||``, trenger vi å gjøre en omregning for å fikse dette problemet. Finn forholdstallet mellom de to tilfellene og gang det med verdien som variablene mottar.

| Helningsvinkel |||||| Kjør/Svinge |
| :------------: |||||| :---------: |
|      -45       ||||||     1023    |
|       0        ||||||      0      |
|       45       ||||||    -1023    |

```blocks
let AvPå_Bil = 0
let Kjør = 0
let Svinge = 0
radio.onReceivedValue(function (name, value) {
    if (name == "P") {
        Kjør = value * -22.7
    } else if (name == "A") {
        AvPå_Bil = value
    } else if (name == "R") {
        Svinge = value * -22.7
    }
})
```

## Steg 10

### Styre Bit:Bot i egen funksjon

Lag ``||functions: funksjonen||`` "BitBot_Bilen". Inni funksjonen: La bilen kun få lov til å kjøre hvis ``||variables: PåAv_Bil||`` er lik 1. Ellers skal vi kjøre ``||bitbot: begge motorene||`` med fart 0.

Kall opp ``||functions: BitBot_Bilen||`` fra blokken ``||basic: gjenta for alltid||``.

```blocks
function BitBot_Bilen () {
    let PåAv_Bil = 0
    if (PåAv_Bil == 1) {
    	
    } else {
        bitbot.motor(BBMotor.Both, 0)
    }
}
basic.forever(function () {
    BitBot_Bilen()
})
```

## Steg 11

### Styre bit:bot-bilen

Inni ``||functions: BitBot_Bilen||`` hvis ``||variables: PåAv_Bil||`` er lik 1:

For å kunne styre hver motor individuelt, må vi gjøre en justering av hver motor ut ifra hvor mye fjernkontrollen helles til ``||input: Høyre-Venstre||``. Vi må lage to nye variabler: ``||variables: Venstrejustering||`` og ``||variables: Høyrejustering||``, for å justere hver av motorene, og sett de til å være ``||variables: Kjør||``, minus en omregning. 

*(Se i tabellen under hvilke verdier som skal brukes for å justere venstre og høyre motor.)*

|          |||||| Venstrejustering |||||| Høyrejustering |
| :------- |||||| :--------------: |||||| :------------: |
| Regn om: ||||||      Svinge      ||||||     Svinge     |
| Fra Lav: ||||||        0         ||||||       0        |
| Fra Høy: ||||||      1023        ||||||    -1023       |
| Til Lav: ||||||      Kjør        ||||||     Kjør       |
| Til Høy: ||||||    Kjør ⋅ -1     ||||||   Kjør ⋅ -1    |


```blocks
function BitBot_Bilen () {
    let PåAv_Bil = 0
    if (PåAv_Bil == 1) {
    	Venstrejustering = Kjør - pins.map(
        Svinge,
        0,
        1023,
        Kjør,
        Kjør * -1
        )
        Høyrejustering = Kjør - pins.map(
        Svinge,
        0,
        -1023,
        Kjør,
        Kjør * -1
        )
    } else {
        bitbot.motor(BBMotor.Both, 0)
    }
}
basic.forever(function () {
    BitBot_Bilen()
})
```


## Steg 12

### Styre bit:bot-bilen

Nederst inni ``||functions: BitBot_Bilen||`` hvis ``||variables: PåAv_Bil||`` er lik 1:

- Kjør ``||bitbot: venstre motor||`` med farten ``||variables: Kjør||`` minus (-) ``||variables: Venstrejustering||``.
- Kjør ``||bitbot: høyre motor||`` med farten ``||variables: Kjør||`` minus (-) ``||variables: Høyrejustering||``.

```blocks
function BitBot_Bilen () {
    let PåAv_Bil = 0
    if (PåAv_Bil == 1) {
    	Venstrejustering = Kjør - pins.map(
        Svinge,
        0,
        1023,
        Kjør,
        Kjør * -1
        )
        Høyrejustering = Kjør - pins.map(
        Svinge,
        0,
        -1023,
        Kjør,
        Kjør * -1
        )
        bitbot.motor(BBMotor.Left, Kjør - Venstrejustering)
        bitbot.motor(BBMotor.Right, Kjør - Høyrejustering)
    } else {
        bitbot.motor(BBMotor.Both, 0)
    }
}
basic.forever(function () {
    BitBot_Bilen()
})
```

## Steg 13

### Sette opp lys på bilen

Legg til lys på BitBot-bilen slik du ønsker det!

Når du er ferdig med det, ``||math: Last ned||`` koden til begge micro:bitene, og test koden på en bit:bot for å se at den funker som den skal.

**Lykke til!**

*(Se hint for hvordan komplett kode ser ut.)*

```blocks
let Høyrejustering = 0
let Svinge = 0
let Kjør = 0
let Venstrejustering = 0
let AvPå_Bil = 0
let AvPå = 0
let Roll = 0
let Pitch = 0
radio.setGroup(1)
basic.forever(function () {
    Fjernkontroll()
    BitBot_Bilen()
})
function Fjernkontroll () {
    Pitch = input.rotation(Rotation.Pitch)
    Roll = input.rotation(Rotation.Roll)
    radio.sendValue("P", Pitch)
    radio.sendValue("A", AvPå)
    radio.sendValue("R", Roll)
}
input.onButtonPressed(Button.AB, function () {
    if (AvPå == 0) {
        AvPå = 1
    } else {
        AvPå = 0
    }
})
radio.onReceivedValue(function (name, value) {
    if (name == "P") {
        Kjør = value * -22.7
    } else if (name == "A") {
        AvPå_Bil = value
    } else if (name == "R") {
        Svinge = value * -22.7
    }
})
function BitBot_Bilen () {
    if (AvPå_Bil == 1) {
        Venstrejustering = Kjør - pins.map(
        Svinge,
        0,
        1023,
        Kjør,
        Kjør * -1
        )
        Høyrejustering = Kjør - pins.map(
        Svinge,
        0,
        -1023,
        Kjør,
        Kjør * -1
        )
        bitbot.motor(BBMotor.Left, Kjør - Venstrejustering)
        bitbot.motor(BBMotor.Right, Kjør - Høyrejustering)
    } else {
        bitbot.motor(BBMotor.Both, 0)
    }
}
```

## Steg 14

### Legger med ekstra blokker her...

```blocks
basic.pause(100)
basic.showIcon(IconNames.Heart)
bitbot.setLedColor(0xFF0000)
bitbot.setPixelColor(0, 0xFF0000)
bitbot.ledRainbow()
bitbot.ledRotate()
```

```package
bitbot=github:4tronix/bitbot#1.5.2
```