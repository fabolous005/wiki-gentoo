<!-- source: https://wiki.gentoo.org/wiki/Cpio | group: Gentoo Wiki (Main) | wiki-title: Cpio -->
---
title: cpio
url: https://wiki.gentoo.org/wiki/Cpio
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-20"
fingerprint: da8f9ecb9c837b7f
license: CC BY-SA 4.0
---

# cpio

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**cpio** is a file [archiving](https://wiki.gentoo.org/wiki/Data_compression) utility. It utilises a file format of the same name, [cpio(5)](https://man.archlinux.org/man/cpio.5.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

By default, Gentoo uses [GNU cpio](https://www.gnu.org/software/cpio/) (gcpio) as its cpio implementation. However, [app-arch/libarchive](https://packages.gentoo.org/packages/app-arch/libarchive) provides an alternative implementation, bsdcpio.

## Installation

### Emerge

`root #``emerge --ask app-alternatives/cpio`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-alternatives/cpio`
## See also

- [Data compression](https://wiki.gentoo.org/wiki/Data_compression) — a list of some of the **compression and file-archiver utilities** available in Gentoo Linux
- [pax](https://wiki.gentoo.org/wiki/Pax) — a file [archiving utility](https://wiki.gentoo.org/wiki/Data_compression) specified by [POSIX](https://wiki.gentoo.org/wiki/POSIX), intended to replace the [cpio] utility
- [Tar](https://wiki.gentoo.org/wiki/Tar) — an [archiver](https://wiki.gentoo.org/wiki/Data_compression) tool that provides the ability to create tar archives, as well as various other kinds of manipulation.
