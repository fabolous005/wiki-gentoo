<!-- source: https://wiki.gentoo.org/wiki/Power_management | group: Gentoo Wiki (Main) | wiki-title: Power management -->
---
title: Power management
url: https://wiki.gentoo.org/wiki/Power_management
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-31"
fingerprint: a6d2306ec0b0bbe0
license: CC BY-SA 4.0
---

# Power management

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes methods to save energy for longer battery runtimes, a quieter computer, lower power bills, and an environmentally friendly impact.

## Configuration

### UEFI

The [UEFI](https://wiki.gentoo.org/wiki/UEFI) or [BIOS](https://wiki.gentoo.org/wiki/BIOS) might provide settings to save power, however, the machine must reboot to apply changes this way. It's better to make changes to the kernel and/or make use of userspace utilities as (most) changes in the machine's firmware can be overridden and done without rebooting.

Changes include, but not limited to enabling/disabling:

- Processor cores
- [USB](https://wiki.gentoo.org/wiki/USB) ports
- [Ethernet](https://wiki.gentoo.org/wiki/Ethernet) network devices
- [Wireless](https://wiki.gentoo.org/wiki/Wi-Fi) network devices
- [Sound cards](https://wiki.gentoo.org/wiki/Category:Sound_devices)
- [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) controllers
- Serial ports
- Parallel ports

### Kernel and Userspace

Below are links to sub-articles that explain what changes to make to increase power savings; they include changes to the kernel and use userspace utilities.

### Power modes

## See also

- [PowerTOP](https://wiki.gentoo.org/wiki/PowerTOP) — a Linux utility that can monitor and display a system's electrical power usage.

## External resources

- [https://www.linux.com/news/power-saving-linux](https://www.linux.com/news/power-saving-linux) - An article that provides generic explanations and advice for power saving on Linux.
