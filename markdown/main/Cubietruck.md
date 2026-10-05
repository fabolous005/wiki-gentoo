<!-- source: https://wiki.gentoo.org/wiki/Cubietruck | group: Gentoo Wiki (Main) | wiki-title: Cubietruck -->
---
title: Cubietruck
url: https://wiki.gentoo.org/wiki/Cubietruck
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-10"
fingerprint: "71d2022281bc56cc"
license: CC BY-SA 4.0
---

# Cubietruck

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Cubietruck**

![Cubietruck in ewell case](https://wiki.gentoo.org/images/thumb/9/9a/Paperbox.jpg/265px-Paperbox.jpg)

**Resources**

Cubietruck is the third generation of the famous Cubieboard, and is the most full-featured board to date. It has a dual-core ARM SoC (Allwinner A20, ARMv7a) with 2 GB DDR3 RAM and uses a microSD(HC) card or 8 GB NAND for storage. Also, there are TSD versions out there.

## Hardware

### Component status

| Device | Make/model | Works | Notes | 
|---|---|---|---|
| SoC |  |  |  | 
| Video | Mali 400 MP2 |  |  | 
| Audio |  |  |  | 
| Ethernet | Realtek RTL8211E |  |  | 
| 802.11b/g/n WLAN | Ampak AP6210 |  | brcm/brcmfmac43362-sdio.cubietech,cubietruck.txt of linux-firmware is required | 
| USB Controller (Host) |  |  |  | 
| USB Controller (OTG) |  |  |  | 
| NAND |  |  |  | 
| SATA |  |  |  | 
| microSD |  |  |  | 
| Hardware monitoring |  |  |  | 
| GPIO |  |  |  | 
| LED control (blue, green, orange, white) |  |  | Manipulate through /sys/class/leds/ | 
| IR (Infrared) input |  |  |  | 

## Installation

For installation the u-boot environment and kernel must be built and then written to the microSD card.
