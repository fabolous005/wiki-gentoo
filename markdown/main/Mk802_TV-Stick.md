<!-- source: https://wiki.gentoo.org/wiki/Mk802_TV-Stick | group: Gentoo Wiki (Main) | wiki-title: Mk802 TV-Stick -->
---
title: Mk802 TV-Stick
url: https://wiki.gentoo.org/wiki/Mk802_TV-Stick
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-04"
fingerprint: "96333d1544a8266b"
license: CC BY-SA 4.0
---

# Mk802 TV-Stick

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Revive an Allwinner TV-Stick with Mainline Kernel (plus a small patch)

Use RCN multiplatform repo for now, or just grab the patches from patches/sunxi on the latest branch (v4.7.x) and apply them to your kernel source.

Follow RCN wiki steps for building from his repo and deploying u-boot; there is also an mk802\_defconfig in the above patch dir (recommended). The A20 OLinuXino-LIME steps also work for mk802, just replace the machine name.

Example: [https://eewiki.net/display/linuxonarm/A20-OLinuXino-LIME](https://eewiki.net/display/linuxonarm/A20-OLinuXino-LIME)

The device tree patches add additional clocks and hardware (from the upstream sunxi-next branch) and set the OTG (mini-USB) port to host mode.  With the port set to OTG mode in the device tree, the kernel module will not switch to host mode, so if you need a real USB 2.0 host port, then you need these patches.  Note the full-size host port on the stick is OHCI/USB 1.0 **only** so a network adapter will be annoyingly slow.  Use the mini-port and things are **much** less annoying.  Also note that this whole thing requires a TV stick with a micro-SDCard slot.
