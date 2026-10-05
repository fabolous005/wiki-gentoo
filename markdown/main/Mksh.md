<!-- source: https://wiki.gentoo.org/wiki/Mksh | group: Gentoo Wiki (Main) | wiki-title: Mksh -->
---
title: mksh
url: https://wiki.gentoo.org/wiki/Mksh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-15"
fingerprint: ab94565899bfeb93
license: CC BY-SA 4.0
---

# mksh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


mksh is the MirBSD Korn Shell, an actively developed free implementation of the Korn Shell programming language and a successor to the Public Domain Korn Shell (pdksh). It is developed as part of [the MirOS Project](https://www.mirbsd.org/main.htm) as native Bourne/POSIX/Korn shell for MirOS BSD, but also to be readily available under other UNIX-like operating systems. It targets users who desire a compact, fast, reliable, secure shell not cut off modern extensions, with unicode support.

Because of its speed, POSIX compliance, and advanced features, it is ideally suited for scripting. But it can serve very well as a login shell too. It is used as default shell on [Android](https://wiki.gentoo.org/wiki/Android).

## Installation

### Emerge

Install mksh:

`root #``emerge --ask app-shells/mksh`
Installed size is about 280K on an amd64 system (vs. 721K for bash-4).

## Configuration

### Set default login shell

To make mksh the default login shell, run:

`user $``chsh -s /bin/mksh`
### Files

#### Local

The local configuration file used is \~/.mkshrc — see /usr/share/doc/mksh-\*/dot.mkshrc\* for an example that is shipped with the package.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-shells/mksh`
## See also

- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
