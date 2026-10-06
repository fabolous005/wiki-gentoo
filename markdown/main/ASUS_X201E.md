<!-- source: https://wiki.gentoo.org/wiki/ASUS_X201E | group: Gentoo Wiki (Main) | wiki-title: ASUS X201E -->
---
title: ASUS X201E
url: https://wiki.gentoo.org/wiki/ASUS_X201E
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "7f81885bd1ab3048"
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

```
Device Drivers  --->
    <*> Serial ATA and Parallel ATA drivers  --->  
        <*>   AHCI SATA support
```
### Memory Card Reader

TODO

### Video Chipset

Integrated Intel HD Graphics uses I915 driver:

**Video Support**

```
Device Drivers  --->
        Graphics support  --->  
        <*> Intel 8xx/9xx/G3x/G4x/HD Graphics
```
### Input Devices

The keyboard support for X11 is provided by evdev.

To make `Fn` + `F5` (brightness down) and `Fn` + `F6` (brightness up) function keys work it is needed to unspecify acpi\_osi in GRUB.

**`/etc/default/grub`**

**Brightness Keys Support**

```
GRUB_CMDLINE_LINUX_DEFAULT='quiet splash acpi_osi='
```
Nevertheless brightness set with keyboard will not synchronize with brightness set with KDE.

Touchpad support is provided through synaptics.

**`/etc/portage/package.use/00input`**

```
*/* INPUT_DEVICES: evdev synaptics
```

Also you must enable CONFIG\_MOUSE\_PS2\_ELANTECH in kernel.

**Touchpad Support**

```
Device Drivers  --->
    [*] Input device support  --->  
        [*] Generic input layer (needed for keyboard, mouse, ...)  --->
            [*] Mice  --->
                [*] PS/2 mouse  --->
                    [*] Elantech PS/2 protocol extension
```
Double- and triple- tapping and scroll will work, although 4- and 5- finger tap will not be recognized (driver issue?).

### Ethernet

Networking is provided by Qualcomm Atheros AR8162 Fast Ethernet. Alx driver is needed:

**Ethernet Support**

```
Device Drivers  --->
    [*] Network device support  --->  
        [*] Ethernet driver support  --->
            [*] Atheros devices  --->
                [*] Qualcomm Atheros AR816x/AR817x support
```
### 802.11 Wifi

Wifi is provided by Qualcomm Atheros AR9485 Wireless Network Adapter. ath9k driver is needed:

**Wifi Support**

```
Device Drivers  --->
    [*] Network device support  --->  
        [*] Wireless LAN  --->
            [*] Atheros Wireless Cards  --->
                [*] Atheros 802.11n wireless cards support  --->
                    [*] Atheros ath9k PCI/PCIe bus support
```
### Bluetooth

Though Atheros AR9485 has integrated bluetooth, ath9k driver doesn't support it.

### Sound

Sound system is based on Intel HD Audio and could be easily brought up by snd\_hda\_intel driver:

**Audio Support**

```
Device Drivers  --->
    <*> Sound card support  --->  
        <*> Advanced Linux Sound Architecture  --->
            [*] PCI sound devices  --->
                <*> Intel HD Audio
```
### USB/USB3.0

TODO (not tested)

### Webcam

Webcam is supported with standart UVC:

**Webcam Support**

```
Device Drivers  --->
    <*> Multimedia support  ---> 
        [*] Media USB Adapters  ---> 
            <*> USB Video Class (UVC)
```
