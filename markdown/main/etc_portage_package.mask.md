<!-- source: https://wiki.gentoo.org/wiki//etc/portage/package.mask | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/package.mask -->
---
title: "/etc/portage/package.mask"
url: https://wiki.gentoo.org/wiki//etc/portage/package.mask
hostname: gentoo.org
sitename: "/etc/portage/package.mask"
date: "2025-12-23"
fingerprint: "75b93e6e0ea7b389"
license: CC BY-SA 4.0
---

# /etc/portage/package.mask

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/etc/portage/package.mask** is a file, or a directory of files, controlled by the system administrator that can be used to prevent certain packages from being installed.

## Format

- One `DEPEND` atom per line.
- Anything following a `#` (hash) is treated as a comment, and is ignored.
- For `DEPEND` atom syntax, see [version specifier](https://wiki.gentoo.org/wiki/Version_specifier).

## Example

FILE **`/etc/portage/package.mask`****package.mask example**

Now Portage helpfully explains when packages are masked.

`root #``emerge virtual/jdk:1.8`
Calculating dependencies... done!
Dependency resolution took 3.47 s (backtrack: 0/20).
 
 
!!! All ebuilds that could satisfy "virtual/jdk:1.8" have been masked.
!!! One of the following masked packages is required to complete your request:
- virtual/jdk-1.8.0-r9::gentoo (masked by: package.mask)
/etc/portage/package.mask:
# Want to go without Java 8; mask JDK and JRE that use Java 8.
 
 
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.

## See also

- [Knowledge Base:Masking a package](https://wiki.gentoo.org/wiki/Knowledge_Base:Masking_a_package)
- [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage) — the primary configuration directory for [Portage](https://wiki.gentoo.org/wiki/Portage), Gentoo's package manager.
