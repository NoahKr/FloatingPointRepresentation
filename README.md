# Stappenplan floating point represenatie tabel ding

## 1. Initialiseren van de tabel

Je krijgt de volgende gegevens altijd gegeven:
* Base - Het getalstelsel dat je gebruikt (decimaal, binair, hexagonaal, octaal, etc.)
* P-digits - Het aantal significante cijfers.
* emin - De lowerbound van de exponentenrange
* emax - De upperbound van de exponentenrange

### 1a. De x-as (bovenkant van de tabel) vul je met alle waardes van emin t/m emax.

### 1b. De y-as vul je in met alle mogelijkheden van je base in combinatie met je P-digits.

De Base beschrijft alle mogelijke getallen die je hebt. Bij een base van 10 (decimaal) zijn alle mogelijke getallen 0-9 en bij een base van 2 (binair) zijn de mogelijke getallen 0-1. Bij een base van 3 zijn de mogelijke getallen 0-2.

De P-digits beschrijft het aantal significante cijfers, simpel gezegd zijn dat het aantal getallen dat laten zien wordt. Het getal 1 zou met P-digits 3 gerepresenteerd worden als 1,00, met P-digits 2 als 1,0 en als P-digits 1 als 1.

Het aantal mogelijkheden bepaal je dan door elk getal in de getalreeks in te vullen voor elk plekje van de P-digits zonder duplicaten. Dat klinkt ingewikkeld maar het is bij Base 2 en P-digits 2 dit:
```
0.0
0.1
1.0
1.1
```
Bij Base 2 zijn de enige mogelijke getalen 0 en 1. En je hebt 2 plekken om ze in te vullen.

En bij Base 10 en P-digits 1 is het dit:
```
0
1
2
3
4
5
6
7
8
9
```

Want je hebt bij base 10 de getallen 0-9 tot je beschikking en 1 plek om ze in te vullen.

#### Rekening houden met `normalized`
Als een representatie normalized moet zijn dan houdt dat in dat getallen waar het getal voor de komma 0 of lager is niet meegerekend wordt.

De bovenstaande twee voorbeelden zouden dan dus repectievelijk 
```
1.0
1.1
```
en 
```
1
2
3
4
5
6
7
8
9
```
worden.

### 2. Het invullen van de tabel
Voor elk van de hokjes moet je de volgende stappen ondernemen:

#### 2a. het omrekenen van het getal op de y-as naar decimaal
Het omrekenen van alle getallen naar decimaal doe je door de volgende formule te hanteren voor elk apart cijfer in een getal en het resultaat bij elkaar op te tellen:
```x * ß^i```

* `x` staat hier voor het getal dat je wilt omrekenen.
* `ß` staat hier voor de base waarvanuit je rekent (dus als vanuit binair, dan is je base 2)
* `i` staat hier voor de index/positie van het getal.

Als je het volgende decimale getal hebt (let op van decimaal omrekenen naar decimaal blijft uiteraard hetzelfde getal. Dit is enkel ter voorbeeld):
```
65536
```
Als je de indici van deze getallen erboven zou zetten dan zou het er als volgt uit zien:
```
43210
65536
```
De indici beginnen dus van het meest rechtse getal af bij 0 en tellen daarvanuit naar links op.

Als we het getal dan uitrekenen door de formule in te vullen voor elk getal dan krijgen we de volgende berekening:
* Cijfer 6: `x * ß^i` = `6 * ß^i` = `6 * 10^i` = `6 * 10^4` = 60000
* Cijfer 5: `5 * 10^3` = 5000
* Cijfer 5: `5 * 10^2` = 500
* Cijfer 3: `5 * 10^1` = 30
* Cijfer 5: `6 * 10^0` = 6

Als we alle optellen: `60000 + 5000 + 500 + 30 + 6` = 65536

Het klopt dus.

Als we nu hetzelfde trucje bij een binair getal hanteren:
```
10101
```

(Ik heb hier het opschrijven van de indici overgeslagen)
* Cijfer 1: `x * ß^i` = `1 * ß^i` = `1 * 2^i` = `1 * 2^4` = 16
* Cijfer 0: `0 * 2^3` = 0
* Cijfer 1: `1 * 2^2` = 4
* Cijfer 0: `0 * 2^1` = 0
* Cijfer 1: `1 * 2^0` = 1

Het totaal is dan `16 + 0 + 4 + 0 + 1` = 21.

##### Floating Point Numbers
Kommagetallen werken bijna hetzelfde, het enige verschil is dat er naast indici van 0-n ook nog negatieve indici zijn. Die beginnen bij het eerste getal achter de komma met -1 en tellen vanaf daar naar rechts af.

Stel we hebben het volgende getal:
```
72144.256
```

Als we er dan de indici bovenzetten zou het er als volgt uitzien:
```
4 3 2 1 0 -1 -2 -3
7 2 1 4 4. 2  5  6
```

##### Putting it all together
Als we dan alles samenvoegen om het volgende binaire getal om te rekenen naar decimaal:
```
11001.101
```
* Cijfer 1: `x * ß^i` = `1 * ß^i` = `1 * 2^i` = `1 * 2^4` = 16
* Cijfer 1: `0 * 2^3` = 8
* Cijfer 0: `1 * 2^2` = 0
* Cijfer 0: `0 * 2^1` = 0
* Cijfer 1: `1 * 2^0` = 1
* Cijfer 1: `1 * 2^-1` = 0.5
* Cijfer 0: `0 * 2^-2` = 0
* Cijfer 1: `1 * 2^-3` = 0.125

Alles opgeteld is dan: `16 + 8 + 0 + 0 +1 + 0.5 + 0 + 0.125` = 25.625

#### 2b. Het uitrekenen van de eindwaarde (dat in het vakje/hokje moet)
Om de eindwaarde te berekenen moet de volgende formule hanteren:
```d.ddd... *  ß^e```

* `d.ddd...` staat hier voor het naar-decimaal-omgerekende getal
* `ß` staat hier voor de base waarvanuit je rekent (dus bij binair 2 bijvoorbeeld)
* `e` staat hier voor de exponent, dat is de waarde in de kolom op de x-as die hoort bij het hokje dat je wilt invullen.

Dus stel je wilt het hokje bereken waarvan de y-as waarde `1.01` is en de x-as waarde `-2` en je gegeven Base is 2.
Dan moet je, zoals bij 2a beschreven, eerst het getal omrekenen naar decimaal.

Dat is in dit geval: `1,25`

Vervolgens kan je de formule invullen:
`d.ddd... *  ß^e` = `1,25 *  ß^e` = `1,25 *  2^e` = `1,25 *  2^-2` = `1,25 * 0.25` = 0.3125

Doe dat voor alle hokjes en je bent er.
Succes!

### Notities
* Je kunt stap 2a., het omrekenen naar decimaal, ook overslaan en dat pas op het eind doen maar dat maakt de berekening naar mijn mening significant lastiger.
* Als je 2 hokjes van een kolom hebt berekend dan kun je waarschijnlijk een patroon herkennen en de andere getallen gewoon 'doortrekken'.


