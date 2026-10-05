<!-- source: https://wiki.gentoo.org/wiki/Wild | group: Gentoo Wiki (Main) | wiki-title: Wild -->
---
title: Wild
url: https://wiki.gentoo.org/wiki/Wild
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-30"
fingerprint: ce004b185c416dca
license: CC BY-SA 4.0
---

# Wild

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Wild is a linker written in Rust that aims to be very fast for iterative development.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [lto](https://packages.gentoo.org/useflags/lto) | Build with the experimental LTO plugin support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask sys-devel/wild`
## Configuration

### package.env

To use wild on a select set of packages, make a file in [/etc/portage/env](https://wiki.gentoo.org/wiki//etc/portage/env):

**`/etc/portage/env/wild`**

To apply this, create an entry in [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env):

**`/etc/portage/package.env/wild`**

## Usage

### Verifying a package used Wild

To verify that a package used Wild to link, use readelf:

`user $``readelf --string-dump .comment /usr/bin/hyperfine`
String dump of section '.comment':
  \[     1\]  GCC: (Gentoo 16.0.0\_p20251130 p25) 16.0.0 20251130 (experimental)
  \[    43\]  rustc version 1.91.0 (f8297e351 2025-10-28)
  \[    6f\]  GCC: (Gentoo 16.0.0\_p20251207 p26) 16.0.0 20251207 (experimental)
  \[    b1\]  Linker: Wild version 0.7.0 (6321c81fbcfda5604752ca3b889142a969e6c84b) (compatible with GNU linkers)

## Troubleshooting

### Known issues

- `error: unrecognized option(s): --defsym=__gentoo_check_ldflags__=0` ([bug](https://bugs.gentoo.org/966883)) - Fixed [upstream](https://github.com/davidlattimore/wild/commit/f70a0f3ce3401ded8c232d20cd83506ceff4c5ca), awaiting release.
- `error: -m elf_i386 is not yet supported` - Issue occurs in multilib packages with 32-bit enabled.
