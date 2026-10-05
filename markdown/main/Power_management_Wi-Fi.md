<!-- source: https://wiki.gentoo.org/wiki/Power_management/Wi-Fi | group: Gentoo Wiki (Main) | wiki-title: Power management/Wi-Fi -->
---
title: Power management/Wi-Fi
url: https://wiki.gentoo.org/wiki/Power_management/Wi-Fi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-24"
fingerprint: "66d1db4cc9d272b6"
license: CC BY-SA 4.0
---

# Power management/Wi-Fi

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of Wi-Fi devices.

## Configuration

### Udev

Make the following [udev](https://wiki.gentoo.org/wiki/Udev) rule file to automate power management:

FILE **`/etc/udev/rules.d/10-my-wifi-power.rules`**
