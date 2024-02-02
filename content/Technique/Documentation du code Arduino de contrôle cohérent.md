---
date: 2024-02-02
aliases: 
tags: doc
draft: false
---

## Prérequis pour comprendre le programme

### Architecture de l'Atemga328p

![[Pasted image 20240202152148.png]]

### Notions de C

#### Compilation

La compilation permet à partir d'un code source de produire un fichier exécutable par une machine. Dans le cas d'un microcontrôleur, il s'agit de produire un code hexadécimal qui sera transversé dans la mémoire flash du microcontrôleur.

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

La macro `NOP` permet de ne rien faire pendant un cycle d'horloge du processeur soit pendant 62.5 nanosecondes.
Elle est implémentée de la manière suivante:
```c
#define NOP __asm__ __volatile__("nop\n\t"); 
```

`__asm__` est un mot clé du language C permettant d'introduire un code assembleur dans un code C. Ici la seule instruction assembleur introduite est la `nop` qui permet de faire ce qui a été décrit précédemment. Le mot clé `__volatile__` permet quant à lui d'éviter les optimisations du compilateur. En effet, sans celui ci le compilateur supprimerait de son programme toutes les fois où l'instruction `NOP` serait appelé, car il ne peut pas comprendre l'utilité d'une telle opération.

##### Macro cbi

> ESR_MWRF ligne 21

##### Macro sbi

> ESR_MWRF ligne 22


## Code ESR_MWRF

- voir [[ESR_MWRF]]

## Code RABI_MWRF

- voir [[RABI_MWRF]]