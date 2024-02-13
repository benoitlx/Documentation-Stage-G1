---
date: 2024-02-02
aliases: 
tags: doc, arduino
draft: false
---


### Architecture de l'Atemga328p

Le datasheet est disponible [ici](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf)

![[Pasted image 20240202152148.png]]

#### Registres

Il peut être utile pour la suite de comprendre la notion de registre.

Un registre est un composant intégré dans le microcontrôleur et dans les processeurs, permettant de stocker un nombre donné de bits (le même nombre que la taille du bus, ici 8). Deux opérations peuvent être effectué par le processeur sur chacun des registres:
- store: pour écrire les donnés du bus dans le registre
- load: pour récupérer les donnés du registre et les rendre disponible sur le bus
A noter que les registres et le processeur sont tous cadencé par la même horloge, pour assurer une cohérence (pour pas que deux registres écrivent en même temps sur le bus).

Les registres sont utilisés comme moyen de stockage temporaire pour faire des calculs, comme un moyen de communiquer avec des modules externes (voir paragraphe suivant), comme un moyen de se repérer dans l'exécution d'un programme (program counter).

### Manipulation de Port

Un port est un registre (branché sur le bus), mais aussi relié à des composants externes. Ils peuvent être utilisés en tant qu'entrée ou sortie numérique.
Pour définir leur comportement (entrée ou sortie), on peut utiliser la fonction suivante de la librairie `Arduino.h` (`Arduino.h` est importé directement par l'IDE Arduino)
```c
pinMode(pin, OUTPUT/INPUT/INPUT_PULLUP);
```
ou directement en manipulant des registres de l'arduino :
```
DDR[B/C/D] = B00100000
```
Configure la 6ème pin du port B C ou D en sortie.

Pour utiliser les ports utilisés comme sortie, il suffit de faire : 
```c
PORT[B/C/D] = B00x00000
```
Avec `x` valant soit 0 soit 1 en fonction de l'état de la pin voulu. On remarque avec cette technique que l'on peut indiquer l'état de plusieurs pins d'un même port en même temps.

Si les pins mis à 1 avec `PORT[B/C/D]` était mis à 0 avec `DDR[B/C/D]`, cela aura pour effet de les configurer en `INPUT_PULLUP`.

### Interruption

Les interruption permettent de modifier l'état du program counter (l'endroit où est lut l'instruction courante), pour exécuter un code arbitraire dans la RAM et retourner au précédent endroit.

Lorsque une interruption survient, un délai est ajouté entre les différentes instructions exécutés, c'est problématique dans notre cas car on veut générer des impulsions d'un cycle d'horloge.

Pour remédier à ce problème, il faut encapsuler chaque partie de code critique dans le bloc suivant:
```c
cli(); // désactive les interruptions
// code critique
sei(); // réactive les interruptions
```

> [!attention]+
> Les fonctions manipulant le port série configurent des interruptions, sans que l'on puisse le voir dans le code source.

### Notions de C

#### Compilation

La compilation permet à partir d'un code source de produire un fichier exécutable par une machine. Dans le cas d'un microcontrôleur, il s'agit de produire un code hexadécimal qui sera téléversé dans la mémoire flash du microcontrôleur.

La compilation se fait en 4 grandes étapes:
- Le préprocesseur
- Le compilateur
- L'assembleur
- Le Linker

Le premier point est expliqué dans la partie suivante, le compilateur permet de traduire un fichier c en un code assembleur (c'est l'étape la plus compliqué), l'assembleur permet de le transformer en code machine (dans notre cas en hexadécimal), le linker permet de faire le lien entre différents fichiers compilé séparément, et d'include les fichier sources.

#### Macros

Une macro permet de manipuler le fichier source pendant l'étape *préprocesseur* de la chaîne de compilation. Tout les lignes d'un code en C commençant par un `#` vont être utilisé à ce moment là. Dans le cas d'un `#define foo bar` (c'est la seule qui nous intéresse dans notre cas), toutes les occurrences de `foo` du code source vont être remplacé par `bar` après le passage dans le *préprocesseur*

##### Macro NOP

> ESR_MWRF ligne 9
> RABI_MWRF ligne 9

La macro `NOP` permet de ne rien faire pendant un cycle d'horloge du processeur soit pendant 62.5 nanosecondes.
Elle est implémentée de la manière suivante:
```c
#define NOP __asm__ __volatile__("nop\n\t"); 
```

`__asm__` est un mot clé du language C permettant d'introduire un code assembleur dans un code C. Ici la seule instruction assembleur introduite est la `nop` qui permet de faire ce qui a été décrit précédemment. Le mot clé `__volatile__` permet quant à lui d'éviter les optimisations du compilateur. En effet, sans celui ci le compilateur supprimerait de son programme toutes les fois où l'instruction `NOP` serait appelé, car il ne peut pas comprendre l'utilité d'une telle opération.

##### Macro cbi

> ESR_MWRF ligne 21
> RABI_MWRF ligne 21

```c
#define cbi(sfr, i) (_SFR_BYTE(sfr) &= ~_BV(i)) 
```

LA macro `cbi` doit être utilisé comme une fonction : elle prend deux arguments (un octet et un entier entre 0 et 7), et met le bit numéro `i` de `sfr` à 0. On peut directement passer en premier paramètre un registre de l'arduino pour pouvoir contrôler un bit de ce registre directement.

- voir ce [lien](https://arduino.stackexchange.com/questions/50423/sbi-and-cli-implementation) pour plus de lecture

##### Macro sbi

> ESR_MWRF ligne 22
> RABI_MWRF ligne 22

```c
#define sbi(sfr, bit) (_SFR_BYTE(sfr) |= _BV(bit))  
```

Cette macro est la même que `cbi`, mais tourne le bit à 1.

##### Macro OFFALL

> ESR_MWRF ligne 84
> RABI_MWRF ligne 86

```c
#define OFFALL  PORTD &= B11110011; PORTD |= B01000000;
```

A pour effet d'éteindre le laser, la source de RF et de micro-ondes.

### Fonctions communes aux deux codes

- voir [[Fonctions Communes pour les deux codes Arduino]]

