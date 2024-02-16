---
date: 2024-02-12
aliases: 
tags: doc, apt-interface
draft: false
---

## Mouvement sans configuration

Pour créer un mouvement quelconque qui n'a pas besoin de paramètre externe pour être configuré, il suffit d'éditer le fichier `scan.py` et d'ajouter une méthode générant les coordonnés à la classe `Scan`. Cette méthode doit renvoyer un objet de type `np.ndarray[tuple]` c'est à dire une liste de coordonnés `(x, y, z)`. Si l'un des axes n'est pas utilisé, il suffira de remplacer une composante par un `None`.

Par exemple, si l'on veut générer un déplacement selon la diagonale, on peut écrire le code de cette manière :

```python
class Scan():

	# ...

	def diagonale(self) -> np.ndarray[tuple]:
		n = self.conf.zoi.dimensions.X
		
		# On créer la liste qui va être retourné 
		coords = np.zeros(n, dtype=(float, 3))

		# On remplie la liste
		for i in range(n):
			coords[i] = (i, i, None)

		return coords
```

Pour pouvoir utiliser cette méthode, il convient de modifier deux choses.

Il faut modifier le type de l'attribue `scan_type` dans la classe `ScanConfig`, afin d'ajouter la possibilité d'écrire le nom de notre fonction dans le fichier `scan.yaml` :
```python
class ScanConfig(BaseModel):
	# ...
	scan_type: Literal["balayage", "spirale", "diagonale"]
	# ...
```

Il faut aussi modifier le contenue de la méthode `__init__(...)` de la classe `Scan` :

```python
class Scan():

	def __init__(self, axis: Tuple[KPZ101], config_file="scan.yaml") -> None:
		# ...
		match self.conf.scan_type:
			case "balayage": 
			case "spirale":
			case "diagonale":
				self.coords = self.diagonale()
```

Cela aura pour effet de générer les coordonnés lors de la création d'un objet de type `Scan`.

A partir de maintenant, il suffit de mettre `scan_type: diagonale` dans le fichier de configuration `yaml` et les nouvelles coordonnées seront disponibles

## Mouvement avec configuration

Le mouvement généré par la méthode que l'on a créé précédemment ne peut pas s'adapter à des paramètres d'un fichier de configuration. Il peut être intéressant d'ajouter dans le fichier de configuration des paramètres pour la méthode que l'on vient de créer. Disons par exemple :
```yaml
diagonale:
	diag_number: 0 
```

`diag_number` étant un paramètre pour indiquer quelle diagonale suivre (`(0, 0)` $\rightarrow$ `(X_max, Y_max)` ou `(X_max, 0)` $\rightarrow$ `(0, Y_max)`)

Il faudra ainsi rajouter une classe nommée `DiagonaleConfig` dans le fichier `scan.py` :

```python
class DiagonaleConfig(BaseModel):
	# name ---- type - default value
	diag_number: int = 0
```

et ajouter dans la classe `ScanConfig` :

```python
class ScanConfig(BaseModel):
	# ...
	diagonale: Optional[DiagonaleConfig] = None
```

Après ça, il sera possible d'utiliser tous les paramètres du fichier `yaml` dans la méthode `digonale` en faisant `self.conf.diagonale.diag_number`.

Si vous avez été jusque là, que vous avez implémenté votre fonction, et que vous pensez qu'elle peut être utile à d'autres personnes n'hésitez pas à faire une [Pull-request](https://github.com/benoitlx/APT-interface/pulls) de vos changements.