<!-- source: https://wiki.gentoo.org/wiki/Google_Chromebook_Pixel_(2013) | group: Gentoo Wiki (Main) | wiki-title: Google Chromebook Pixel (2013) -->
---
title: Google Chromebook Pixel (2013)
url: https://wiki.gentoo.org/wiki/Google_Chromebook_Pixel_(2013)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "7c066df5c9a39369"
license: CC BY-SA 4.0
---

# Google Chromebook Pixel (2013)

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This Chromebook went on sale in 2013 in two versions. The first version has a 32 GB SSD. The second version has a 64 GB SSD and a removable LTE modem. The SSD in both versions is soldered into the motherboard, so it is not upgradeable. Both versions have the same motherboard, so the first version also has the slot for the modem, but not the modem itself.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel Core i5-3427U (dual-core 1.8 GHz) | Works | N/A | N/A | N/A |  | 
| GPU | Intel HD Graphics 4000 | Works | N/A | N/A | N/A |  | 
| SSD | SanDisk SSD i100 (soldered on-board) | Works | N/A | N/A | N/A |  | 
| WiFi | Atheros AR5BMD22 | Works | N/A | N/A | N/A |  | 
| Touchpad | N/A | Works | N/A | N/A | N/A |  | 
| Touchscreen | Atmel mXT224SL | Works | N/A | N/A | N/A |  | 

### Accessories

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| LTE Modem | N/A | Not tested | N/A | N/A | N/A |  | 

## Installation

### Firmware

To install the UEFI firmware, the hardware write protection must first be disabled. To do this, it is necessary to open the case and remove the special screw according to [the manual](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/chromebook-pixel). After that, the [remaining steps](https://wiki.gentoo.org/wiki/Chromebook#Installation) must be performed.

### Kernel

KERNEL

```
Device Drivers  --->
  [*] Network device support  --->
    [*]   Wireless LAN  --->
      <*>   Atheros Wireless Cards  --->
        <*>   Atheros 802.11n wireless cards support
  Input device support  --->
    [*]   Touchscreens  --->
      <*>   Atmel mXT I2C Touchscreen 
  [*] Platform support for Chrome hardware  --->
    <*>   Chrome OS Laptop
```
### Emerge

FILE **`/etc/portage/make.conf`**

```
VIDEO_CARDS="intel i965"
INPUT_DEVICES="evdev synaptics"
```
