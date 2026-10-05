<!-- source: https://wiki.gentoo.org/wiki/Mandoc | group: Gentoo Wiki (Main) | wiki-title: Mandoc -->
---
title: Mandoc
url: https://wiki.gentoo.org/wiki/Mandoc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-12"
fingerprint: dac93a68b0d5790d
license: CC BY-SA 4.0
---

# Mandoc

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**mandoc** is a suite of tools for [mdoc(7)](https://mandoc.bsd.lv/man/mdoc.7.html)[, the semantic](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [roff](<https://en.wikipedia.org/wiki/Roff_(software)>) macro language preferred for BSD manual pages, and [man(7)](https://man.archlinux.org/man/man.7.en)[, the historical presentation-based roff macro language for UNIX manuals.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## Installation

### USE flags


| [cgi](https://packages.gentoo.org/useflags/cgi) | build man.cgi web plugin for viewing man pages | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [system-man](https://packages.gentoo.org/useflags/system-man) | set as the default man provider | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

Before enabling the [system-man](https://packages.gentoo.org/useflags/system-man) `USE` flag, please refer to "[The system-man USE flag](https://wiki.gentoo.org#The_system-man_USE_flag)".

### Emerge

`root #``emerge --ask app-text/mandoc`
### The system-man USE flag

The [system-man](https://packages.gentoo.org/useflags/system-man) `USE` flag makes mandoc the default program for accessing man pages.

To ensure mandoc can display man pages, either set the value of `PORTAGE_COMPRESS` variable to `gzip` or `pigz` in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf), as described in the following sections, or leave man pages uncompressed. Then reinstall packages containing man pages.

### Compressing man pages with gzip

To compress with [gzip](https://wiki.gentoo.org/wiki/Gzip) and optionally select a compression level via `PORTAGE_COMPRESS_FLAGS`:

**`/etc/portage/make.conf`**

```
PORTAGE_COMPRESS="gzip"
# Optionally set the compression level
PORTAGE_COMPRESS_FLAGS="-9"
```
### Optional: Using pigz for higher gzip compression

[app-arch/pigz](https://packages.gentoo.org/packages/app-arch/pigz) is mainly known for being a parallel implementation of gzip. However, pigz offers the compression level `-11`, which utilizes the [zopfli](https://en.wikipedia.org/wiki/zopfli) algorithm.

To install [app-arch/pigz](https://packages.gentoo.org/packages/app-arch/pigz):

`root #``emerge -va app-arch/pigz`
**`/etc/portage/make.conf`**

```
PORTAGE_COMPRESS="pigz"
# Optionally set the compression level. 1-9 are comparable to gzip, -11 leads to using the zopfli algorithm
PORTAGE_COMPRESS_FLAGS="-11"
```
### Requesting uncompressed man pages

To configure `PORTAGE_COMPRESS_EXCLUDE_SUFFIXES` to not compress man pages:

**`/etc/portage/make.conf`**

```
# man pages suffixes, may not be exhaustive
PORTAGE_COMPRESS_EXCLUDE_SUFFIXES="[1-9] n [013]p [1357]ssl [1357]ossl 3am 3pcap 3perl 3pm [1-9]bsd"
```
### Reinstalling packages to make their man pages usable

Packages that will need to be reinstalled include:

For a complete reinstallation of packages to be reinstalled, the following commands can be used. The first line requires [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) and, if using Bash, the `globstar` option:

`user $``qfile /usr/share/man/**/*.bz2 | cut -d: -f1 | sort -u >/tmp/reinstall_packages.txt``user $``for package in $(cat /tmp/reinstall_packages.txt); do emerge -1 "$package"; done`
## See also

[man page](https://wiki.gentoo.org/wiki/Man_page) — contains system reference documentation. It is found on most Unix-like systems.

## External resources

- The [mdoc(7)](https://mandoc.bsd.lv/man/mdoc.7.html)
