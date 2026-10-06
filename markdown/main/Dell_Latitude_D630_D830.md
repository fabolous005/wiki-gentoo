<!-- source: https://wiki.gentoo.org/wiki/Dell_Latitude_D630_D830 | group: Gentoo Wiki (Main) | wiki-title: Dell Latitude D630 D830 -->
---
title: Dell Latitude D630 D830
url: https://wiki.gentoo.org/wiki/Dell_Latitude_D630_D830
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "1a4e49c78da79e2c"
license: CC BY-SA 4.0
---

# Dell Latitude D630 D830

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the hardware and the respective drivers on Dell Latitude D630/D830 laptops.

## Hardware and Drivers

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes |  | 
|---|---|---|---|---|---|---|---|
| CPU | Intel Core Duo | Works |  | [Intel Core 2](https://wiki.gentoo.org/wiki/Intel_Core_2) |  |  |  | 
| Hard disk drive | Hitachi Travelstar | Works |  | [ahci](https://wiki.gentoo.org/wiki/HDD) |  |  |  | 
| Optical drive | TSSTcorp DVD+-RW TS-L632D | Works |  | [ata\_piix](https://wiki.gentoo.org/wiki/CDROM) |  |  |  | 
| Graphic card | Intel GMA X3100M | Works |  | [intel](https://wiki.gentoo.org/wiki/Intel) |  |  |  | 
| Graphic card | NVIDIA Quadro NVS 135M | Works |  | [nvidia-drivers](https://wiki.gentoo.org/wiki/Nvidia-drivers)/[Nouveau](https://wiki.gentoo.org/wiki/Nouveau) |  |  |  | 
| Keyboard |  | Works |  | [evdev](https://wiki.gentoo.org/wiki/Evdev) |  |  |  | 
| Touchpad | ALPS GlidePoint | Works |  | [synaptics](https://wiki.gentoo.org/wiki/Synaptics) |  |  |  | 
| Ethernet | Broadcom NetXtreme BCM5755M | Works |  | [tg3](https://wiki.gentoo.org/wiki/Ethernet) |  |  |  | 
| WiFi | Intel PRO/Wireless 3945ABG / 4965AGN | Works |  | [iwl3945 / iwl4965](https://wiki.gentoo.org/wiki/Wifi) |  | See [Intel Corporation PRO/Wireless 3945ABG](https://wiki.gentoo.org/wiki/Intel_Corporation_PRO/Wireless_3945ABG) |  | 
| WiFi | Dell Wireless 1390 / 1490 / 1505 | Works |  | [b43](https://wiki.gentoo.org/wiki/Wifi) |  |  |  | 
| Modem | Connexant HSF |  |  |  |  | Proprietary software, full version with costs |  | 
| UMTS modem | Dell Wireless 5520 HSDPA |  |  |  |  |  |  | 
| CDMA modem | Dell Wireless 5720 EVDO |  |  |  |  |  |  | 
| Sound card | Intel HD Audio, SigmaTel STAC9205 Codec | Works |  | [snd\_intel\_hda](https://wiki.gentoo.org/wiki/ALSA) |  |  |  | 
| USB | USB 1.1 | Works |  | [uhci](https://wiki.gentoo.org/wiki/USB) |  |  |  | 
| USB | USB 2.0 | Works |  | [ehci](https://wiki.gentoo.org/wiki/USB) |  |  |  | 
| Bluetooth | Dell Wireless 360 Bluetooth | Works |  | [btusb](https://wiki.gentoo.org/wiki/Bluetooth) |  |  |  | 
| Firewire | 02 Micro FireWire | Works |  | [FireWire](https://wiki.gentoo.org/wiki/FireWire) |  |  |  | 
| PC-card | O2 Micro OZ601/6912/711E0 | Works |  | [yenta\_socket](https://wiki.gentoo.org/wiki/PC-Card) |  |  |  | 
| Serial port |  | Works |  | serial |  |  |  | 
| ACPI |  | Works |  | [ACPI](https://wiki.gentoo.org/wiki/ACPI) |  |  |  | 
| Fingerprint reader | SG5 Thomson Microelectronics | Works |  | [fprint](https://wiki.gentoo.org/wiki/Fingerprint_Reader) |  |  |  | 
| Sensors |  | Works |  | [coretemp](https://wiki.gentoo.org/wiki/Lm_sensors), [i8k](https://wiki.gentoo.org/wiki/I8k) |  |  |  | 
| Onboard | O2 Micro OZ776CCID | Works |  | [ccid](https://wiki.gentoo.org/wiki/PCSC-Lite) |  |  |  | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Docking station | Dell Latitude D Models | Works |  | [acpi\_dock](https://wiki.gentoo.org/wiki/Acpi_dock) |  |  | 

## See also

- [Power management](https://wiki.gentoo.org/wiki/Power_management) — describes methods to save energy for longer battery runtimes, a quieter computer, lower power bills, and an environmentally friendly impact.
