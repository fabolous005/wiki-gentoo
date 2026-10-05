<!-- source: https://wiki.gentoo.org/wiki/QEMU/Guest/HPPA | group: Gentoo Wiki (Main) | wiki-title: QEMU/Guest/HPPA -->
---
title: QEMU/Guest/HPPA
url: https://wiki.gentoo.org/wiki/QEMU/Guest/HPPA
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: a29d267789bb11ec
license: CC BY-SA 4.0
---

# QEMU/Guest/HPPA

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A collection of handy hints for HPPA Gentoo users.

## QEMU install

First enable HPPA system support in [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu):

**`/etc/portage/package.use/qemu`**

Then rebuild [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu):

`root #``emerge --ask app-emulation/qemu`
### Boot installcd

First download the hppa installcd from your local mirror at [https://gentoo.org/downloads/mirrors/](https://gentoo.org/downloads/mirrors/)

then, create an image to use as a hard drive source:

`user $``qemu-img create -f qcow2 gentoo-hppa.qcow2 30G`
Next, you can boot the installcd with:

`user $``qemu-system-hppa -drive file=gentoo-hppa.qcow2 -drive file=<Gentoo ISO location>,media=cdrom -boot order=d -nographic -serial mon:stdio -accel tcg,thread=multi -smp cpus=2`
### Stage3

qemu-system-hppa as of 2024-08-24. only supports 32bit installs so use the hppa-1.1 stage tarballs.
