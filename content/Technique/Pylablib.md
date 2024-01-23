---
date: 2024-01-23
aliases: 
tags: doc
draft: false
---

Pylablib est une librairies python permettant l'interfaçage d'un grand nombre d'équipement de laboratoire.
Il est intéressant dans ce projet, car il permet non seulement de contrôler des moteurs piezo mais aussi d'interfacer une caméra Hamamatsu.


Actuellement, `pylablib` et `python 3.12` ne sont pas compatible, car `numba` une dépendance de `pylablib` n'a pas encore été porté pour `python 3.12`.

Contournement:

- [Installation](https://realpython.com/intro-to-pyenv/) de `pyenv` 
- Installation de `python 3.11` dans un environnement `pyenv`
- Utiliser la version `3.11` de python avec la commande `pyenv global 3.11` (pour revenir à la version de python utilisé par défaut par le système, utiliser `pyenv global system`)


## Pylablib pour controller un moteur piézoélectrique Kinesis

- [`KinesisPiezoMotor`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor)
    - [`KinesisPiezoMotor.get_enabled_channels()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_enabled_channels)
    - [`KinesisPiezoMotor.enable_channels()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.enable_channels)
    - [`KinesisPiezoMotor.get_status_n()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_status_n)
    - [`KinesisPiezoMotor.get_status()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_status)
    - [`KinesisPiezoMotor.wait_for_status()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.wait_for_status)
    - [`KinesisPiezoMotor.get_position()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_position)
    - [`KinesisPiezoMotor.set_position_reference()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.set_position_reference)
    - [`KinesisPiezoMotor.move_by()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.move_by)
    - [`KinesisPiezoMotor.move_to()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.move_to)
    - [`KinesisPiezoMotor.jog()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.jog)
    - [`KinesisPiezoMotor.is_moving()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.is_moving)
    - [`KinesisPiezoMotor.wait_move()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.wait_move)
    - [`KinesisPiezoMotor.stop()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.stop)
    - [`KinesisPiezoMotor.get_drive_parameters()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_drive_parameters)
    - [`KinesisPiezoMotor.setup_drive()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.setup_drive)
    - [`KinesisPiezoMotor.get_jog_parameters()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_jog_parameters)
    - [`KinesisPiezoMotor.setup_jog()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.setup_jog)
    - [`KinesisPiezoMotor.Error`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.Error)
    - [`KinesisPiezoMotor.add_background_comm()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.add_background_comm)
    - [`KinesisPiezoMotor.apply_settings()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.apply_settings)
    - [`KinesisPiezoMotor.blink()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.blink)
    - [`KinesisPiezoMotor.check_background_comm()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.check_background_comm)
    - [`KinesisPiezoMotor.close()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.close)
    - [`KinesisPiezoMotor.flush_comm()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.flush_comm)
    - [`KinesisPiezoMotor.get_all_axes()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_all_axes)
    - [`KinesisPiezoMotor.get_all_channels()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_all_channels)
    - [`KinesisPiezoMotor.get_device_info()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_device_info)
    - [`KinesisPiezoMotor.get_device_variable()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_device_variable)
    - [`KinesisPiezoMotor.get_full_info()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_full_info)
    - [`KinesisPiezoMotor.get_full_status()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_full_status)
    - [`KinesisPiezoMotor.get_number_of_channels()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_number_of_channels)
    - [`KinesisPiezoMotor.get_settings()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.get_settings)
    - [`KinesisPiezoMotor.is_opened()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.is_opened)
    - [`KinesisPiezoMotor.list_devices()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.list_devices)
    - [`KinesisPiezoMotor.lock()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.lock)
    - [`KinesisPiezoMotor.locking()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.locking)
    - [`KinesisPiezoMotor.open()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.open)
    - [`KinesisPiezoMotor.query()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.query)
    - [`KinesisPiezoMotor.recv_comm()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.recv_comm)
    - [`KinesisPiezoMotor.remap_axes()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.remap_axes)
    - [`KinesisPiezoMotor.send_comm()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.send_comm)
    - [`KinesisPiezoMotor.send_comm_data()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.send_comm_data)
    - [`KinesisPiezoMotor.set_default_channel()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.set_default_channel)
    - [`KinesisPiezoMotor.set_device_variable()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.set_device_variable)
    - [`KinesisPiezoMotor.status_bits`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.status_bits)
    - [`KinesisPiezoMotor.unlock()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.unlock)
    - [`KinesisPiezoMotor.using_channel()`](https://pylablib.readthedocs.io/en/latest/.apidoc/pylablib.devices.Thorlabs.html#pylablib.devices.Thorlabs.kinesis.KinesisPiezoMotor.using_channel)
