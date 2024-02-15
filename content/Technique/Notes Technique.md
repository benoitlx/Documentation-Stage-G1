---
date: 2024-01-20
aliases: 
tags: doc 
draft: false 
---

## Architecture logiciel envisageable pour le projet Qsense

#### Structure de Fichiers

- `hardware` (contenant les fichiers Kicad, de CAO ...)
	- `MW Antenna ODMR` 
	- `Permanent Magnet` (support pour aimant et capteur MV2)
	- `Electro Magnet`
	- ...
- `Simulation` 
	- `Ramsey`
	- `Rabi`
	- ...
- `Result` (pour stocker les résultats d'expérience, fichier csv ...)
- `conf` (Configuration des appareils et de l'expérience)
	- `x.yaml`
	- `y.yaml`
	- `z.yaml`
	- `scan.yaml` (pour le nanomax, voir [[APT-interface]])
	- `pulse_ESR.yaml` (contenant les informations temporelles pour générer les signaux RF et les impulsions laser)
	- `pulse_RABI.yaml`
	- `camera.yaml` 
	- ...
- `src` Les sources python
	- `control and acquisition`
		- `pulse_generator.py` utilise [spinapi](https://pypi.org/project/spinapi/)
		- `agilent_control.py` utilise [pyVisa](https://pypi.org/project/PyVISA/)
		- `camera.py` utilise [hamamatsu](https://pypi.org/project/hamamatsu/)
		- `nanomax.py` utilise [[APT-interface]]
		- `magnet.py` interagi avec l'arduino et le capteur MV2 
		- ...
	- `Web` (l'interface de contrôle et de visualisation des donnés)
	- `main.py` le point d'entrée de l'expérience


## Documentation du module APT-interface

Ce module va permettre de contrôler le nanomax pour le [[Microscopie Confocale|microscope]].

- [[APT-interface]]

## Documentation Arduino du contrôle cohérent de spin

Ce montage avec arduino extrait de la publication de Mariani[^1] est une version miniaturisé du projet [Qsense](https://github.com/yannickdusch/qsense) permettant de mettre en évidence quelques résultats et d'illustrer des phénomènes physique à des fins d'éducation.

- [[Documentation Code Arduino]]

### Autres outils découvert pendant le stage (qui peuvent être utiles dans d'autres expériences à l'IEMN)

- [ARTIQ](https://m-labs.hk/artiq/manual/introduction.html) pour simplifier la programmation de pulse sequencer et plus généralement de toute expérience de physique quantique ayant des contraintes de temps qui requiert l'utilisation d'un FPGA
- [pylablib](https://pylablib.readthedocs.io/en/latest/) Un module python regroupant plein d'appareil de labo. La caméra hammamatsu est supportée, mais le dépôt github est maintenue par une seule personne (comme mon repo à l'heure actuelle)
- [cubini](https://github.com/Schlabonski/cubini) un dépot github qui implémente la communication avec un [[KPZ101]] 


[^1]: [[References#Mariani]]