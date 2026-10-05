<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Missing_keywords_and_keyword_requests | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Missing_keywords_and_keyword_requests -->
---
title: Knowledge Base:Missing keywords and keyword requests
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Missing_keywords_and_keyword_requests
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-17"
fingerprint: "9f311e4848a3e32d"
license: CC BY-SA 4.0
---

# Knowledge Base:Missing keywords and keyword requests

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is about packages that have not been marked as available for installation on a specific architecture, and how to make them available. On architectures such as *amd64*, this usually isn't required.

## Synopsis

Some software packages have a missing [keyword](https://wiki.gentoo.org/wiki/KEYWORDS) for a specific [architecture](https://packages.gentoo.org/arches).

Here is an example of trying to emerge a package with no **\~arm** keyword.

`root #``emerge --ask media-libs/liblo`
These are the packages that would be merged, in order:
Calculating dependencies... done!
!!! All ebuilds that could satisfy "=media-libs/liblo-0.31" have been masked.
!!! One of the following masked packages is required to complete your request:
- media-libs/liblo-0.31::gentoo (masked by: missing keyword)
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.

## Environment

Gentoo installations on architectures with less keyword coverage, new architectures, embedded systems or specialized applications.
While **amd64** is probably the most common architecture with the most keywords added, it's possible to install Gentoo by stage3 on at least 10 architectures, not including prefix or variants with other C libraries than glibc. See a list of [supported architectures](https://packages.gentoo.org/arches) and a directory of [experimental downloads](https://bouncer.gentoo.org/fetch/root/all/experimental/). A Raspberry Pi is an example; it's perfectly possible to have a desktop environment or run server processes on this type of machine as with other architectures, even though the ebuild in Gentoo may not have a keyword at all.

## Analysis

Lots of packages may compile on an architecture even if there is no **\~arch** keyword added to them, finding missing keywords and filing keyword requests on the bug tracker can help the Gentoo project extend support for different architectures.
A package with an **\~arch** keyword means that the package has had some testing on that architecture, and would hopefully progress to being marked with a stable keyword (without the tilde prefix) once tested.

## Resolution

To test packages with missing keywords for an architecture, add them to /etc/portage/package.accept\_keywords appending \*\* (a space and two asterisks) to the category/package name and (optionally) the version required. Then emerge the package again.

`root #````
mkdir -p /etc/portage/package.accept_keywords/
```
`root #``$EDITOR /etc/portage/package.accept_keywords/liblo`
**`/etc/portage/package.accept_keywords/liblo`**

`root #``emerge --ask media-libs/liblo`
Lots more packages with missing keywords would need to be added in the same manner to the /etc/portage/package.accept\_keywords list if the package has a lot of dependencies. It's sometimes useful to keep these grouped in a file for that particular package or feature, if there are lots of them.

In some cases using the autounmask functions of portage to add the entries can make this process easier.

### Fixing it for everybody

If the package compiles successfully and tests of the package succeed, [open a bug](https://bugs.gentoo.org/enter_bug.cgi?format=guided) to request keywords for the package(s).

\* Product - Gentoo Linux
\* Component - Keywording
\* Hardware - Your architecture, for example arm or arm64.
\* Summary - "category/package \~myarch keyword request"
\* Bugzilla Keywords - do **not** add CC-ARCHES
\* Description - "I use this package on myarch please add \~myarch keyword"

Thanks to Jannik2099 on IRC for a magic link to open a template for arm64 keyword requests, here are some extra links for other arches. Click the link to go to a template for a keyword request on Gentoo bugzilla for each architecture.

## Don't request keywording in some cases

In some cases, e.g. [app-office/libreoffice](https://packages.gentoo.org/packages/app-office/libreoffice), there should be no keywording request.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-norequest-1)</sup> A hint may be if a version has no keyword at all:

`user $``equery m libreoffice`
\* app-office/libreoffice \[gentoo\]
Maintainer:  office@gentoo.org (Gentoo Office project)
Upstream:    None specified
Homepage:    https://www.libreoffice.org
Location:    /var/db/repos/gentoo/app-office/libreoffice
Keywords:    7.2.5.2-r1:0: amd64 arm64 x86 \~amd64-linux \~arm \~ppc64
Keywords:    7.2.6.1:0:
Keywords:    7.2.9999:0:
Keywords:    7.3.1.1-r1:0:
Keywords:    7.3.9999:0:
Keywords:    9999:0:
License:     || ( LGPL-3 MPL-1.1 )

## See also

- [Knowledge Base:Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package)
- [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) — files or directories of files containing definitions for per-package `ACCEPT_KEYWORDS` statements.
- [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS)
- [Bugzilla/Bug report guide](https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide) — explains how to report bugs using Gentoo's Bugzilla instance, which may be lightly customized to collect specific details for each Gentoo project area
- [Package testing](https://wiki.gentoo.org/wiki/Package_testing) — provides information for ebuild developers on **testing ebuilds**.
- [Gentoo Cheat Sheet](https://wiki.gentoo.org/wiki/Gentoo_Cheat_Sheet) — a reference card of useful commands for administrating Gentoo systems.

## External resources

## Acknowledgments

- Jannik2099
- sam
- dmb


Much of this page was written from taking notes from the chat on IRC in gentoo-arm. Thanks.
