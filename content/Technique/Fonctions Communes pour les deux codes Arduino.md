---
date: 2024-02-05
aliases: 
tags: doc
draft: false
---

Cette page de documentation est là pour expliquer les différentes fonctions qu'utilisent les code [[ESR_MWRF]] et [[RABI_MWRF]], il est préférable d'avoir vu les [[Prérequis pour la compréhension des codes Arduino]] afin de comprendre cette page.


## Entrées - sorties - interfaçage avec les modules

### ADF4351

#todo

### AD9834

#todo

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

### Fonction `commands` et liste des commandes

> void commands(char comm) ligne 160 

Les commandes sont interprétés par la fonction `commands`

Liste des commandes:
- `+` Augmente la fréquence RF de `RFstep`
- `-` Diminue la fréquence RF de `RFstep`
- `1`...`4` Règle la puissance RF en `-4`.`-1`.`+2`.`+5` dbm
- `x` Met le switch des micro-ondes sur ON
- `z` Met le switch des micro-ondes sur OFF


> [!tip]-
> Dans un futur code, il peut être intéressant de remplacer tous ces `if else` par une structure comme `switch case` (c'est fait pour)

> [!tip]-
> Il peut aussi être intéressant de remplacer les lignes :
> ```c
> Serial.print("Un message");
> ```
> par
> ```c
> Serial.print(F"Un message")
> ```
> Cela a pour effet d'enregistrer les string dans la mémoire flash plutôt que dans la mémoire RAM, c'est pratique sur des microcontrôleur comme arduino avec une mémoire RAM limité.

