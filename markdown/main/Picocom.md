<!-- source: https://wiki.gentoo.org/wiki/Picocom | group: Gentoo Wiki (Main) | wiki-title: Picocom -->
---
title: Picocom
url: https://wiki.gentoo.org/wiki/Picocom
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-20"
fingerprint: f65655faab1d7bdd
license: CC BY-SA 4.0
---

# Picocom

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Picocom** is a minimal terminal emulation program similar to minicom.

## Installation

### Emerge

`root #``emerge --ask net-dialup/picocom`
## Usage

Command line utility, for example:

`user $``picocom -b 19600 /dev/ttyUSB0`
This opens the serial comms port ttyUSB0 at a baudrate of 19600.

Note: The user will require permission to access /dev/ttyUSB0

To exit, use `Ctrl`+`a`, then `Ctrl`+`x`.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-dialup/picocom`
