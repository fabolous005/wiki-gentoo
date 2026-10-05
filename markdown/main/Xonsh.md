<!-- source: https://wiki.gentoo.org/wiki/Xonsh | group: Gentoo Wiki (Main) | wiki-title: Xonsh -->
---
title: Xonsh
url: https://wiki.gentoo.org/wiki/Xonsh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-12-02"
fingerprint: d9a66e1e09ecc483
license: CC BY-SA 4.0
---

# Xonsh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Xonsh** combines a traditional [shell](https://wiki.gentoo.org/wiki/Shell) with the full power of Python, allowing POSIX shell style constructs used along with standard Python code.

Xonsh is multi platform, and is designed for interactive use, but can also be useful for scripting. Xonsh is very customizable, and there is a plugin system called xontribs.

## Installation

Xonsh is not currently present in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), though it may be installed by following instructions from the project's website. Care must however be taken to keep Xonsh up to date, independently of [Portage](https://wiki.gentoo.org/wiki/Portage) software.

## Configuration

To set Xonsh as the default shell with chsh, it will have to be added to /etc/shells. See [https://xon.sh/customization.html#set-xonsh-as-my-default-shell](https://xon.sh/customization.html#set-xonsh-as-my-default-shell) for how to do this.

### Files

The main Xonsh user configuration file is either \~/.xonshrc or, \~/.config/xonsh/rc.xsh - choose one upon first use. There can also be a system-wide config file at /etc/xonshrc.

If a there is a \~/.config/xonsh/rc.d directory, any *\*.xsh* files in it will get "sourced" on startup.

## See also

- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
