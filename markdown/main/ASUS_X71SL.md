<!-- source: https://wiki.gentoo.org/wiki/ASUS_X71SL | group: Gentoo Wiki (Main) | wiki-title: ASUS X71SL -->
---
title: ASUS X71SL
url: https://wiki.gentoo.org/wiki/ASUS_X71SL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: ee991b3f1baeb3d8
license: CC BY-SA 4.0
---

# ASUS X71SL

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Asus X71SL is a laptop manufactured by Asus.

Product page: [http://www.asus.com/Notebooks/Multimedia\_Entertainment/X71SL/](http://www.asus.com/Notebooks/Multimedia_Entertainment/X71SL/)

This article describes the hardware on the X71SL and the drivers required to use it.

## Hardware

| Hardware Summary and Support Status |  |  |  |  | 
|---|---|---|---|---|
| Hardware Type | Device | Model | Support | Driver | 
|---|---|---|---|---|
| Processor | Processor | Intel Pentium(R) Dual-Core T4200 (2.00GHz) | Full | acpi-cpufreq | 
|  | Power Management | ACPI | Full | acpi | 
|  | PCI Express Bus | SiS 671MX | Full | pcieport | 
| Secondary Storage | Hard Disk | SiS (SATA / IDE mode!) | Full | pata\_sis? | 
|  | DVD RW Drive | TSSTcorp CDDVDW TS-L633A | Full | sata\_sis | 
|  | Memory Card Reader | Ricoh R5C822 | Full | sdhci-pci | 
| Video Chipset | Discrete GPU | nVidia 9300M GS | Full | nvidia | 
| Input | Keyboard | - | Full | evdev | 
|  | Touchpad | - | Partial | synaptics | 
| Network | Gigabit Ethernet | SiS 191 | Full | sis190 | 
|  | 802.11n Wifi | Atheros AR928X | Full | ath9k | 
|  | Modem | N/A | ? | ? | 
|  | Infrared Interface | N/A | ? | ? | 
| Sound | HD Audio | SiS, Azalia | Full | snd\_hda\_intel (realtek codec) | 
| Peripheral | USB 1.1 | SiS USB 1.1 Controller | Full | ohci\_hcd | 
|  | USB 2.0 | SiS USB 2.0 Controller | Full | ehci\_hcd | 
|  | IEEE 1394 Firewire | Ricoh R5C832 | Full | firewire\_ohci | 
|  | Bluetooth | N/A (optional?) | ? | ? | 
|  | Webcam | Chicony USB2.0 1.3M UVC WebCam | ? | ? | 
|  | Fingerprint Reader | N/A |  |  | 
|  | Brightness Sensor | - | ? | asus\_laptop | 
|  | Info LEDs | - | ? | asus\_laptop | 

## General Configuration

Wired network doesn't work perfectly by default. MTU need to be set to 1492 instead of the default 1500 to work. I don't know why. This problem happens with systemresc cd, Ubuntu and every Linux distribution I tried.

**`/etc/conf.d/net`**

**Wired Network**

You have to create the /etc/init.d/net.eth0 symlink to /etc/init.d/net.lo and start it at boot by:

`root #``rc-update add net.eth0 default`
You don't really need to add it to the default boot level, because net.eth0 is started by hotplug by default. But I noticed that if I use dracut to boot the system, the ip is fine, but the settings in /etc/conf.d/net are ignored!

### Sound

The audio hardware is supported by the Intel HD Audio drivers:

**Audio Support**
