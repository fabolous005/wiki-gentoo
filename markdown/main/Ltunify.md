<!-- source: https://wiki.gentoo.org/wiki/Ltunify | group: Gentoo Wiki (Main) | wiki-title: Ltunify -->
---
title: Ltunify
url: https://wiki.gentoo.org/wiki/Ltunify
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-02-04"
fingerprint: "2f259fd8c0af28f4"
license: CC BY-SA 4.0
---

# Ltunify

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Many Logitech input peripherals - mostly mouse and keyboards, but also numpads, presenters, trackballs and touchpads - come with a unique USB receiver. If your peripherals and receiver are not paired from fabric, but support the [Logitech Unifying technology](http://support.logitech.com/en_us/software/unifying), you can still pair them using the [app-misc/ltunify](https://packages.gentoo.org/packages/app-misc/ltunify) package.

The package is available for **\~amd64** and **\~x86** only.

## Installation

### Emerge

`root #``emerge --ask app-misc/ltunify`
## Usage

The usage is trivial.

`root #``ltunify list`
shows the devices actually paired.

`root #``ltunify unpair idx`
removes a listed paired devices that doesn't exist anymore.

`root #``ltunify pair`
adds a device simply asking to switch it off and on.

`root #``ltunify --help`
shows a complete help.
