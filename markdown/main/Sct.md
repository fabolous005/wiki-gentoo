<!-- source: https://wiki.gentoo.org/wiki/Sct | group: Gentoo Wiki (Main) | wiki-title: Sct -->
---
title: sct
url: https://wiki.gentoo.org/wiki/Sct
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: "57f3b6d07d9bfbd0"
license: CC BY-SA 4.0
---

# sct

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**sct** (**s**creen **c**ontrol **t**emperature) is a simple tool for changing color temperature of a screen.

## Installation

### Emerge

`root #``emerge --ask x11-misc/sct`
## Usage

**sct** expects a value between **1000** and **10000** Kelvins:

`user $``sct color_temperature`
When provided with out-of-range value or run without arguments, temperature is set to **6500 K**.
