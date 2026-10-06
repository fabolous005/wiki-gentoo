<!-- source: https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles/license_groups | group: Gentoo Wiki (Main) | wiki-title: /var/db/repos/gentoo/profiles/license groups -->
---
title: "/var/db/repos/gentoo/profiles/license_groups"
url: https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles/license_groups
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-06-05"
fingerprint: e4079ce24f99cd34
license: CC BY-SA 4.0
---

# /var/db/repos/gentoo/profiles/license\_groups

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/var/db/repos/gentoo/profiles/license\_groups** contains  groups  of  licenses  that  may  be  specified in the `ACCEPT_LICENSE` variable (see make.conf(5)). Refer to [GLEP 23](https://www.gentoo.org/glep/glep-0023.html) for further information.

## Format

- Comments begin with `#` (no inline comments).
- One group name, followed by list of licenses and nested groups.
- Nested groups are prefixed with the `@` symbol.

## Examples

The FSF-APPROVED group includes the entire GPL-COMPATIBLE group and more:

FILE **`/var/db/repos/gentoo/profiles/license_groups`****license\_groups example 1**

```
FSF-APPROVED @GPL-COMPATIBLE Apache-1.1 BSD-4 MPL-1.0 MPL-1.1
```
The GPL-COMPATIBLE group includes all licenses compatible with the GNU GPL:

FILE **`/var/db/repos/gentoo/profiles/license_groups`****license\_groups example 2**

```
GPL-COMPATIBLE Apache-2.0 BSD BSD-2 GPL-2 GPL-3 LGPL-2.1 LGPL-3 X11 ZLIB
```
