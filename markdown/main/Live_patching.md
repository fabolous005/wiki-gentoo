<!-- source: https://wiki.gentoo.org/wiki/Live_patching | group: Gentoo Wiki (Main) | wiki-title: Live patching -->
---
title: Live patching
url: https://wiki.gentoo.org/wiki/Live_patching
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-19"
fingerprint: de032d1ec1c337e9
license: CC BY-SA 4.0
---

# Live patching

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Kernel live patching is an 'update-and-coming' kernel feature being developed by a few corporate Linux companies. Several companies have open sourced their development efforts, making it possible to bring kernel live patching to Gentoo.

A note of caution: Kernel live patching is risky. Count on hard freezing or panics to become normal...

If at all possible, it's recommended to instead pursue making kernel upgrades as automated and painless as possible, like by using a [Distribution Kernel](https://wiki.gentoo.org/wiki/Distribution_Kernel) which builds like a regular package.

## Installation

### Kernel

The Linux kernel must be version 4.0 or higher in order to have `LIVEPATCH` support.[\[1\]](https://wiki.gentoo.org#cite_note-1)

**Enable`CONFIG_LIVEPATCH` support**

## Available software

Here are some live patch packages available in Gentoo:

| Name | Package | Homepage | Description | 
|---|---|---|---|
| [kpatch](https://wiki.gentoo.org/wiki/Kpatch) | [sys-kernel/kpatch](https://packages.gentoo.org/packages/sys-kernel/kpatch) | [https://github.com/dynup/kpatch](https://github.com/dynup/kpatch) | Dynamic kernel patching for Linux. | 
| ksplice | N/A | [http://www.ksplice.com/](http://www.ksplice.com/) | Rebootless Linux kernel security updates. Absorbed by Oracle in 2011 and available only by paid support. The 2011 version can be [found on GitHub](https://github.com/jirislaby/ksplice). |
