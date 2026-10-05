<!-- source: https://wiki.gentoo.org/wiki/Hwinfo | group: Gentoo Wiki (Main) | wiki-title: Hwinfo -->
---
title: hwinfo
url: https://wiki.gentoo.org/wiki/Hwinfo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-13"
fingerprint: f28d7d2b42069317
license: CC BY-SA 4.0
---

# hwinfo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Hardware Information Tool (**hwinfo**) is a small utility created by OpenSUSE to gather information on system hardware.

## Installation

### Emerge

hwinfo can be installed with a simple emerge:

`root #``emerge --ask sys-apps/hwinfo`
## Configuration

Since the tool is so lightweight, configuration is not necessary.

## Usage

To probe for hardware information simply issue:

`root #``hwinfo`
To probe for all hardware information use:

`root #``hwinfo +all`
This usually takes longer, but provides oodles of information.

To shorten information to a more human-friendly format, add the `--short` option to the command:

`root #``hwinfo --short all`
To specify a log file for hwinfo to write, use `log=`. This is different from standard redirection, but should be used for hwinfo:

`root #``hwinfo log=hardware.txt all`
## See also

- [Hardware detection](https://wiki.gentoo.org/wiki/Hardware_detection) — lists and describes utilities used to detect and provide information on hardware.
- [Lspci](https://wiki.gentoo.org/wiki/Lspci) — contains various utilities dealing with the [PCI bus](https://en.wikipedia.org/wiki/Peripheral_Component_Interconnect) (primarily lspci).
- [Usbutils](https://wiki.gentoo.org/wiki/Usbutils) — a collection various utilities for querying the the Universal Serial Bus (USB).
