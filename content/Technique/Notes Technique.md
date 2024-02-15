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


[¹]: 
```bibtex
@article{10.1063/5.0089161,
    author = {Mariani, G. and Umemoto, A. and Nomura, S.},
    title = "{A home-made portable device based on Arduino Uno for pulsed magnetic resonance of NV centers in diamond}",
    journal = {AIP Advances},
    volume = {12},
    number = {6},
    pages = {065321},
    year = {2022},
    month = {06},
    abstract = "{We describe the realization of a homemade and portable setup to perform experiments of pulsed magnetic resonance of nitrogen-vacancy (NV) centers in diamonds. The system is fully implemented by using an Arduino Uno board equipped with an AVR microcontroller that is used as a transistor-transistor logic pulse sequencer to drive precise laser and microwave pulses with a resolution of 62.5 ns. The equipment is assembled with low-cost modules on a printed circuit board and placed in a compact box with a volume of 20 × 40 × 10 cm3. The detection system is based on a switched integrator and a photodiode in the vicinity of a diamond substrate and read by oversampling the analog-to-digital converter of Arduino Uno. We characterize a CVD diamond sample by performing the pulsed optically detected magnetic resonance and we show the possibility to perform a coherent manipulation of the electron spin of NV centers by driving Rabi oscillations up to 6 MHz with microwave powers within 1 W. We demonstrate different pulse sequences to study electron spin relaxation and dephasing. Finally, we propose additional modules and an antenna to perform the multifrequency manipulation of the electron spin by microwave and radio-frequency pulses. Compared to the previous studies, our system results in a low-cost setup with significantly reduced complexity, which finds application as a learning module for science education and enables a wider audience to access the magnetic resonance in diamond.}",
    issn = {2158-3226},
    doi = {10.1063/5.0089161},
    url = {https://doi.org/10.1063/5.0089161},
    eprint = {https://pubs.aip.org/aip/adv/article-pdf/doi/10.1063/5.0089161/16470477/065321\_1\_online.pdf},
}
```