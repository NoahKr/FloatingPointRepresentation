# Stappenplan floating point represenatie tabel ding:

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
