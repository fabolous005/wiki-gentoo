<!-- source: https://wiki.gentoo.org/wiki/Yuzu | group: Gentoo Wiki (Main) | wiki-title: Yuzu -->
---
title: Yuzu
url: https://wiki.gentoo.org/wiki/Yuzu
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-30"
fingerprint: "46b13845891af86d"
license: CC BY-SA 4.0
---

# Yuzu

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

tuzu is an experimental open-source emulator for the Nintendo Switch from the creators of Citra. Yuzu enables to play your Nintendo Switch games on your PC. Yuzu recommends you to always use latest build.

## Add guru ebuild repository

Visit [Adding the GURU repository](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users#Adding_the_GURU_repository) page

Do not forget to sync your repositories

`root #``emerge --sync guru`
## Compile yuzu

`root #``emerge -av '=games-emulation/yuzu-9999'`
