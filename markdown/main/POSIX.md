<!-- source: https://wiki.gentoo.org/wiki/POSIX | group: Gentoo Wiki (Main) | wiki-title: POSIX -->
---
title: POSIX
url: https://wiki.gentoo.org/wiki/POSIX
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-20"
fingerprint: b70d8a0c41bf6dc1
license: CC BY-SA 4.0
---

# POSIX

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

POSIX is a collection of standards for Unix-like operating systems, defining "a standard operating system interface and environment, including a command interpreter (or 'shell'), and common utility programs to support applications portability at the source code level.".[\[1\]](https://wiki.gentoo.org#cite_note-1)

More formally, it currently refers to The Open Group Technical Standard Base Specifications, Issue 8, 2024 Edition (technically identical to IEEE Std 1003.1, 2024 Edition), also known as POSIX.1-2024. The previous version was POSIX.1-2017, consisting of POSIX.1-2008 plus Technical Corrigenda 1 and 2. An unofficial blog post describing a number of the changes to be found in POSIX.1-2024 is available [here](https://sortix.org/blog/posix-2024/).

Man pages documenting functions and utilities specified by POSIX are provided in the [sys-apps/man-pages-posix](https://packages.gentoo.org/packages/sys-apps/man-pages-posix) package, and have their relevant section numbers suffixed with a `p`. For example, [cat(1)](https://man.archlinux.org/man/cat.1.en) [refers to the man page for the version of cat provided by GNU Coreutils (](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)[sys-apps/coreutils](https://packages.gentoo.org/packages/sys-apps/coreutils)), but [cat(1p)](https://man.archlinux.org/man/cat.1p.en) [refers to the man page for cat as defined by POSIX.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

### Base Specifications

An [online frame-based interface to the Base Specifications](https://pubs.opengroup.org/onlinepubs/9799919799/) is available, but each volume can also be accessed directly:

- [Volume 1: Base Definitions](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/toc.html)
- [Volume 2: System Interfaces](https://pubs.opengroup.org/onlinepubs/9799919799/functions/toc.html)
- [Volume 3: Shell and Utilities](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/toc.html)
- [Volume 4: Rationale](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/toc.html)

#### Previous editions

Previous editions of Issue 7 of the Base Specifications:

The first edition of Issue 6 was released in 2001; the second edition, in 2004.

POSIX.1, "Core Services", was released in 1988, and incorporated ANSI C; POSIX.2, "Shell and Utilities", was released in 1992.

## External resources

- "[POSIX 2024 Changes](https://sortix.org/blog/posix-2024/)", on the Sortix blog
- [standards(7)](https://man.archlinux.org/man/standards.7.en)
