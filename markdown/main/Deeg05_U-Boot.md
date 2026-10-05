<!-- source: https://wiki.gentoo.org/wiki/Deeg05:U-Boot | group: Gentoo Wiki (Main) | wiki-title: Deeg05:U-Boot -->
---
title: Deeg05:U-Boot
url: https://wiki.gentoo.org/wiki/Deeg05:U-Boot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-31"
fingerprint: "8fdb1c1a7b91f971"
license: CC BY-SA 4.0
---

# Deeg05:U-Boot

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document describes how to compile U-Boot for linux-sunxi devices

## Prerequisites

- crossdev toolchain for target (for example, armv7a-unknown-linux-gnueabihf is used for cubietruck)
- python:2.7 python swig dev-python/pip dev-python/setuptools bc dev-python/python-distutils-extra dtc git

## U-Boot compilation

Prepare working environment:

`user $``cd u-boot-sunxi``user $``export CROSS_COMPILE="armv7a-unknown-linux-gnueabihf-"``user $``virtualenv -p /usr/bin/python2.7 venv``user $``source venv/bin/activate`
Remove line from scripts/dtc/dtc-lexer.lex.c

FILE **`scripts/dtc/dtc-lexer.l`****dtc-lexer.l**

Compile:

`user $``make Cubietruck_config``user $``make clean``user $``make all`
## U-Boot installation

dd u-boot-sunxi-with-spl.bin into SD card you're going to be using for your machine

`root #``dd if=u-boot-sunxi-with-spl.bin of=/dev/mmcblk0 bs=1024 seek=8`
