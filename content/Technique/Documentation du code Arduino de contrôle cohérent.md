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

#### Macros

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