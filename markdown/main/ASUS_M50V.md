<!-- source: https://wiki.gentoo.org/wiki/ASUS_M50V | group: Gentoo Wiki (Main) | wiki-title: ASUS M50V -->
---
title: ASUS M50V
url: https://wiki.gentoo.org/wiki/ASUS_M50V
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "2f059a6599a6d8dd"
license: CC BY-SA 4.0
---

# ASUS M50V

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

The Asus M50V is a laptop manufactured by Asus in 2009. It features an Intel Core 2 Duo P8400 along with 4GB of RAM, a 250GB hard disk, and an NVIDIA GeForce 9600M GS with 1GB of VRAM.

This article describes the hardware on the M50V and the drivers required to use it.

## Hardware

| Hardware Summary and Support Status |  |  |  |  | 
|---|---|---|---|---|
| Hardware Type | Device | Model | Support | Driver | 
|---|---|---|---|---|
| Processor | Processor | Intel Core 2 Duo P8400 | Full | acpi-cpufreq | 
|  | Power Management | ACPI | Full | acpi | 
|  | PCI Express Bus | Intel ICH9 PCIe | Full | pcieport | 
| Secondary Storage | Hard Disk | Intel ICH9M SATA AHCI Controller | Full | ahci | 
|  | DVD RW Drive | HL-DT-ST DVDRAM GSA-T50N | Full | ahci | 
|  | Memory Card Reader | Ricoh R5C822 | Full | sdhci-pci | 
| Video Chipset | Discrete GPU | NVIDIA 9600M GS | Partial | nouveau | 
| Input | Keyboard | - | Full | evdev | 
|  | Touchpad | - | Partial | synaptics | 
| Network | Gigabit Ethernet | Realtek 8168B | Full | r8169 | 
|  | 802.11n Wifi | Atheros AR928X | Full | ath9k | 
|  | Modem | ? | ? | ? | 
|  | Infrared Interface | ? | ? | ? | 
| Sound | HD Audio | Intel ICH9 HD Audio | Full | snd\_hda\_intel | 
| Peripheral | USB 1.0 | Intel ICH9 UHCI USB Controller | Full | uhci\_hcd | 
|  | USB 2.0 | Intel ICH9 EHCI USB Controller | Full | ehci\_hcd | 
|  | IEEE 1394 Firewire | Ricoh R5C832 | Full | firewire\_ohci | 
|  | Bluetooth | ASUSTek BT-253 | ? | bluetooth | 
|  | Webcam | ? | Full |  | 
|  | Fingerprint Reader | AuthenTec AES1600 | ? |  | 
|  | Brightness Sensor | - | ? | asus\_laptop | 
|  | Info LEDs | - | ? | asus\_laptop | 

### General Configuration

TODO

### Hard Disks and DVD Drives

The hard disk controller should work with the standard AHCI driver:

**AHCI Driver**

Make sure that AHCI support is compiled into the kernel, and not as a module, or the kernel may fail to boot.

The DVD drive may lock up spinning a disc upon insertion due to bad firmware.
A firmware update exists, but the install tool is made for Windows, so it may be a good idea to install the update first if coming from a Windows environment.
Note that installing this update may void the warranty.
See [this bug](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/309384) for more details.

### Memory Card Reader

The memory card reader should work fine with the standard driver:

**Card Reader Support**

### Video Chipset

The video chipset is an NVIDIA GeForce 9600M GS, which has full support for 2D/3D acceleration, CUDA, and OpenCL with the proprietary drivers.
At the time of this writing, the open source drivers provide 2D acceleration and partial 3D support.
See [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) for details on video card support and driver selection.

### Input Devices

**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```
The media buttons on the touchpad are not supported, but normal touchpad and scrollwheel functionality works perfectly.

### Ethernet

Networking is provided by a Realtek 8168B Gigabit Ethernet device.

**Ethernet Support**

### 802.11 Wifi

Wifi is provided by an Atheros AR928X wifi adapter:

**Wifi Support**

### Sound

The audio hardware is supported by the Intel HD Audio drivers:

**Audio Support**

### USB

USB support is provided by the standard drivers:

**USB Support**

### Firewire

The Firewire port should work fine with the standard driver:

**Firewire Support**

At the time of this writing, the author has not tested Firewire.

### Bluetooth

TODO

### Webcam

The built-in webcam is a is supported through V4L with USB2.0 UVC:

**Webcam Support**

### Fingerprint Reader

The fingerprint reader should be supported through USB and libfprint. See [Fingerprint Reader](https://wiki.gentoo.org/wiki/Fingerprint_Reader).

### ASUS Extras

TODO
