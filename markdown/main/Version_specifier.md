<!-- source: https://wiki.gentoo.org/wiki/Version_specifier | group: Gentoo Wiki (Main) | wiki-title: Version specifier -->
---
title: Version specifier
url: https://wiki.gentoo.org/wiki/Version_specifier
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-26"
fingerprint: efa13c0ecee386c8
license: CC BY-SA 4.0
---

# Version specifier

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a reference on how to indicate a specific package when interacting with [Portage](https://wiki.gentoo.org/wiki/Portage).

A **version specifier**, or **atom**, is the precise format used to tell [emerge](https://wiki.gentoo.org/wiki/Emerge) exactly what package to install, with optional version, slot, or [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) of origin. The format is also used in files in [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage), and generally in Gentoo.

A version specifier is based on the *category/package* pair, with extra information if necessary. In some case the *category* may be omitted, if the package name is unique in all ebuild repositories configured with Portage, for example when using the emerge command.

## Version specifier format

### Basic

**category/package**

Matches **any version** of a package.

### By version

**\~category/package-1.23**

Matches version and any revision.

**=category/package-1.23\***

Matches a version by the version range. Note that there's no "`.`"  before the "`*`".

**=category/package-1.23**

Matches a version exactly.

**>=category/package-1.23**

Matches the specified version or any higher version.

**>category/package-1.23**

Matches a version strictly later than specified.

**\<category/package-1.23**

Matches a version strictly older than specified.

**\<=category/package-1.23**

Matches the specified version or any older version.

### By SLOT

**category/package:2**

Matches package in the specified package [SLOT](https://wiki.gentoo.org/wiki/SLOT). Note that there is no prefix.

### By ebuild repository

**category/package::repository**

Matches a package from a specific [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository). This can be combined with other specifiers. The official Gentoo repository is `::gentoo`.

## See also

- [emerge](https://wiki.gentoo.org/wiki/Emerge) — the main command-line interface to [Portage](https://wiki.gentoo.org/wiki/Portage)
- [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage) — the primary configuration directory for [Portage](https://wiki.gentoo.org/wiki/Portage), Gentoo's package manager.
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.

- [The Package Manager Specification, Chapter 3](https://projects.gentoo.org/pms/latest/pms.html#names-and-versions) — a more detailed description of the format
