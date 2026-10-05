<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/Portage_powered_Android | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2018/Ideas/Portage powered Android -->
---
title: Google Summer of Code/2018/Ideas/Portage powered Android
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/Portage_powered_Android
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-11"
fingerprint: "9f1ee0299a956cd1"
license: CC BY-SA 4.0
---

# Google Summer of Code/2018/Ideas/Portage powered Android

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Android custom ROM development has been based on cross-compilation and flashing whole partitions. This paradigm has been serving well for the embedded system developments. But as the performance of personal mobile devices boost, it becomes feasible and desirable to introduce package management like personal computers. Package manangement will make software installation and update reliable, reproducible, incremental and convienent.

This project aims to introduce Gentoo's prestiges package manager, portage, to manage Android software stack.  We are going to use the [Gentoo on Android](https://wiki.gentoo.org/wiki/Project:Android) as a starting point.  Starting from the GNU userland provided, we are going to work with the [LineageOS](https://lineageos.org) build system based on the [Android Open Source Project](http://source.android.com), to write ebuilds for the individual components.  Starting from the linux kernel first, the Android will be reproduced from bottom up.



| Contacts | Required Skills | 
|---|---|
|  |  |
