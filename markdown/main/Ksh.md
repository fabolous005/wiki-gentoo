<!-- source: https://wiki.gentoo.org/wiki/Ksh | group: Gentoo Wiki (Main) | wiki-title: Ksh -->
---
title: Ksh
url: https://wiki.gentoo.org/wiki/Ksh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-07"
fingerprint: b3f5164cc19b7bd3
license: CC BY-SA 4.0
---

# Ksh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ksh** (**K**orn **sh**ell) is a [POSIX](https://wiki.gentoo.org/wiki/POSIX)-compliant [shell](https://wiki.gentoo.org/wiki/Shell) developed by David Korn. Gentoo provides ksh93u+m, forked from the original ATT ksh.

## Installation

`root #``emerge --ask app-shells/ksh`
## Configuration

ksh can be configured via the file \~/.kshrc, which is executed for interactive shells when `ENV` is not set. Refer to the [man page](https://manpages.debian.org/unstable/ksh93u+m/ksh93.1.en.html) for details.

### User shell

Users can make ksh their default shell by using [chsh(1)](https://man.archlinux.org/man/chsh.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``chsh -s /usr/bin/ksh`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-shells/ksh`
## See also

- [Bash](https://wiki.gentoo.org/wiki/Bash) — the default shell on Gentoo systems and a popular [shell](https://wiki.gentoo.org/wiki/Shell) program found on many Linux systems.
- [Dash](https://wiki.gentoo.org/wiki/Dash) — a small, fast, and [POSIX](https://wiki.gentoo.org/wiki/POSIX)-compliant [shell](https://wiki.gentoo.org/wiki/Shell).
- [Dsh](https://wiki.gentoo.org/wiki/Dsh) — a shell that allows parallel execution of remote commands across large numbers of servers
- [mksh](https://wiki.gentoo.org/wiki/Mksh) — an actively developed free implementation of the Korn Shell programming language and a successor to the Public Domain Korn Shell
- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
- [Zsh](https://wiki.gentoo.org/wiki/Zsh) — an interactive login shell that can also be used as a powerful scripting language interpreter.
