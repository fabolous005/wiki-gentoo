<!-- source: https://wiki.gentoo.org/wiki//etc/portage/license_groups | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/license groups -->
---
title: "/etc/portage/license_groups"
url: https://wiki.gentoo.org/wiki//etc/portage/license_groups
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-12"
fingerprint: e0019ce20f998d34
license: CC BY-SA 4.0
---

# /etc/portage/license\_groups

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


/etc/portage/license\_groups is a file containing groups of licenses that may be specified in the `[ACCEPT_LICENSE](https://wiki.gentoo.org/wiki//etc/portage/make.conf#ACCEPT_LICENSE)` variable. Refer to [GLEP 23](https://www.gentoo.org/glep/glep-0023.html) for further information.

## Format

- Comments begin with `#` (no inline comments).
- One group name, followed by list of licenses and nested groups.
- Nested groups are prefixed with the `@` symbol.

## Example

FILE **`/etc/portage/license_groups`****License groups example**

```
# The FSF-APPROVED group includes the entire GPL-COMPATIBLE group and more.
FSF-APPROVED @GPL-COMPATIBLE Apache-1.1 BSD-4 MPL-1.0 MPL-1.1
# The GPL-COMPATIBLE group includes all licenses compatible with the GNU GPL.
GPL-COMPATIBLE Apache-2.0 BSD BSD-2 GPL-2 GPL-3 LGPL-2.1 LGPL-3 X11 ZLIB
```
