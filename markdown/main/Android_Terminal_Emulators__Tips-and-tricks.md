<!-- source: https://wiki.gentoo.org/wiki/Android/Terminal_Emulators_(Tips-and-tricks) | group: Gentoo Wiki (Main) | wiki-title: Android/Terminal Emulators (Tips-and-tricks) -->
---
title: Android/Terminal Emulators (Tips-and-tricks)
url: https://wiki.gentoo.org/wiki/Android/Terminal_Emulators_(Tips-and-tricks)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-05"
fingerprint: "379db6cecdcb77fe"
license: CC BY-SA 4.0
---

# Android/Terminal Emulators (Tips-and-tricks)

From Gentoo Wiki

\< [Android](https://wiki.gentoo.org/wiki/Android)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## File System Layout

This directory is universal between all Android devices.

`root #``cd "/data/data/${_PN}"`
## Portage

### Termux

`$``python setup.py install --system-prefix /data/data/com.termux/files/usr --sysconfdir /data/data/com.termux/files/usr/etc`
