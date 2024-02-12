---
date: 2024-02-12
aliases: 
tags: doc, apt-interface
draft: false
---

> The pypi package is not published yet, it will be soon

The package is based on the library [pyftdi](https://github.com/eblot/pyftdi) which uses `libusb 1.x` as a native dependency and thus should be installed.

## Windows

The easiest way is to install [zadig](https://zadig.akeo.ie/) (it will detect ftdi device automatically)

## Linux

```bash
apt-get install libusb-1.0
```

### udev rules configuration

Udev rules need to be created to communicate with devices without using administrator privilege.
A special `11-ftdi.rules` is available in the `rules` directory (it might be necessary to adapt it if you want to use different devices than mine), you can copy it to `/etc/udev/rules.d` and run the following command as root :

```bash
udevadm control --reload-rules
udevadm trigger
```


Finally, run the following command to install my package (install from source for now).
```bash
pip install apt-interface
```