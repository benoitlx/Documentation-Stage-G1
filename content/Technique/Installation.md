---
date: 2024-02-12
aliases: 
tags: doc, apt-interface
draft: false
---

> The package is for now on test_pypi it will be moved on pypi in times !

The package is based on the library [pyftdi](https://github.com/eblot/pyftdi) which uses `libusb 1.x` as a native dependency and thus should be installed.

## Linux

```bash
apt-get install libusb-1.0
```

### udev rules configuration

Udev rules need to be created to communicate with devices without using administrator privilege.
A special rule `11-ftdi.rules` is available in the `rules` directory (it might be necessary to adapt it if you want to use different devices than mine), you can copy it to `/etc/udev/rules.d` and run the following command as root :

```bash
udevadm control --reload-rules
udevadm trigger
```

## Windows

The easiest way is to install [zadig](https://zadig.akeo.ie/) (it will detect ftdi device automatically)

1. Start up the Zadig utility
2. Select `Options/List All Devices`, then select the FTDI devices you want to communicate with. Its names depends on your hardware, _i.e._ the name stored in the FTDI EEPROM.
 - With FTDI devices with multiple channels, such as FT2232 (2 channels) and FT4232 (4 channels), you **must** install the driver for the composite parent, **not** for the individual interfaces. If you install the driver for each interface, each interface will be presented as a unique FTDI device and you may have difficulties to select a specific FTDI device port once the installation is completed. To make the composite parents to appear in the device list, uncheck the `Options/Ignore Hubs or Composite Parents` menu item.
 - Be sure to select the parent device, _i.e._ the device name should not end with _(Interface N)_, where _N_ is the channel number.
     - for example _Dual RS232-HS_ represents the composite parent, while _Dual RS232-HS (Interface 0)_ represents a single channel of the FTDI device. Always select the former.
3. Select `libusb-win32` (not `WinUSB`) in the driver list.
4. Click on `Replace Driver`

## Pip Install

Finally, run the following command to install my package (only in test.pypi for now).
```bash
pip install -i https://test.pypi.org/simple/ apt-interface
#pip install apt-interface
```