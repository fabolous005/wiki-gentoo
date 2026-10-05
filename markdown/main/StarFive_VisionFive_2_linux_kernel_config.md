<!-- source: https://wiki.gentoo.org/wiki/StarFive_VisionFive_2/linux_kernel_config | group: Gentoo Wiki (Main) | wiki-title: StarFive VisionFive 2/linux kernel config -->
---
title: StarFive VisionFive 2/linux kernel config
url: https://wiki.gentoo.org/wiki/StarFive_VisionFive_2/linux_kernel_config
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-19"
fingerprint: "56b39f56b1e2fbf4"
license: CC BY-SA 4.0
---

# StarFive VisionFive 2/linux kernel config

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page lists some working configurations for the Linux kernel on RISC-V. Only set options are mentioned.

## Linux 6.12 (tested with 6.12.4)

- Flavour: [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources)
- Notes: No loadable modules support (no ramdisk), monolitic image

FILE **`config-linux-6.12`**
