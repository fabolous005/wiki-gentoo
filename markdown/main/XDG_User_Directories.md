<!-- source: https://wiki.gentoo.org/wiki/XDG/User_Directories | group: Gentoo Wiki (Main) | wiki-title: XDG/User Directories -->
---
title: XDG/User Directories
url: https://wiki.gentoo.org/wiki/XDG/User_Directories
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-27"
fingerprint: a60744fbbb7ac4ea
license: CC BY-SA 4.0
---

# XDG/User Directories

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The XDG User Directories are "well known" user directories like the desktop folder and the music folder.

## Specifying directories

The [xdg-user-dirs specification](https://www.freedesktop.org/wiki/Software/xdg-user-dirs/) says that defaults for "well known" user directories are defined in:

- /etc/xdg/user-dirs.defaults, and
- ${XDG\_CONFIG\_HOME}/user-dirs.dirs

(where `XDG_CONFIG_HOME` defaults to \~/.config).

Sysadmins can use /etc/xdg/user-dirs.conf to disable this functionality, and to specify the charset encoding used for filenames.
