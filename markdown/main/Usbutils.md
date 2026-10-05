<!-- source: https://wiki.gentoo.org/wiki/Usbutils | group: Gentoo Wiki (Main) | wiki-title: Usbutils -->
---
title: usbutils
url: https://wiki.gentoo.org/wiki/Usbutils
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-01"
fingerprint: "97870805ca63b3d1"
license: CC BY-SA 4.0
---

# usbutils

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


usbutils is a collection various utilities for querying the the Universal Serial Bus (USB). The most prominent utility included is lsusb, a hardware detection tool for system resources connected to the Universal Serial Bus.

## Installation

### USE flags


### Emerge

`root #``emerge --ask sys-apps/usbutils`
lsusb detects the devices based on an ID database provided by [sys-apps/hwids](https://packages.gentoo.org/packages/sys-apps/hwids) which will be installed as a dependency of usbutils.

## Usage

### Invocation

`user $``qlist usbutils | grep bin/`
/usr/bin/usb-devices
/usr/bin/lsusb
/usr/bin/usbhid-dump

`user $``lsusb -h````
Usage: lsusb [options]...
List USB devices
  -v, --verbose
      Increase verbosity (show descriptors)
  -s [[bus]:][devnum]
      Show only devices with specified device and/or
      bus numbers (in decimal)
  -d vendor:[product]
      Show only devices with the specified vendor and
      product ID numbers (in hexadecimal)
  -D device
      Selects which device lsusb will examine
  -t, --tree
      Dump the physical USB device hierarchy as a tree
  -V, --version
      Show version of program
  -h, --help
      Show usage and help
```
## See also

- [Hardware detection](https://wiki.gentoo.org/wiki/Hardware_detection) — lists and describes utilities used to detect and provide information on hardware.
- [Lshw](https://wiki.gentoo.org/wiki/Lshw) — a small tool that provides detailed information on the hardware configuration of the machine. It can report exact memory configuration, firmware version, mainboard configuration, CPU version and speed, cache configuration, bus speed, etc. on DMI-capable x86 or EFI (IA-64) systems and on some PowerPC machines (PowerMac G4 is known to work).
- [Pciutils](https://wiki.gentoo.org/wiki/Pciutils) — contains various utilities dealing with the [PCI bus](https://en.wikipedia.org/wiki/Peripheral_Component_Interconnect) (primarily lspci).
