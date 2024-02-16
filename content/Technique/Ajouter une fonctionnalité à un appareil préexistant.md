---
date: 2024-02-12
aliases: 
tags: doc, apt-interface
draft: false
---

Disons que l'on veut rajouter une fonctionnalité `set_display_timeout()` à un [[KPZ101]]. Cette fonction pourrait prendre en paramètre un entier indiquant le nombre de seconde avant que l'écran du KPZ s'éteigne. Mais il est plus judicieux de configurer ce comportement dans le fichier de configuration `yaml` associé.

Ajoutons donc: `disp_timeout: 100` dans le fichier `kpz101.yaml`

Pour faire comprendre à notre programme qu'un nouveau champ est disponible, il faut rajouter dans la classe `KPZ101Config()` `disp_timeout: int = 50`.

```python
class KPZ101Config(BaseModel):
	# ...

	# name ----- type - default
	disp_timeout: int = 50
```

> [!important]+
> Il est important d'indiquer le type pour que mon programme puisse automatiquement convertir les valeurs du fichier `yaml` en des valeurs manipulable en python.

Pour plus d'info sur ce que l'on vient de faire, voir [pydantic](https://docs.pydantic.dev/latest/)

Il faut maintenant implémenter notre fonction `set_disp_timeout()`:
```python
class KPZ101():
	# ...

	def set_disp_timeout(self) -> None:
		#              func  param1                 param2
		self.dev.write(0x00, self.conf.disp_timeout, 0x00)

		# Les valeurs ne sont ici que représentative !
```

Pour savoir quelle valeur donner à `func` `param1` et `param2`, il faut aller voir la [documentation APT](https://www.thorlabs.com/Software/Motion%20Control/APT_Communications_Protocol.pdf). Si des données sont à transmettre en plus, il faut utiliser la fonction [[Device#`write_with_data(self, func bytes, data_length bytes, data bytes) -> bool`]] de la classe `Device`