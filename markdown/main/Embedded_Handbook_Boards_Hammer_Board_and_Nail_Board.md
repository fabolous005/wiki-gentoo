<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Hammer_Board_and_Nail_Board | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Boards/Hammer Board and Nail Board -->
---
title: Embedded Handbook/Boards/Hammer Board and Nail Board
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Hammer_Board_and_Nail_Board
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-15"
fingerprint: a2ca906396a64ddf
license: CC BY-SA 4.0
---

# Embedded Handbook/Boards/Hammer Board and Nail Board

From Gentoo Wiki

\< [Embedded Handbook](https://wiki.gentoo.org/wiki/Embedded_Handbook) | [Boards](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Little-endian armv4l board.

## Nail Board specifications

Board specifications:

## /proc/cpuinfo

CPU info:

FILE **`/proc/cpuinfo`**

```
Processor	: ARM920T rev 0 (v4l)
BogoMIPS	: 101.17
Features	: swp half thumb 
CPU implementer	: 0x41
CPU architecture: 4T
CPU variant	: 0x1
CPU part	: 0x920
CPU revision	: 0
Cache type	: write-back
Cache clean	: cp15 c7 ops
Cache lockdown	: format A
Cache format	: Harvard
I size		: 16384
I assoc		: 64
I line length	: 32
I sets		: 8
D size		: 16384
D assoc		: 64
D line length	: 32
D sets		: 8
Hardware	: TCT_HAMMER
Revision	: 0000
Serial		: 0000000000000000
```
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
