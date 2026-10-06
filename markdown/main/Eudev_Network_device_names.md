<!-- source: https://wiki.gentoo.org/wiki/Eudev/Network_device_names | group: Gentoo Wiki (Main) | wiki-title: Eudev/Network device names -->
---
title: Eudev/Network device names
url: https://wiki.gentoo.org/wiki/Eudev/Network_device_names
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: "3e21d979c9cfa3df"
license: CC BY-SA 4.0
---

# Eudev/Network device names

From Gentoo Wiki

\< [Eudev](https://wiki.gentoo.org/wiki/Special:MyLanguage/Eudev)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**



Network device names such as eth0 or wlan0 as provided by the kernel are normally changed on system boot (see dmesg) by the /lib/udev/rules.d/80-net-name-slot.rules udev rule.

To keep the classic naming this rule can be overwritten with an equally named empty file in the /etc/udev/rules.d directory:

`root #````
touch /etc/udev/rules.d/80-net-name-slot.rules
```
