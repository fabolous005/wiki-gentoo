<!-- source: https://wiki.gentoo.org/wiki/Full_Source_Bootstrap | group: Gentoo Wiki (Main) | wiki-title: Full Source Bootstrap -->
---
title: Full Source Bootstrap
url: https://wiki.gentoo.org/wiki/Full_Source_Bootstrap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-24"
fingerprint: "65d33cd1d36e3cc3"
license: CC BY-SA 4.0
---

# Full Source Bootstrap

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**[Full Source Bootstrap](https://guix.gnu.org/manual/en/html_node/Bootstrapping.html)** refers to building everyting from source with minimal binaries.

## Preparing

First, download from fosslinux:

`user $``cd live-bootstrap`
Then download the required files:

`user $``./download-distfiles.sh`
If this does not work, try downloading from [link](https://github.com/fosslinux/live-bootstrap/wiki/Mirrors):

Now bootstrap from scratch:

`root #``./rootfs.py -c --external-sources --cores $(nproc)`
