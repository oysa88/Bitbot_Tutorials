# Bitbot vanlig - Tutorial

## Steg 1 @unplugged

### Bitbot med fjernkontroll 

I denne veiledningen skal dere lage et program som skal brukes på to micro:biter: En som skal brukes som fjernkontroll og en som skal styre bil:bit-bilen.

**Følg instruksjonen videre for å løse oppgaven.**

![microbit-bitbot-radio-800px.jpg](https://i.postimg.cc/hG6BTbSZ/microbit-bitbot-radio-800px.jpg)


## Steg 2 @unplugged

### Lage fjernkontroll for å styre bit:BitBot

Vi skal starte å lage fjernkontrollen som skal styre bilen. Vi skal bruke helningsvinkelen til micro:biten for å få bilen til å kjøre forover eller bakover. Også skal vi bruke knapp A og B for å svinge bit:bot.

![microbit-pitch-500px.png](https://i.postimg.cc/136y9LqT/microbit-pitch-500px.png)


## Steg 3

### Sette opp egen radiogruppe

Inni ``||basic: ved start||``: Sett opp egen ``||radio: radiogruppe||`` slik at fjernkontroll og bil kan snakke sammen.

*(Du får utdelt hvilken ``||radio: radiogruppe||`` du skal bruke fra læreren.)*

```blocks
radio.setGroup()
```

## Steg 4

### Fjernkontroll

Inni ``||basic: gjenta for alltid||`` skal vi lage en lang ``||logic: hvis-betingelse||`` som sjekker hvilken funksjon på fjernkontrollen som brukes.

Vi starter med å legge inn ``||input: Knapp A||``, som skal få bilen til å svinge til venstre, og ``||input: Knapp B||``, som skal få bilen til å svinge til høyre.

For hver funksjon skal vi bruke en ``||radio: radio send tall||`` som sender et unikt tall slik at bilen vet hvilken funksjon som brukes på fjernkontrollen.

|   Funksjon  ||||||   Radiotall   |
| :------------: |||||| :------------: |
| Knapp A |||||| 1 |
| Knapp B |||||| 2 |


```blocks
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        radio.sendNumber(1)
    } else if (input.buttonIsPressed(Button.B)) {
        radio.sendNumber(2)
    }
})
```

## Steg 5

### Fjernkontroll fortsetter

Utvid ``||logic: hvis-betingelse||`` med pluss-tegnet så vi har plass til 5 betingelser.

Vi skal nå lage funksjonen som lar bilen kjøre forover eller bakover. For å få til det, skal vi bruke blokken ``||input: helningsvinkel forover-bakover.||``.

Vinkelen til fjernkontrollen blir negativ når vi heller den forover og positiv når vi heller den bakover. Vi setter en terskelverdi på 30 grader for at bilen skal starte å kjøre.

- ``||logic: Hvis||`` ``||input: helningsvinkel forover-bakover.||`` er mindre enn (<) -30 grader, send ``||radio: radiotall = 3||`` for å la bilen kjøre forover.
- ``||logic: Hvis||`` ``||input: helningsvinkel forover-bakover.||`` er større enn (>) +30 grader, send ``||radio: radiotall = 4||`` for å la bilen kjøre bakover.

Under ``||logic: ellers||`` send ``||radio: radiotall = 0||``. *Dette gjør vi for å si ifra til bilen at ingen av funksjonene på fjernkontrollen er i bruk. Da skal bilen stå i ro.*


```blocks
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        radio.sendNumber(1)
    } else if (input.buttonIsPressed(Button.B)) {
        radio.sendNumber(2)
    } else if (input.rotation(Rotation.Pitch) < -30) {
        radio.sendNumber(3)
    } else if (input.rotation(Rotation.Pitch) > 30) {
        radio.sendNumber(4)
    } else {
        radio.sendNumber(0)
    }
})
```

## Steg 6 @unplugged

### Programmere bit:bot-bilen

**Gratulerer!** Du er nå ferdig med den delen av koden som skal være på fjernkontrollen. 

Vi er nå klare for å lage den delen av koden som skal styre bit:bot-bilen!

![Bitbot-bil.png](https://i.postimg.cc/fWqWwHTG/Bitbot-bil.png)


## Steg 7

### Motta radio fra fjernkontrollen

Inne i blokken ``||radio: når radio mottar "ReceivedNumber"||``: Lag en ``||logic: hvis-betingelse||`` som sjekker opp tallene vi sendte fra fjernkontrollen. 

- ``||logic: Hvis||`` ``||radio: ReceivedNumber = 1||``, ``||bitbot: snu til venstre med fart||`` 60%-100%.
- ``||logic: Ellers hvis||`` ``||radio: ReceivedNumber = 2||``, ``||bitbot: snu til høyre med fart||`` 60%-100%.
- ``||logic: Ellers hvis||`` ``||radio: ReceivedNumber = 3||``, ``||bitbot: kjør forover med fart||`` 60%-100%.
- ``||logic: Ellers hvis||`` ``||radio: ReceivedNumber = 4||``, ``||bitbot: kjør bakover med fart||`` 60%-100%.
- ``||logic: Ellers||``, ``||bitbot: kjør forover med fart||`` 0%.

```blocks
radio.onReceivedNumber(function (receivedNumber) {
    if (receivedNumber == 1) {
        bitbot.rotate(BBRobotDirection.Left, 60)
    } else if (receivedNumber == 1) {
        bitbot.rotate(BBRobotDirection.Right, 60)
    } else if (receivedNumber == 1) {
        bitbot.go(BBDirection.Forward, 60)
    } else if (receivedNumber == 1) {
        bitbot.go(BBDirection.Reverse, 60)
    } else {
        bitbot.go(BBDirection.Forward, 0)
    }
})
```
## Steg 8

### Sette opp lys på bilen

Legg til lys på BitBot-bilen slik du ønsker det!

Når du er ferdig med det, ``||math: Last ned||`` koden til begge micro:bitene, og test koden på en bit:bot for å se at den funker som den skal.

**Lykke til!**

*(Se hint for hvordan komplett kode ser ut.)*

```blocks
radio.setGroup(1)
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        radio.sendNumber(1)
    } else if (input.buttonIsPressed(Button.B)) {
        radio.sendNumber(2)
    } else if (input.rotation(Rotation.Pitch) < -30) {
        radio.sendNumber(3)
    } else if (input.rotation(Rotation.Pitch) > 30) {
        radio.sendNumber(4)
    } else {
        radio.sendNumber(0)
    }
})
radio.onReceivedNumber(function (receivedNumber) {
    if (receivedNumber == 1) {
        bitbot.rotate(BBRobotDirection.Left, 60)
    } else if (receivedNumber == 1) {
        bitbot.rotate(BBRobotDirection.Right, 60)
    } else if (receivedNumber == 1) {
        bitbot.go(BBDirection.Forward, 60)
    } else if (receivedNumber == 1) {
        bitbot.go(BBDirection.Reverse, 60)
    } else {
        bitbot.go(BBDirection.Forward, 0)
    }
})
```
## Steg 9

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