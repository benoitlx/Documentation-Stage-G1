---
date: 2024-02-05
aliases: balayage, space-filling curve
tags: doc
draft: false
---

Le module `Scan` permet de scanner une surface ou un volume défini dans un fichier de configuration selon plusieurs paramètres. Des déplacements plus compliqués peuvent être définies par l'utilisateur (voir [[Ajouter un mouvement quelconque]]).

## Documentation de la classe Scan

Un scan se configure via un fichier `.yaml`[^1] par défaut il doit s'appeler `scan.yaml`.

- `zoi`:
  - `ref_point`:
    - `X`: 0
    - `Y`: 0
    - `Z`: null
    - **Explication**: La zone d'intérêt (zoi) est définie comme un parallélépipède. `ref_point` désigne le point de référence de la zoi (zone of interest), c'est-à-dire le point le plus proche de (0, 0, 0) dans le parallélépipède. Dans ce cas, les coordonnées X et Y du point de référence sont définies comme 0, et la coordonnée Z n'est pas utilisé (null).
  - `dimensions`:
    - `X`: 32767
    - `Y`: 32767
    - `Z`: null
    - **Explication**: Les dimensions de la zoi spécifient la taille du parallélépipède dans les directions X, Y et Z. Dans ce cas, la dimension X est de 32767 (la valeur d'extension maximale en tension et position pour le KPZ101), la dimension Y est de 32767 et la dimension Z n'est pas utilisé (null).
- `scan_type`: "spirale"
  - **Explication**: Le type de scan est défini comme une spirale. Cela indique que le système de scan utilisera une trajectoire en spirale pour le balayage.
- `acquisition_time`: 0.02
  - **Explication**: Le temps d'acquisition spécifie la durée pendant laquelle chaque point de la trajectoire de scan sera mesuré. Dans ce cas, il est défini comme 0.02 unités de temps. Ce paramètre est seulement utilisé pour fournir une estimation de la durée du scan.
- `balayage`:
  - `steps`:
    - `X`: 1000
    - `Y`: 1000
    - `Z`: null
    - **Explication**: Pour le type de scan "balayage", le nombre de pas est spécifié dans chaque direction X, Y et Z. Dans ce cas, il y a un pas de 1000 dans les directions X et Y, et la direction Z n'est pas utilisée (null).
- `spirale`:
  - `v`: 50
    - **Explication**: Le paramètre v de la spirale spécifie la vitesse de déplacement le long de la spirale. 
  - `w`: 0.1256
    - **Explication**: Le paramètre w de la spirale spécifie la fréquence de rotation de la spirale. 

Voir [[Exemples]] pour des exemples de configurations.

## Utilisation du module `Scan`

### Méthodes

#### `__init__(self, axis: tuple[KPZ101], config_file="scan.yaml") -> None`

Constructeur de la classe. Initialise le système de balayage ou de spirale en lisant la configuration à partir d'un fichier YAML spécifié. Par défaut, le fichier de configuration est `scan.yaml`. Un tuples de [[KPZ101]] correspondants aux contrôleurs des différents axes doit aussi être passé en paramètre (ex: `(x, y, None)` si deux instances `x` et `y` de la classe [[KPZ101]] sont instancié et que l'axe `z` n'est pas utilisé)

#### `scan(self, function, *args, **kwargs) -> np.ndarray`

Effectue un scan en utilisant la fonction spécifiée. La fonction spécifié est appelé entre chaque mouvement (chaque itération de la boucle) avec les paramètres `*args` et `**kwargs` (paramètres génériques). Retourne un tableau NumPy contenant les résultats du scan.

#### `visualize(self) -> None`

Visualise les coordonnées du scan en 3D.

#### `switch_axis(self, axis: str, n: list[int]) -> enumerate`

Gère la commutation entre les axes pour le balayage.

#### `balayage(self, stepx: float, stepy: float, stepz: float) -> np.ndarray[tuple]`

Génère les coordonnées pour un balayage en fonction des paramètres spécifiés.

#### `spiral(self) -> np.ndarray[tuple]`

Génère les coordonnées pour une spirale en fonction des paramètres spécifiés.

[^1]: https://yaml.org/