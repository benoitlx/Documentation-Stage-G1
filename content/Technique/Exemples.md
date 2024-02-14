---
date: 2024-02-12
aliases: 
tags: doc, apt-interface
draft: false
---

Tous les exemples suivants sont disponibles sur dans le dossier `confocal` du repo [qsense](https://github.com/yannickdusch/qsense)

## Identifier les Modules avec leur numéro de série

Le programme suivant va faire clignoter l'écran des modules APT ayant les numéros de série "29501986" et "59000407".

```python
from apt_interface.device import Device
from time import sleep

# KPZ101
with Device("29501986", 115200) as dev:
    print("\nWriting")
    print(dev.write(0x0223, 2, 0x00))
    print("should make the KPZ screen blink")
    
sleep(8)

# KSG101
with Device("59000407", 115200) as dev:
    print("Writing")
    print(dev.write(0x0223, 2, 0x00))
    print("should make the KSG screen blink")
```

## Exemple d'utilisation du module KPZ101

Le programme suivant est un exemple simple d'utilisation de python pour contrôler un KPZ101. Le programme permet soit d'envoyer une tension sinusoïdale au KPZ soit d'envoyer directement une consigne de tension.

```c
from apt_interface.KPZ101 import KPZ101, KPZ101Config
from time import sleep
from math import sin, pi

def voltage_prompt(kpz):
    while True:
        p = int(input("desired voltage 0..32767: "))
        kpz.set_output_voltage(p)

def sinus_voltage(kpz):
    i = 0
    while True: 
        v = int(32767/2+(32767/2)*sin(i*4*pi/100))
        kpz.set_output_voltage(v)
        i+=1
        sleep(.1)


with KPZ101(config_file="conf/config_KPZ.yaml") as kpz:
    print(kpz.conf)
    print(kpz.get_info())

    kpz.enable_output()

    voltage_prompt(kpz)
    # sinus_voltage(kpz)
```

Ce programme nécessite un fichier de configuration situé dans le répertoire `conf/config_KPZ.yaml` par rapport au répertoire du fichier python. Voici son contenu :

```yaml
name: X_axis_controller # nom du contrôleur
serial_nm: "29501986" # numéro de série du contrôleur
baudrate: 115200 # vitesse du port série
mode: open_loop # mode donne au contrôleur (ici open car contrôle en tension)
voltage_limit: 75 # tension maximale (il est possible de la changer mais le nanomax n'accepte pas des tensions superieur à 75V)
```

## Exemple d'utilisation du module KSG101

Le programme suivant relève toutes les secondes la valeur lu par la gauge.

```python
from apt_interface.KSG101 import KSG101, KSG101Config
from time import sleep

with KSG101(config_file="conf/config_KSG.yaml") as ksg:
    print(ksg.conf)
    print(ksg.get_max_travel())

    ksg.identify()

    while True:
        print(ksg.get_reading()) 
        sleep(1)
```

De même voici le contenu du fichier de configuration :

```yaml
name: X_axis_gauge
serial_nm: "59000407"
baudrate: 115200
out: chann2 # la valeur de la gauge est disponible sur la voie 2
unit: pos 
```


## Utilisation du KPZ et du KSG en boucle fermée

Ce programme demande la position, envoie la consigne au KPZ affiche la position actuelle avec le KSG et recommence.

```python
from apt_interface.KSG101 import KSG101
from apt_interface.KPZ101 import KPZ101
from time import sleep

with KSG101("conf/config_KPZ.yaml") as ksg, KPZ101("conf/config_KSG.yaml") as kpz:
    print(ksg.conf)
    print(kpz.conf)
    ksg.get_io() 
    ksg.get_max_travel()

    kpz.enable_output()
    ksg.zeroing()

    while True:
        p = int(input("desired position 0..32767: "))
        kpz.set_position(p)
        sleep(2)
        print("reading -32768..32767: ", ksg.get_reading())
```

Voici les fichiers de configurations correspondants :

##### KPZ
```yaml
name: X_axis_controller
serial_nm: "29501986"
baudrate: 115200
mode: closed_loop
feedback_in: chann2 # où récupérer le signal
voltage_limit: 75 
```

##### KSG
```yaml
name: X_axis_gauge
serial_nm: "59000407"
baudrate: 115200
out: chann2 # où envoyer le signal
unit: pos 
```


## Scan d'une zone avec un KPZ 

### Configuration

```yaml
zoi: # zone of interest (define a parallelepiped)
  ref_point: # the nearest point from (0, 0, 0) in the parallelepiped
    X: 0 
    Y: 0 
    Z: 0 
  dimensions:
    X: 32767 
    Y: 32767
    Z: 32767 
scan_type: "balayage"
acquisition_time: 0.01
balayage:
  steps:
    X: 1000
    Y: 1000
    Z: 5000 
spirale:
  rmax: 100
  n: 10000
  w: 0.1256
mode: "open_loop"
```

### Simulation

Le parcours que va effectué le plateau peut être visualisé avec le programme suivant :

```python
from apt_interface.scan import Scan, ScanConfig
from time import sleep

def mesure(*args, **kwargs):
    sleep(.1)

    return 1

s = Scan((None, None), config_file="conf/scan.yaml")

print(s.conf)
print(s.coords)

s.visualize()
```

On obtient une animation du type

![[Figure_1.png|600]]

### Déplacement du plateau

```python
from apt_interface.KPZ101 import KPZ101
from apt_interface.scan import Scan
from time import sleep

def mesure(*args, **kwargs):
    # il faut laisser assez de délais pour que les mouvements ne soient pas trop brusques si le pas est grand

    sleep(.01)
    return 1

with KPZ101(config_file="conf/x.yaml") as x, KPZ101(config_file="conf/y.yaml") as y:
    # La boucle fermée n'est pas utiliser ici, il aurait fallu instancier des ksg
    # pour les configurer sinon

    print(x.conf)
    print(y.conf)


    x.enable_output()
    y.enable_output()

    s = Scan((x, y))

    # Launching scan on nanomax
    m = s.scan(mesure)

    # m contains the matrix of measurement
    print(m)

    while True:
        pass
```