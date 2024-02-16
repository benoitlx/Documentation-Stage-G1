---
date: 2024-02-12
aliases: 
tags: doc, apt-interface
draft: false
---

Il faut créer un nouveau fichier `devicename.py` dans le dossier `apt_interface`.

## Template

Voici un template pour un nouveau device :

```python
from .device import Device
from pydantic import BaseModel, field_validator, Field, ValidationInfo
from pydantic_yaml import parse_yaml_file_as
from apt_interface import VALID_BAUDRATES
from struct import pack
from typing import Literal, Annotated


class DeviceNameConfig(BaseModel):
    """Description du fichier yaml"""

    name: str = "KPZ101_default_controller"

	# remplacez xx par les deux premiers chiffre du numéro de série du device
    serial_nm: Annotated[str, Field(pattern=r"^xx.*")]
    baudrate: VALID_BAUDRATES = 115200


class DeviceName():
    pass

class DeviceName():

    def __init__(self, config_file="config_devicename.yaml") -> None:
        self.conf = parse_yaml_file_as(DeviceNameConfig, config_file)

        self.dev = Device(self.conf.serial_nm, self.conf.baudrate)

    def __enter__(self) -> DeviceName:
        self.dev.begin_connection()
        # ajouter des fonction à appeller au début de la connection
        return self
    
    def identify(self) -> bool:
        """MGMSG_MOD_IDENTIFY"""

		# Les paramètres devraients marcher sur la plupart des appareils
        return self.dev.write(0x0223, 2, 0x00)


    def __exit__(self, *exc_info) -> None:
	    # Ajouter des fonctions à appeller avant que la connexion s'arrête
        self.dev.end_connection()

if __name__ == "__main__":
    with DeviceName() as dev:
        dev.identify()
```

## Importation

```python
from .device import Device
from pydantic import BaseModel, field_validator, Field, ValidationInfo
from pydantic_yaml import parse_yaml_file_as
from apt_interface import VALID_BAUDRATES
from struct import pack
from typing import Literal, Annotated
```

- La classe `Device` permet de faire la communication bas niveau avec le device.
- Le module `pydantic` permet de créer un "clone" python du fichier de configuration `yaml`. On doit préciser les types de chaque champs afin qu'ils soient convertis automatiquement et que l'on puisse vérifier la cohérence du fichier de configuration.
- Le module `pydantic_yaml` permet d'importer le fichier `yaml` et de le traiter directement avec `pydantic`
- `VALID_BAUDRATES` est un type de donné qui n'autorise que les valeurs usuelles de baudrate
- La fonction `pack`[^1] permet de transformer une suite de paramètre de différentes nature en une liste d'octets. Son homologue `unpack` fait la transformation inverse. C'est utile quand il faut extraire des données renvoyer par un device.
- `Literal` et `Annotated` sont des types python permettant de construire d'autres types par exemple `Literal['a', 'b']` est un type de donnée n'autorisant que le string `'a'` et `'b'`.

[^1]: https://docs.python.org/3/library/struct.html