<!-- source: https://wiki.gentoo.org/wiki/OpenRC/Event_driven | group: Gentoo Wiki (Main) | wiki-title: OpenRC/Event driven -->
---
title: OpenRC/Event driven
url: https://wiki.gentoo.org/wiki/OpenRC/Event_driven
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-06-29"
fingerprint: b914fbeec3cd0a4e
license: CC BY-SA 4.0
---

# OpenRC/Event driven

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of **September, 2014**, this article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

In the history of init system redesign, [upstart](http://upstart.ubuntu.com/) made a bold claim<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> that it is revolutionary over the dependency based systems. We credit the event driven concept upstart has been proved as a init system.

In this article we are going to show that OpenRC can get to know **Dynamic Nature of Linux** via external tools. There is a philosophical difference between OpenRC and upstart. OpenRC's approach to the init system is to combine many small, independent tools together while Upstart's approach is to integrate every wanted feature into a single program.

## Example: hotplug iPhone for tethering

This example makes use of [sys-fs/udev](https://packages.gentoo.org/packages/sys-fs/udev) and [app-pda/ipheth-pair](https://packages.gentoo.org/packages/app-pda/ipheth-pair). Details [here](https://wiki.gentoo.org/wiki/Iphone_USB_tethering#udev_trigger)
