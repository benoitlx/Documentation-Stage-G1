---
date: 2024-02-12
aliases: 
tags: doc, apt-interface
draft: false
---
#todo

## Utilisation du module `Device`

> [!warning]-
> On Linux, you also need to create a udev configuration file to allow user-space processes to access to the FTDI devices. 
> Look [here](https://eblot.github.io/pyftdi/installation.html) to find how to configure udev rules.
> This way, `pyftdi` (the library that run low level communication command) will find all the ftdi chips in kinesis module.
> see [[Installation]]

### Méthodes

#### `__init__(self, sn: str, baud: VALID_BAUDRATES) -> None`

Constructeur de la classe. 
Initialise les paramètres du Device (serial number et baudrate)

#### `begin_connection(self) -> None`

Ouvre une connexion bas niveau avec le device ayant son numéro de série.

#### `__enter__(self) -> Device`

Retourne un `Device` avec une connection établie. (voir [context manager](https://docs.python.org/3/library/contextlib.html) pour plus d'info)

#### `read_data(self, func: bytes, size: int) -> bytes`

Interroge le device avec la fonction `func` (voir la [documentation](https://www.thorlabs.com/Software/Motion%20Control/APT_Communications_Protocol.pdf) APT pour les différentes fonctions possibles), et renvoi les `size` octets retourné par le device.

#### `write(self, func: bytes, param1: bytes, param2: bytes) -> bool`

Exécute la fonction `func` sur le device avec les paramètres `param1` et `param2`.
Renvoi `True` si la commande s'est bien exécuté et `False` sinon.

#### `write_with_data(self, func: bytes, data_length: bytes, data: bytes) -> bool`

Exécute la fonction `func` sur le device en envoyant les données `data` codés sur `data_length` octets.
Renvoi `True` si la commande s'est bien exécuté et `False` sinon.

#### `end_connection(self) -> None`

Termine la connection avec le device

#### `__exit__(self, *exc_info) -> None`

Termine la connection, sortie du contexte d'exécution.