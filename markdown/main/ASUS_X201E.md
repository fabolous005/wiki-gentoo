<!-- source: https://wiki.gentoo.org/wiki/ASUS_X201E | group: Gentoo Wiki (Main) | wiki-title: ASUS X201E -->
---
title: ASUS X201E
url: https://wiki.gentoo.org/wiki/ASUS_X201E
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "2f899a7b6feff4c9"
license: CC BY-SA 4.0
---

# ASUS X201E

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Asus x201e is ultra-slim and light notebook manufactured by Asus. It has Intel Core i3, Pentium or Celeron processors with integrated graphics with up to 4GB of RAM. This article can help you to install Gentoo Linux on Asus X201E and point to typical problems during installation and setting the system.

## Hardware

### General Configuration

There is no need for any specific configuration except drivers and one GRUB hack to make function keys work.

### Hard Disks and DVD Drives

The only hard drive is connected via SATA. There is no empty space to place another hard drive or cd/dvd drive. Driver in use for hard drive is ahci:

**SATA Hard Drive Support**

### Memory Card Reader

TODO

### Video Chipset

Integrated Intel HD Graphics uses I915 driver:

**Video Support**

### Input Devices

The keyboard support for X11 is provided by evdev.

To make `Fn` + `F5` (brightness down) and `Fn` + `F6` (brightness up) function keys work it is needed to unspecify acpi\_osi in GRUB.

**`/etc/default/grub`**

**Brightness Keys Support**

Nevertheless brightness set with keyboard will not synchronize with brightness set with KDE.

Touchpad support is provided through synaptics.

**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```

Also you must enable CONFIG\_MOUSE\_PS2\_ELANTECH in kernel.

**Touchpad Support**

Double- and triple- tapping and scroll will work, although 4- and 5- finger tap will not be recognized (driver issue?).

### Ethernet

Networking is provided by Qualcomm Atheros AR8162 Fast Ethernet. Alx driver is needed:

**Ethernet Support**

### 802.11 Wifi

Wifi is provided by Qualcomm Atheros AR9485 Wireless Network Adapter. ath9k driver is needed:

**Wifi Support**

### Bluetooth

Though Atheros AR9485 has integrated bluetooth, ath9k driver doesn't support it.

### Sound

Sound system is based on Intel HD Audio and could be easily brought up by snd\_hda\_intel driver:

**Audio Support**

### USB/USB3.0

TODO (not tested)

### Webcam

Webcam is supported with standart UVC:

**Webcam Support**
