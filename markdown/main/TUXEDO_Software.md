<!-- source: https://wiki.gentoo.org/wiki/TUXEDO_Software | group: Gentoo Wiki (Main) | wiki-title: TUXEDO Software -->
---
title: TUXEDO Software
url: https://wiki.gentoo.org/wiki/TUXEDO_Software
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-04"
fingerprint: de503f5d48f73a56
license: CC BY-SA 4.0
---

# TUXEDO Software

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![](https://wiki.gentoo.org/images/thumb/b/bf/Screenshot_20220423-191112.png/300px-Screenshot_20220423-191112.png)

This article is a guide about installing TUXEDO's Linux drivers and Control Center on Gentoo Linux - software for vendor-specific TUXEDO hardware.

## Kernel

First enable these Kernel options<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> to make the compiling of [app-laptop/tuxedo-keyboard](https://packages.gentoo.org/packages/app-laptop/tuxedo-keyboard) and [sys-power/tuxedo-cc-wmi](https://packages.gentoo.org/packages/sys-power/tuxedo-cc-wmi) packages possible.

**Enable ACPI WMI in 6.1.31-gentoo**

## Drivers

It is possible to configure the keyboard via the [TUXEDO Control Center](https://wiki.gentoo.org/wiki/TUXEDO_Software#Control_Center). But effects, e. g. “Wave”, are not configurable in the UI yet.

Before rebooting, add this service:

`root #``rc-update add tccd default`
## Control Center

TUXEDO Computers provides an application to control various hardware components. A binary package exists in the Gentoo tree.

`root #``emerge --ask app-laptop/tuxedo-control-center-bin`
## See also

- [TUXEDO Aura 15 (Gen2)](<https://wiki.gentoo.org/wiki/TUXEDO_Aura_15_(Gen2)>) — a configurable Linux notebook from 2022.
- [TUXEDO InfinityBook Pro 15 (Gen10)](<https://wiki.gentoo.org/wiki/TUXEDO_InfinityBook_Pro_15_(Gen10)>) — a configurable Linux notebook from 2025.
