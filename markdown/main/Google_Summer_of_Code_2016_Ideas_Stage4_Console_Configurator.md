<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas/Stage4_Console_Configurator | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2016/Ideas/Stage4 Console Configurator -->
---
title: Google Summer of Code/2016/Ideas/Stage4 Console Configurator
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas/Stage4_Console_Configurator
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "5724b26a8b675d25"
license: CC BY-SA 4.0
---

# Google Summer of Code/2016/Ideas/Stage4 Console Configurator

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This should be a console configuration and build tool for customizing a given stage3 tarball with user-selected packages and USE flags, and rebuilding into a deployable stage4 rootfs.

Basic Features:

- Similar to "make menuconfig" for the kernel or buildroot
- Input fields for packages, USE flags, bootloader, and kernel sources
  - use dependency resolution to verify selection, update make.conf
  - bootloader/kernel options should include "none"
- Select input stage3 and output/logging destination, save/load configuration
- Automated download, chroot, build, archive results
- Support current autobuilds, experimental, hardened on all current arches
- Use ncurses/slang/other, include internationalization support



| Contacts | Required Skills | 
|---|---|
|  |  |
