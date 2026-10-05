<!-- source: https://wiki.gentoo.org/wiki/Power_management/PCI | group: Gentoo Wiki (Main) | wiki-title: Power management/PCI -->
---
title: Power management/PCI
url: https://wiki.gentoo.org/wiki/Power_management/PCI
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-17"
fingerprint: "4641e9dce9f3e0a4"
license: CC BY-SA 4.0
---

# Power management/PCI

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of PCI devices.

## Configuration

### Kernel

KERNEL

Device Drivers --->
  \[\*\] PCI support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PCI\</code> to find this item.
    \[\*\] PCI Express ASPM control [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PCIEASPM\</code> to find this item.
    Default ASPM policy (Powersave) ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PCIEASPM\_POWERSAVE\</code> to find this item.

### Udev

Make the following [udev](https://wiki.gentoo.org/wiki/Udev) rule file to automate power management:

FILE **`/etc/udev/rules.d/10-my-pci-power.rules`**
