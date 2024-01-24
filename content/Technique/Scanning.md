---
date: 2024-01-24
aliases: balayage
tags: doc
draft: false
---

Le script python `scanning.py` permet de lancer un balayage selon plusieurs dimension dans un volume donné. Ce script attend en argument le chemin vers un fichier de configuration `yaml` (chemin relatif).

### Exemple de fichier de configuration

```python
generic_conf:
  stage: # nanomax
    serial_port: /dev/ttyUSB0 # serial port of the hub controller
    channels: 
      - 0 # x axis
      - 1 # y axis
      - 2 # z axis - optionnal
  spcm: # single photon counter module, à préciser
  export_format: 
  debug: true # print information during scan, such as number of photon received, position, time ...

scan_conf:
  zoi: # zone of interest (define a parallelepiped)
    ref_point: # the nearest point from (0, 0, 0) in the parallelepiped
      - 200
      - 300
      - 10 
    dimensions:
      - 100
      - 100
      - 20
  step_res:
    - 1
    - 1
    - 1
  acquisition_time: 1
```
