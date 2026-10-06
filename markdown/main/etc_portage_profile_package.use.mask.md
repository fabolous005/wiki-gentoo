<!-- source: https://wiki.gentoo.org/wiki//etc/portage/profile/package.use.mask | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/profile/package.use.mask -->
---
title: "/etc/portage/profile/package.use.mask"
url: https://wiki.gentoo.org/wiki//etc/portage/profile/package.use.mask
hostname: gentoo.org
sitename: "/etc/portage/profile/package.use.mask"
date: "2025-01-19"
fingerprint: "6fb123d9ed25fefd"
license: CC BY-SA 4.0
---

# /etc/portage/profile/package.use.mask

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The /etc/portage/profile/package.use.mask file contains per-package USE flag masks.

## Format

- Comments begin with `#` (no inline comments).
- One DEPEND atom per line with space-delimited USE flags.

## Example

FILE **`/etc/portage/profile/package.use.mask`****Per-package USE flag masks example**

```
# Mask docs for GTK 2.x
=x11-libs/gtk+-2* gtk-doc
# Unmask mysql support for QT
dev-qt/qtbase -mysql
```
