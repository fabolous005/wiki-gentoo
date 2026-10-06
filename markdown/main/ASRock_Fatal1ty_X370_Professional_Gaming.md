<!-- source: https://wiki.gentoo.org/wiki/ASRock_Fatal1ty_X370_Professional_Gaming | group: Gentoo Wiki (Main) | wiki-title: ASRock Fatal1ty X370 Professional Gaming -->
---
title: ASRock Fatal1ty X370 Professional Gaming
url: https://wiki.gentoo.org/wiki/ASRock_Fatal1ty_X370_Professional_Gaming
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: "3f87db570b17c987"
license: CC BY-SA 4.0
---

# ASRock Fatal1ty X370 Professional Gaming

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The ASRock Fatal1ty X370 Professional Gaming motherboard is a moderate quality gaming motherboard for first generation Ryzen systems (socket AM4). It includes 10 SATA ports, PCI3 4x M.2 socket for NVMe solid state drives, a built in Wi-Fi controller, and two onboard NICs. Because of the emphesis on gaming, it includes two onboard RGB headers and a higher quality integrated sound card.

## Installation

### Motherboard firmware

When exclusively running a Linux based operating system, the motherboard firmware can be updated by using the "Instant Flash" feature. [Download the ROM firmware update](https://www.asrock.com/mb/AMD/Fatal1ty%20X370%20Professional%20Gaming/index.asp#BIOS) from the manufacturer's website, extract onto a [FAT32](https://wiki.gentoo.org/wiki/FAT) formatted USB flash memory device, then reboot the system. Firmware can be discovered and uploaded via the USB flash device from within the motherboard's graphical user interface.

### Kernel

Sensors for Ryzen:

```
Driver `nct6775':
  * ISA bus, address 0x290 
    Chip `Nuvoton NCT5532D/NCT6779D Super IO Sensors' (confidence: 9)
```
Which means `CONFIG_SENSORS_NCT6775` should be enabled in the kernel:

**Enable`CONFIG_SENSORS_NCT6775` support**

```
Device Drivers  --->
   -*- Hardware Monitoring support  --->
      <*>   Nuvoton NCT6775F and compatibles
```
## See also

- [Ryzen](https://wiki.gentoo.org/wiki/Ryzen) — a multithreaded, high performance processor manufactured by AMD.
