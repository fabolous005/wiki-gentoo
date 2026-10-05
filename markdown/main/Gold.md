<!-- source: https://wiki.gentoo.org/wiki/Gold | group: Gentoo Wiki (Main) | wiki-title: Gold -->
---
title: Gold
url: https://wiki.gentoo.org/wiki/Gold
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-14"
fingerprint: b955bd9813a33f0f
license: CC BY-SA 4.0
---

# Gold

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

GNU **gold** is a linker intended as a replacement for the ld.bfd linker.

The two installed linkers are available directly as ld.bfd and ld.gold respectively. Additionally, the default linker is also installed as ld - and this binary is used by compilers.

## Installation

Gold can be enabled by setting the `gold` USE flag for [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils). Additionally, setting `default-gold` will make ld.gold the default linker.

**`/etc/portage/package.use/gold`**

```
# enable gold and set it as default
sys-devel/binutils      gold default-gold
```
After setting the USE flags, re-install binutils:

`root #``emerge --ask --changed-use --deep --oneshot --verbose sys-devel/binutils`
## See also

- [Mold](https://wiki.gentoo.org/wiki/Mold) — a linker that aims to provide drop-in compatibility with existing Unix linkers.
