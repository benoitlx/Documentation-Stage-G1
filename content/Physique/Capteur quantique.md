---
date: 2024-01-22
aliases: quantum sensing
tags: 
  - bibli
  - phy
draft: false
---

## Définition

#### Capteur Quantique

- La configuration énergétique du système quantique doit pouvoir être modélisé par un ensemble de système à deux niveaux d'énergies ($\ket{0}$ et $\ket{1}$ séparé par une énergie $E=\hbar \omega_0$ ([[Qubit]]))
- On doit pouvoir initialiser et lire l'état du système quantique
- #todo à comprendre
- Le système doit interagir avec une grandeur physique $V(t)$ (un champ électrique, magnétique ...). On peut alors introduire le couplage $\gamma = \dfrac{\partial^q E}{\partial^q V}$, qui caractérise les variations d'énergies du système par rapport aux variations de la grandeur $V$.

## Implémentation de Capteurs quantique

| Implémentation | Qubit(s) | grandeurs $V$ | fréquence | méthode d'initialisation/lecture |
| ---- | ---- | ---- | ---- | ---- |
| Vapeur Atomique | Spin Atomique | Champ magnétique - rotation - temps/fréquence | DC-GHz | Optique |
| simple [[Centre NV dans le diamant]] | Spin d'un électron | Champ magnétique - électrique - température - pression - rotation | DC-GHz | Optique |

## Protocole

### Hamiltonien d'un capteur quantique

$$\hat{H}(t) = \hat{H}_0 + \hat{H}_V(t) + \hat{H}_{control}(t)$$

avec $\hat{H}_0$ l'hamiltonien interne au système, $\hat{H}_V$ l'hamiltonien associé au signal $V$ (celui que l'on veut connaitre pour trouver $V$), et $\hat{H}_{control}$ que l'on peut controller comme on veut.

#### Hamiltonien interne
$$\hat{H}_0 = E_0 \ket{0}\bra{0} + E_1 \ket{1}\bra{1}$$

#### Hamiltonien du signal
$$\hat{H}_V(t) = \hat{H}_{V_{\parallel}}(t) + \hat{H}_{V_{\perp}}(t)$$

Avec $\hat{H}_{V_{\parallel}}(t)$ la contribution selon l'axe parallèle à l'axe de référence. (l'axe nitrogen-vacancy dans le cas d'un centre NV)

Les deux termes font intervenir $\gamma$, c'est à dire les variations d'énergie par rapport à $V$.

On peut aussi l'écrire sous la forme :
$$\gamma \vec{V}(t) \cdot \hat{\vec{\sigma}}$$
$$\vec{V}(t)=\{V_x,V_y,V_z\}$$
et $\hat{\vec{\sigma}}$ est un vecteur des [matrices de Pauli](https://en.wikipedia.org/wiki/Pauli_matrices)

#### Hamiltonien de contrôle
Le Hamiltonien de contrôle est obtenue par manipulation de [portes quantiques](https://en.wikipedia.org/wiki/Quantum_logic_gate).

### Protocole

1. Initialisation (exemple état $\ket{0}$)
2. Transformation $\ket{0}$ => $\ket{\psi_0}$ (obtenue par exemple avec une rotation $\pi /2$ voire [portes quantiques](https://en.wikipedia.org/wiki/Quantum_logic_gate))
3. Laisser évolué pendant un temps $t$ sous l'influence de la grandeur $V$ $\ket{\psi_0}$ => $\ket{\psi(t)}$
4. Transformation $\ket{\psi(t)}$ => $\ket{\alpha}$ (obtenue avec une autre rotation dans la [[Qubit#Sphère de Bloch|sphère de Bloch]] en faisant l'autoadjoint de la première transformation)
5. Projection (permet d'obtenir les probabilités d'être dans l'état $\ket{0}$ et $\ket{1}$)

En répétant ces étapes un grand nombre de fois il est possible d'avoir accès aux probabilités et donc de retrouver la grandeur $V(t)$.

Ce protocole est un protocole parmi plusieurs utilisés.

### Exemple

#todo

## Source

```bibtex
@article{Degen2017,
  title = {Quantum sensing},
  volume = {89},
  ISSN = {1539-0756},
  url = {http://dx.doi.org/10.1103/RevModPhys.89.035002},
  DOI = {10.1103/revmodphys.89.035002},
  number = {3},
  journal = {Reviews of Modern Physics},
  publisher = {American Physical Society (APS)},
  author = {Degen,  C. L. and Reinhard,  F. and Cappellaro,  P.},
  year = {2017},
  month = jul 
}
```

