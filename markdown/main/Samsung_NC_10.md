<!-- source: https://wiki.gentoo.org/wiki/Samsung_NC_10 | group: Gentoo Wiki (Main) | wiki-title: Samsung NC 10 -->
---
title: Samsung NC 10
url: https://wiki.gentoo.org/wiki/Samsung_NC_10
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "43bb2eb6a92ab5cf"
license: CC BY-SA 4.0
---

# Samsung NC 10

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is an article about running Gentoo on an Samsung NC 10 series laptop.

## Laptop Specifications

Hardware specs may vary.

## Install driver packages

`root #``emerge --ask media-libs/libva-intel-driver x11-apps/intel-gpu-tools sys-apps/915resolution x11-drivers/xf86-video-intel`
## Problems

The i915 card is really unique in X and boot behaviour.

### Problems with kernel 3.2

Use these boot settings for X:

vga=0x315 acpi=force i915.modeset=1 i915.lvds\_use\_ssc=0 i915.i915\_enable\_rc6=1

### Problems with kernel 3.4

Use these boot settings for X:

vga=0x315 acpi=force pcie\_aspm=force i915.modeset=1 i915.i915\_enable\_fbc=1 i915.lvds\_use\_ssc=0 i915.i915\_enable\_rc6=1 i915.lvds\_downclock=1 drm.vblankoffdelay=1 irqpoll
