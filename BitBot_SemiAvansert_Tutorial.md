# BitBot SemiAvansert - Tutorial

## Steg 1 @unplugged

### SemiAvansert Bitbot med hastighetsjustering 

I denne veiledningen skal dere lage et program som skal brukes på to micro:biter: En som skal brukes som fjernkontroll og en som skal styre bil:bit-bilen.

**Følg instruksjonen videre for å løse oppgaven.**

![microbit-bitbot-radio-800px.jpg](https://i.postimg.cc/hG6BTbSZ/microbit-bitbot-radio-800px.jpg)


## Steg 2 @unplugged

### Lage fjernkontroll for å styre bit:BitBot

Vi skal starte å lage fjernkontrollen som skal styre bilen. Vi skal bruke helningsvinkelen til micro:biten som gasspedal for bilen. Dvs. at jo mer vi heller micro:biten, jo mer fart skal bilen ha. Vi skal også bruke knapp A og B for å svinge bit:bot.

![microbit-pitch-500px.png](https://i.postimg.cc/136y9LqT/microbit-pitch-500px.png)


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

Inni her skal vi lag en ny ``||variables: variabel||`` som skal brukes til å huske hva ``||input: helningsvinkel forover-bakover||`` er. 

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

## Steg 5

### Sette opp kontroll av knappene @diffs true

Vi skal bruke ``||input: knapp A||`` for å få bilen til å svinge til venstre og ``||input: knapp B||`` for å svinge til høyre.

Lag en ``||variables: variabel||`` som kan huske på om du trykker på ``||input: knapp A||`` på micro:biten, f.eks: "Knapp_A". Sett variabelen til 1 hvis knappen trykkes ned, ellers la variabelen være 0. Gjør det samme for ``||input: knapp B||``.

```blocks
let Hastighet = 0
let Knapp_A = 0
let Knapp_B = 0
function Fjernkontroll() {
    Hastighet = input.rotation(Rotation.Pitch)
    if (input.buttonIsPressed(Button.A)) {
        Knapp_A = 1
    } else {
        Knapp_A = 0
    }
    if (input.buttonIsPressed(Button.B)) {
        Knapp_B = 1
    } else {
        Knapp_B = 0
    }
}
basic.forever(function () {
    Fjernkontroll()
})

```

## Steg 6

### Skru bilen Av og På

Vi skal lage en funksjon som skrur bilen AV eller PÅ hver gang du ``||input: rister||`` på fjernkontrollen.

Lag en ``||variables: variabel||`` som du kaller "PåAv": Sett ``||variables: PåAv||`` til å være 1 hvis ``||variables: PåAv||`` = 0, ellers sett ``||variables: PåAv||`` = 0.

|   Status Bil   ||||||   Verdi   |
| :------------: |||||| :------------: |
| PÅ |||||| 1 |
| AV |||||| 0 |


```blocks
let PåAv = 0
input.onGesture(Gesture.Shake, function () {
    if (PåAv == 0) {
        PåAv = 1
    } else {
        PåAv = 0
    }
})
```

## Steg 7

### Sende data til bit:bot-bilen

Lag en ny ``||functions: Funksjon||`` du kaller "SendData". Inni her skal vi sende radiomeldingene med informasjon fra fjernkontrollen til bit:bot-bilen.

Bruk ``||radio: radio send verdi||`` for å sende verdien til AV/PÅ, Knapp A og B, og helningsvinkelen over til bilen. Gi hver av dem en bokstav, og sende med variabelen som hører til.

|   Variabel (verdi)   ||||||   Bokstav (navn)   |
| :------------: |||||| :------------: |
| PåAv |||||| P |
| Knapp_A |||||| A |
| Knapp_B |||||| B |
| Hastighet |||||| H |

Kall opp ``||functions: SendData||`` fra blokken ``||basic: gjenta for alltid||``.

```blocks
function SendData () {
    let Hastighet = 0
    let Knapp_B = 0
    let Knapp_A = 0
    let PåAv = 0
    radio.sendValue("P", PåAv)
    radio.sendValue("A", Knapp_A)
    radio.sendValue("B", Knapp_B)
    radio.sendValue("H", Hastighet)
}
basic.forever(function () {
    SendData()
})
```

## Steg 8 @unplugged

### Programmere bit:bot-bilen

**Gratulerer!** Du er nå ferdig med den delen av koden som skal være på fjernkontrollen. 

Vi er nå klare for å lage den delen av koden som skal styre bit:bot-bilen!

![Bitbot-bil.png](https://i.postimg.cc/fWqWwHTG/Bitbot-bil.png)


## Steg 9

### Motta radio fra fjernkontrollen

Inne i blokken ``||radio: når radio mottar "name" value||``: Lag en hvis-betingelse som sjekker opp bokstavene vi sendte fra fjernkontrollen. Vi skal lage 4 nye variabler som kan huske verdiene vi sendte. Disse må koble sammen med riktig navn og verdi.

- ``||logic: Hvis||`` "name" = P, sett ``||variables: PåAv_Bil||`` til "``||variables: value||``.
- ``||logic: Hvis||`` "name" = A, sett ``||variables: Venstre||`` til "``||variables: value||``.
- ``||logic: Hvis||`` "name" = B, sett ``||variables: Høyre||`` til "``||variables: value||``.
- ``||logic: Hvis||`` "name" = H, sett ``||variables: Kjør||`` til "``||variables: value||``.

```blocks
let PåAv_Bil = 0
let Venstre = 0
let Kjør = 0
radio.onReceivedValue(function (name, value) {
    if (name == "P") {
        PåAv_Bil = value
    } else if (name == "A") {
        Venstre = value
    } else if (name == "B") {
        Venstre = value
    } else if (name == "H") {
        Kjør = value
    }
})
```
## Steg 10

### Omregning mellom helningsvinkel og hastighet @diffs true

For variabelen ``||variables: Kjør||`` som mottar verdien til helningsvinkelen til fjernkontrollen, trenger vi å gjøre en omregning. 

Verdien til ``||variables: Kjør||`` som vi mottar går fra -45 til +45, mens farten til motorene går fra +1023 til -1023. 

Finn ut hvilket tall du må gange ``||variables: value||`` med for at hastigheten til bilen skal bli riktig.

|   Helningsvinkel   ||||||   Hastighet   |
| :------------: |||||| :------------: |
| -45 |||||| 1023 |
| 0 |||||| 0 |
| 45 |||||| -1023 |

```blocks
let PåAv_Bil = 0
let Venstre = 0
let Høyre = 0
let Kjør = 0
radio.onReceivedValue(function (name, value) {
    if (name == "P") {
        PåAv_Bil = value
    } else if (name == "A") {
        Venstre = value
    } else if (name == "B") {
        Høyre = value
    } else if (name == "H") {
        Kjør = value * -22.7
    }
})
```

## Steg 11

### Styre Bit:Bot i egen funksjon

Lag ``||functions: funksjonen||`` "BitBot_Bilen". Inni funksjonen: La bilen kun få lov til å kjøre hvis ``||variables: PåAv_Bil||`` er lik 1. Ellers skal vi kjøre ``||arrays: begge motorene||`` med fart 0.

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

## Steg 12

### Få bilen til å svinge @diffs true

Inni ``||functions: BitBot_Bilen||``, hvis ``||variables: PåAv_Bil||`` = 1:

- ``||logic: Hvis||`` ``||variables: Venstre||`` = 1, kjør ``||arrays: høyre motor||`` med farten vi får fra ``||variables: Kjør||``, og ``||arrays: venstre motor||`` med farten vi får fra ``||variables: Kjør||`` ``||math: * -1||``.
- ``||logic: Hvis||`` ``||variables: Høyre||`` = 1, kjør ``||arrays: venstre motor||`` med farten vi får fra ``||variables: Kjør||``, og ``||arrays: høyre motor||`` med farten vi får fra ``||variables: Kjør||`` ``||math: * -1||``.
- ``||logic: Ellers||``, kjør ``||arrays: begge motorene||`` med farten vi får fra ``||variables: Kjør||``.

```blocks
function BitBot_Bilen () {
    let PåAv_Bil = 0
    let Venstre = 0
    let Høyre = 0
    let Kjør = 0
    if (PåAv_Bil == 1) {
        if (Venstre == 1) {
            bitbot.motor(BBMotor.Right, Kjør)
            bitbot.motor(BBMotor.Left, Kjør * -1)
        }else if (Venstre == 1) {
            bitbot.motor(BBMotor.Left, Kjør)
            bitbot.motor(BBMotor.Right, Kjør * -1)
        } else {
            bitbot.motor(BBMotor.Both, Kjør)
        }
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
let PåAv = 0
let Knapp_A = 0
let Knapp_B = 0
let Hastighet = 0
let PåAv_Bil = 0
let Venstre = 0
let Høyre = 0
let Kjør = 0
radio.setGroup(1)
bitbot.ledRainbow()
basic.forever(function () {
    Fjernkontroll()
    SendInfo()
    BitBot_Bilen()
})
function Fjernkontroll() {
    Hastighet = input.rotation(Rotation.Pitch)
    if (input.buttonIsPressed(Button.A)) {
        Knapp_A = 1
    } else {
        Knapp_A = 0
    }
    if (input.buttonIsPressed(Button.B)) {
        Knapp_B = 1
    } else {
        Knapp_B = 0
    }
}
function SendInfo() {
    radio.sendValue("P", PåAv)
    radio.sendValue("A", Knapp_A)
    radio.sendValue("B", Knapp_B)
    radio.sendValue("H", Hastighet)
}
radio.onReceivedValue(function (name, value) {
    if (name == "P") {
        PåAv_Bil = value
    } else if (name == "A") {
        Venstre = value
    } else if (name == "B") {
        Høyre = value
    } else if (name == "H") {
        Kjør = value * -22.7
    }
})
function BitBot_Bilen() {
    if (PåAv_Bil == 1) {
        let Kjør = 0
        let Venstre = 0
        if (Venstre == 1) {
            bitbot.motor(BBMotor.Right, Kjør)
            bitbot.motor(BBMotor.Left, Kjør * -1)
        } else if (Venstre == 1) {
            bitbot.motor(BBMotor.Left, Kjør)
            bitbot.motor(BBMotor.Right, Kjør * -1)
        } else {
            bitbot.motor(BBMotor.Both, Kjør)
        }
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
BitBot=github:4tronix/bitbot#1.5.2
```