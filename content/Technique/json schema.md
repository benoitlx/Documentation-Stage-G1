---
date: 2024-02-16
aliases: 
tags: doc, apt-interface
draft: false
---

De l’auto-complétion (testé sur vs-code) peut être configuré pour les fichiers de configuration `yaml`.

Il faut:
- installer l'extension `yaml` de RedHat
- dans un fichier `schema.py` il faut importer `json` et les classes de configurations des appareils (`KPZ101Config`, `KSG101Config`, `ScanConfig`) et ajouter la ligne `print(json.dumps(KPZ101Config.model_json_schema()))
- dans un terminal il faut lancer la commande `python schema.py > json_schema/kpz101_schema.json`
- dans le fichier `yaml` où l'auto-complétion est souhaité, il faut ajouter en première ligne `# yaml-language-server: $schema=json_schema/kpz101_schema.json` (il faut adapter l'emplacement du fichier `kpz101_schema.json`)