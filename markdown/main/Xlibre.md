<!-- source: https://wiki.gentoo.org/wiki/Xlibre | group: Gentoo Wiki (Main) | wiki-title: Xlibre -->
---
title: Xlibre
url: https://wiki.gentoo.org/wiki/Xlibre
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-07"
fingerprint: ebf11f4899cfbfc7
license: CC BY-SA 4.0
---

# Xlibre

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Xlibre** is a fork of the [Xorg](https://wiki.gentoo.org/wiki/Xorg) server which describes itself as *lots of code cleanups and enhanced functionality*. It was forked from Xorg after the Xorg developers claimed the author's many recent contributions suffer from poor quality control and numerous regressions. The *enhanced functionality* is not described or listed.

## Installation

Xlibre is not available in the main Gentoo repository. It can be obtained from an unofficial overlay operated by the Xlibre developers:

To add the `xlibre` overlay, run:

`root #````
emerge -va app-eselect/eselect-repository
```
`root #````
eselect repository enable xlibre
```
`root #````
emaint sync -r xlibre
```
xlibre-server is not compatible with xorg-server. When switching to this overlay first fetch the XLibre server package and then remove the X.Org Server and drivers packages.

`root #````
emerge -f x11-base/xlibre-server
```
`root #````
emerge -C x11-base/xorg-server
```
`root #````
emerge -C x11-base/xorg-drivers
```
`root #````
emerge x11-base/xlibre-server
```
`root #````
emerge @x11-module-rebuild
```
`root #````
emerge @preserved-rebuild
```
Use following portage setting to install the software:

`root #``echo "*/*::xlibre ~`*arch*" > /etc/portage/package.accept_keywords/xlibre
## Nvidia

Proprietary Nvidia drivers will not work due to ABI changes. The following workaround is available:

**`/usr/share/X11/xorg.conf.d/10-nvidia.conf`**

```
Section "ServerFlags"
    Option "IgnoreABI" "1"
EndSection
```
## Reporting bugs

Bugs must be reported to [https://github.com/X11Libre/ports-gentoo/issues](https://github.com/X11Libre/ports-gentoo/issues)
