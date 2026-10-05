<!-- source: https://wiki.gentoo.org/wiki/KEYWORDS | group: Gentoo Wiki (Main) | wiki-title: KEYWORDS -->
---
title: KEYWORDS
url: https://wiki.gentoo.org/wiki/KEYWORDS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-16"
fingerprint: "1fb32cfe4fe1233c"
license: CC BY-SA 4.0
---

# KEYWORDS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In an ebuild the `KEYWORDS` variable informs in which [architectures](https://wiki.gentoo.org/wiki/Handbook:Main_Page#Architectures) the ebuild is stable or still in testing phase.

## Some possible values for KEYWORDS

The following box contains some example values for the `KEYWORDS` variable:

**`example.ebuild`**

```
KEYWORDS="alpha amd64 arm arm64 hppa m68k ~mips ppc ppc64 s390 sparc x86"
```
See the /var/db/repos/gentoo/profiles/arch.list for a list of keywords.

The prefix `~` (a tilde character) placed in front of various architectures in the example above means that architecture is in a "testing phase" and is not ready for production usage.

### Prefix keywords

Gentoo Linux keywords consist only of `$ARCH` (ex. `arm64`).
However portage can be used on different operating systems due to the [Prefix Project](https://wiki.gentoo.org/wiki/Project:Prefix).

Keywords used by prefix have an operating system suffix, like `~arm64-` or **macos**`amd64-`.
These keywords would mean that that ebuild package is testing on arm64 MacOS and stable on amd64 Linux **linuxfor prefix**.

Usual keyword rules apply to prefix keywords (ex. only arch team can add them).

### Special keywords

In addition to the normal `KEYWORDS` values Portage supports three special tokens:

- `*` - Package is visible if it is stable on any architecture.
- `~*` - Package is visible if it is in testing on any architecture.
- `**` - Package is always visible (`KEYWORDS` are ignored completely).

### Using more than one keyword

To use a recent version which is marked stable or unstable on any arch use:

**`/etc/portage/package.accept_keywords`**

To use a recent version which is marked unstable on your architecture or stable on any arch use:

**`/etc/portage/package.accept_keywords`**

## Using a package that is released for another architecture only

When the `-*` KEYWORD is specified, this indicates that the package is known to be broken on all systems which are not otherwise listed in KEYWORDS. For example, a binary only package which is built for the **x86** will look like:

`user $``equery meta fdftk`
\* app-text/fdftk \[gentoo\]
Maintainer:  robbat2@gentoo.org
Maintainer:  tex@gentoo.org (Gentoo TeX Project)
Upstream:    None specified
Homepage:    http://www.adobe.com/devnet/acrobat/fdftoolkit.html
Location:    /var/portage/repos/gentoo/app-text/fdftk
Keywords:    6.0-r1:0: x86 -\*
License:     Adobe

To accept this package on a **amd64** system anyways, then use one of the other keywords in the package.accept\_keywords like this:

**`/etc/portage/package.accept_keywords`**

For detailed information see the [portage(5)](https://man.archlinux.org/man/portage.5.en)[(5) man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## See also

- [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS)
- [Knowledge Base:Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package)
- [Knowledge Base:Accepting a keyword for all packages](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_all_packages)
- [Stable request](https://wiki.gentoo.org/wiki/Stable_request) — the procedure for moving an ebuild from testing to stable.
- [Package testing](https://wiki.gentoo.org/wiki/Package_testing) — provides information for ebuild developers on **testing ebuilds**.
- [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) — files or directories of files containing definitions for per-package `ACCEPT_KEYWORDS` statements.
- [equery ke(y)words](https://wiki.gentoo.org/wiki/Equery#Capabilities) — display keywords for specified PKG.
