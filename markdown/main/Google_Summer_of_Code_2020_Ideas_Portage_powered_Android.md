<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/Portage_powered_Android | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2020/Ideas/Portage powered Android -->
---
title: Google Summer of Code/2020/Ideas/Portage powered Android
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/Portage_powered_Android
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-02-16"
fingerprint: bf9ee1058bd3e1cd
license: CC BY-SA 4.0
---

# Google Summer of Code/2020/Ideas/Portage powered Android

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Android custom ROM development has been based on cross-compilation and flashing whole partitions. This paradigm has been serving well for the embedded system developments. But as the performance of personal mobile devices boost, it becomes feasible and desirable to introduce package management like personal computers. In addition, with the introduction of Project Treble in Android 8.0 Oreo, device drivers are managed separately, opening more possibilities of operating system customization. Package manangement will make software installation and update reliable, reproducible, incremental and convienent.

This project aims to introduce Gentoo's prestiges package manager, portage, to manage Android software stack. Taking [KirenaHoro's work](https://jsteward.moe/gsoc-2018-final-report.html) as the starting point, you are going to develop a framework to drive the AOSP/LineageOS build system into a set of portage ebuilds.

In addition, this project targets an update of [Project:Android](https://wiki.gentoo.org/wiki/Project:Android) to work from within an Android app without rooting the device, to offer Gentoo to diverted audience and ease the adoption of GNU userlands on Android devices. For this part, a normal (unrooted, bootlocked phone) is sufficient. Bumping and upstreaming [gentooandroid.sf.net's](https://sourceforge.net/projects/gentooandroid/files/packages/packages/portage.diff/download) [patches](https://sourceforge.net/projects/gentooandroid/files/sys-app_portage-current-HEAD_patch/download) to bootstrap-prefix.sh and in $PORTDIR is a possible pathway. A suggested list of progressive steps would be to make bootstrap-prefix.sh work on a gentoo box, after removing : /usr/bin/python at step 1, /tmp at step 2, /etc/passwd at step 3. Step 4 would be to target Termux on vanilla Android that lacks /bin/sh, step 5 to take out /usr/bin/perl for LaTeX, step 6 /dev/tty for Xvfb.



| Contacts | Required Skills | 
|---|---|
|  |  |
