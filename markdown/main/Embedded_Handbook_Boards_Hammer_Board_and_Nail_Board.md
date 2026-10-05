<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Hammer_Board_and_Nail_Board | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Boards/Hammer Board and Nail Board -->
---
title: Embedded Handbook/Boards/Hammer Board and Nail Board
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Hammer_Board_and_Nail_Board
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-15"
fingerprint: a2ee9662b4ae6dff
license: CC BY-SA 4.0
---

# Embedded Handbook/Boards/Hammer Board and Nail Board

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Little-endian armv4l board.

## Nail Board specifications

Board specifications:

## /proc/cpuinfo

CPU info:

FILE **`/proc/cpuinfo`**

## Cross compile preparation

Setup uClibc:

`root #````
echo '>=cross-arm-softfloat-linux-uclibc/gcc-4' >> /etc/portage/package.mask
```
`root #````
echo 'dev-embedded/openocd ft2232 ftdi' >> /etc/portage/package.use
```
`root #````
modprobe ftdi_sio
```
`root #````
emerge openocd
```
`root #````
ACCEPT_KEYWORDS="~*" emerge crossdev
```
`root #````
crossdev arm-softfloat-linux-uclibc
```
Setup uClibc and EABI:

`root #````
echo '>=cross-armv4l-softfloat-linux-uclibceabi/gcc-4' >> /etc/portage/package.mask
```
`root #````
echo 'dev-embedded/openocd ft2232 ftdi' >> /etc/portage/package.use
```
`root #````
modprobe ftdi_sio
```
`root #````
emerge openocd
```
`root #````
ACCEPT_KEYWORDS="~*" emerge crossdev
```
`root #````
crossdev armv4tl-softfloat-linux-uclibceabi
```
## External resources
