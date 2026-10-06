<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/ACME_SYSTEMS_Netus_G20 | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Boards/ACME SYSTEMS Netus G20 -->
---
title: Embedded Handbook/Boards/ACME SYSTEMS Netus G20
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/ACME_SYSTEMS_Netus_G20
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-15"
fingerprint: f32eaa157ba3578
license: CC BY-SA 4.0
---

# Embedded Handbook/Boards/ACME SYSTEMS Netus G20

From Gentoo Wiki

\< [Embedded Handbook](https://wiki.gentoo.org/wiki/Embedded_Handbook) | [Boards](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Netus G20 (ARMv5TE) from ACME SYSTEMS.

## Documentation

The [Netus G20](http://netus.acmesystems.it/doku.php) is a 4x4cm Linux ready core engine based on the Atmel(TM) AT91SAM9G20 (ARMv5TE) and sold by [ACME SYSTEMS](http://acmesystems.it/). It is supported on Gentoo thanks to the vendor and in special to [Davide Cantaluppi](http://netus.kdev.it/) who maintains it. In fact, Gentoo is the default OS shipped with this device.

## ACME SYSTEMS Netus G20 specifications

Board specifications:

## /proc/cpuinfo

CPU info:

FILE **`/proc/cpuinfo`**

```
netusg20 / # cat /proc/cpuinfo
Processor    : ARM926EJ-S rev 5 (v5l)
BogoMIPS    : 197.83
Features    : swp half thumb fastmult edsp java
CPU implementer    : 0x41
CPU architecture: 5TEJ
CPU variant    : 0x0
CPU part    : 0x926
CPU revision    : 5
Hardware    : Atmel AT91SAM9G20-EK
Revision    : 0000
Serial        : 0000000000000000
```
## dmesg

Kernel messages:

## External resources
