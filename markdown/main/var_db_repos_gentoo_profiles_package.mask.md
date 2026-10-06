<!-- source: https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles/package.mask | group: Gentoo Wiki (Main) | wiki-title: /var/db/repos/gentoo/profiles/package.mask -->
---
title: "/var/db/repos/gentoo/profiles/package.mask"
url: https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles/package.mask
hostname: gentoo.org
sitename: "/var/db/repos/gentoo/profiles/package.mask"
date: "2021-03-22"
fingerprint: d721feef06359ba8
license: CC BY-SA 4.0
---

# /var/db/repos/gentoo/profiles/package.mask

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/var/db/repos/gentoo/profiles/package.mask** is a file containing package atoms to mask. This file is controlled by developers of the main ebuild repository ([gentoo.git](https://gitweb.gentoo.org/repo/gentoo.git)) and is not meant to be edited by system administrators. For this sysadmin friendly version see [/etc/portage/package.mask](https://wiki.gentoo.org/wiki//etc/portage/package.mask).

After syncing, end users can take a look in /var/db/repos/gentoo/profiles/package.mask for some up-to-date examples on packages that are currently masked. The comments preceding the package will provide insight into the reasons for the masking.

## Format

- Comment lines begin with `#` (no inline comments).
- One `DEPEND` atom per line.

## Examples

FILE **`/var/db/repos/gentoo/profiles/package.mask`****package.mask example**

```
# mask out versions 1.0.4496 of the nvidia
# drivers and later
>=media-video/nvidia-kernel-1.0.4496
>=media-video/nvidia-glx-1.0.4496
```
## See also

- [/etc/portage/package.mask](https://wiki.gentoo.org/wiki//etc/portage/package.mask) — a file, or a directory of files, controlled by the system administrator that can be used to prevent certain packages from being installed.
