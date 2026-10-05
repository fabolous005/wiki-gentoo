<!-- source: https://wiki.gentoo.org/wiki/Qman | group: Gentoo Wiki (Main) | wiki-title: Qman -->
---
title: qman
url: https://wiki.gentoo.org/wiki/Qman
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-17"
fingerprint: "974c78682af31feb"
license: CC BY-SA 4.0
---

# qman

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**qman** is a [man page](https://wiki.gentoo.org/wiki/Man_page) viewer with an ncurses [TUI](https://en.wikipedia.org/wiki/Text-based_user_interface). A page's references to other man pages get converted to links and can be viewed by clicking on them, while URLs and mail addresses can be opened in external programs.

## Installation

### Emerge

[qman](https://github.com/plp13/qman/blob/main/man/qman.1.md) is available via the [GURU repository](https://wiki.gentoo.org/wiki/Project:GURU), so be sure to [enable it](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users). Additionally, since packages in GURU are never stable, the appropriate [keyword](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS) needs to be specified:

Emerge:

`root #``emerge --ask app-misc/qman`
## Configuring qman when using mandoc instead of mandb+groff

When using the [mandoc](https://wiki.gentoo.org/wiki/Mandoc) package with the [system-man](https://packages.gentoo.org/useflags/system-man) [USE flag set, qman needs to be configure to use the mandoc system, supported by qman since version 1.5.0.](https://wiki.gentoo.org/wiki/USE_flag)

qman supports different locations for configuration files; only the first one found will be used. Possible locations are listed in the [configuration section of the manpage](https://github.com/plp13/qman/blob/main/man/qman.1.md#Configuration).

One possible file:

**`/etc/xdg/qman/qman.conf`**

## See also

- [man page](https://wiki.gentoo.org/wiki/Man_page) — contains system reference documentation. It is found on most Unix-like systems.
- [mandoc](https://wiki.gentoo.org/wiki/Mandoc) — a suite of tools for [mdoc(7)](https://mandoc.bsd.lv/man/mdoc.7.html)[roff](<https://en.wikipedia.org/wiki/Roff_(software)>) macro language preferred for BSD manual pages, and [man(7)](https://man.archlinux.org/man/man.7.en)- [Pager](https://wiki.gentoo.org/wiki/Pager) — a tool for displaying the contents of files or other output on the terminal, in a user friendly way, across several screens if needed.

## External resources

- The [README](https://github.com/plp13/qman/blob/main/README.md) of the qman project
