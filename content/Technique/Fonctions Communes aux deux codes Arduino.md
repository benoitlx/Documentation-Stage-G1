---
date: 2024-02-05
aliases: 
tags: doc, arduino
draft: false
---

Cette page de documentation est là pour expliquer les différentes fonctions qu'utilisent les code [[ESR_MWRF]] et [[RABI_MWRF]], il est préférable d'avoir vu les [[Prérequis pour la compréhension des codes Arduino]] afin de comprendre cette page.


## Entrées - sorties - interfaçage avec les modules

### ADC Arduino

Les codes en question utilisent des techniques d'[oversampling](https://en.wikipedia.org/wiki/Oversampling) afin d'améliorer la résolution de l'ADC.

Pour mettre en oeuvre cette technique il est nécessaire d'augmenter la fréquence d’échantillonnage de l'ADC de l'arduino. Ceci se fait au moyen du code suivant:
```c
//32 MHz (reading time tested with analogRead(A0) is about 30us)
sbi(ADCSRA,ADPS2);    // set bit 1 to ADPS2
cbi(ADCSRA,ADPS1);    // set bit 0 to ADPS1 
sbi(ADCSRA,ADPS0);    // set bit 1 to ADPS0
```
> ESR_MWRF ligne 126
> RABI_MWRF ligne 129

où `ADCSRA` représente l'adresse d'un registre servant à contrôler l'ADC, et `ADPS[0/1/2]` sont des *prescaler* (des bits particulier dans le dit registre) permettant de contrôler les subdivisions de la fréquence d’échantillonnage, selon le tableau indiqué page 219 du datasheet.

La lecture se fait à l'aide de la fonction `ReadA` et son homologue `ReadB`. La différence entre les deux fonctions vient du fait que la première écrit le résultat de la mesure dans la variable `PDA` et l'autre dans la variable `PDB`.

Elles sont définies de la manière suivante :
```c
void ReadA(){analogRead(A0); for (int i=0; i<256; i++){PDA+=0L+analogRead(A0);}}     
```
> ESR_MWRF ligne 192
> RABI_MWRF ligne 195

A noter qu'avant chaque mesure, il est nécessaire de remettre à 0 `PDA` ou `PDB`.

### ADF4351

[[ADF4351]]

### AD9834

#todo

### Laser

Le laser est piloté par la pin D2

#### Fonction LaserAllign

> ESR_MWRF ligne 349
> ESR_MWRF ligne 648 

Cette fonction permet d'allumer la diode laser pendant quelques secondes à faible puissance afin de pouvoir aligner le laser, l'échantillon et le récepteur.

## Liaison Série

### Dans le setup

```c
Serial.begin(115200); // définition de la vitesse du port série
```

### Dans le loop

```c
if (Serial.available() > 0) {
  char comm = Serial.read();
  commands(comm); 
}
```
Ne nécessite pas grandes explication : reçoit les commandes et les interprète avec la fonction `commands` décrit juste après.

### Fonction `commands` et liste des commandes

> void commands(char comm) ligne 160 

Les commandes sont interprétés par la fonction `commands`

Liste des commandes:
- `+` Augmente la fréquence RF de `RFstep`
- `-` Diminue la fréquence RF de `RFstep`
- `1`...`4` Règle la puissance RF en `-4`.`-1`.`+2`.`+5` dbm
- `x` Met le switch des micro-ondes sur ON (je ne comprend pas à quoi sert le switch sachant qu'il y a la fonction `PowerEnable()` disponible avec l'[[ADF4351]], c'est normal ça va plus vite de manipuler le PORTD)
- `z` Met le switch des micro-ondes sur OFF
- `o` désactive la sortie de l'`ADF4351
- `p` active la sortie de l'`ADF4351
- `k` active la sortie du laser pour faire un test d'alignement
- `n` lance une nouvelle mesure (prend une quinzaine de minutes)


> [!tip]-
> Dans un futur code, il peut être intéressant de remplacer tous ces `if else` par une structure comme `switch case` (c'est fait pour)

> [!tip]-
> Il peut aussi être intéressant de remplacer les lignes :
> ```c
> Serial.print("Un message");
> ```
> par
> ```c
> Serial.print(F("Un message"))
> ```
> Cela a pour effet d'enregistrer les string dans la mémoire flash plutôt que dans la mémoire RAM, c'est pratique sur des microcontrôleur comme arduino avec une mémoire RAM limité.

## Génération d'impulsion

```c
cli(); // on désactive les interruptions
PORTD &= B11110011; 
PORTD |= B00000100;

for (long i = 0; i < cycles; i++){
	_delay_us(9.4375);
	PORTD &= B11111011;
	_delay_us(0.8750);
	PORTD |= B00001000;
	PIND = B00001000;
}
sei() // on réactive les interruptions
```
