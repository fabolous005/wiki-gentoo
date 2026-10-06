<!-- source: https://wiki.gentoo.org/wiki/Android/Root | group: Gentoo Wiki (Main) | wiki-title: Android/Root -->
---
title: Android/Root
url: https://wiki.gentoo.org/wiki/Android/Root
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-05"
fingerprint: bf072414fd433dc0
license: CC BY-SA 4.0
---

# Android/Root

[Android](https://wiki.gentoo.org/wiki/Android)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

# Android Rooting

The purpose of this project is to develop a framework for rooting android devices using free/libre and open source software.

A majority of android devices are locked down and therefore cannot be updated with community-supported patches and upgrades.

All major/well-known rooting apps are [proprietary](https://en.wikipedia.org/wiki/Proprietary_software) and therefore cannot be entirely trusted.

## Introduction

[rage](https://wiki.gentoo.org/wiki/User:Rage): One thing has been troubling me about Android development.
Installing a custom recovery or rom is possible using fastboot.
However, there seems to be no open source application for gaining root access on a device.

[0sm0sis](https://forum.xda-developers.com/member.php?u=4544860): As for rooting/bootloader unlocking using vulnerabilities, that's a whole other game.
You basically need an escalation vulnerability to get a root prompt and then, if possible, 
unlock the bootloader from there, and that's why they generally remain closed-source,
since they don't want to disclose their vulnerabilities and risk the OEM patching them before they can spread it.

[heroxbd](https://wiki.gentoo.org/wiki/User:Heroxbd): Yeah, that is an unfortunate status quo.

[rage](https://wiki.gentoo.org/wiki/User:Rage): Hmm, so I get what you're saying about the vulnerabilities, but using a proprietary utility to achieve root access, for the purpose of liberating my device from proprietary software seems a bit backwards to me.

[rage](https://wiki.gentoo.org/wiki/User:Rage): A big problem that I have is that the more popular devices usually have many utilities, while less popular ones don't have anything at all. If there were a framework that could fetch exploits for a given device then it would be trivial to add new devices to the tool.

[rage](https://wiki.gentoo.org/wiki/User:Rage): Maybe we could write one as part of our toolchain?

[heroxbd](https://wiki.gentoo.org/wiki/User:Heroxbd): That would be very useful. Please go ahead.
