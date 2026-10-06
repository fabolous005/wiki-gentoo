<!-- source: https://wiki.gentoo.org/wiki/Default_applications | group: Gentoo Wiki (Main) | wiki-title: Default applications -->
---
title: Default applications
url: https://wiki.gentoo.org/wiki/Default_applications
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-07"
fingerprint: "8f19b85b1ab6a9c8"
license: CC BY-SA 4.0
---

# Default applications

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

This article lists methods of setting default applications, particularly in Linux [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) that follow [freedesktop.org](https://freedesktop.org/) standards and specifications. Note, however, that not all software and environments do so.

[MIME types](https://en.wikipedia.org/wiki/MIME) describe the kind of content a file contains, such as `text/plain`, `image/gif`, and `audio/mp3`. These types are often used to determine which applications should be used to open particular files.

## Setting default applications

### Via a file manager

In many cases it suffices to use a desktop's [file manager](https://wiki.gentoo.org/wiki/File_managers) to to set default applications for specific file types (e.g. via a right-click context menu); refer to the specific file manager manual.

### Via the desktop environment

Some desktop environments, like [GNOME](https://wiki.gentoo.org/wiki/GNOME) or [KDE](https://wiki.gentoo.org/wiki/KDE), allow setting default applications via their configuration systems.

In [XFCE](https://wiki.gentoo.org/wiki/XFCE), use xfce4-mime-settings from the [xfce-base/xfce4-settings](https://packages.gentoo.org/packages/xfce-base/xfce4-settings) package.

### Via setting a MIME type's default application directly

#### Using the xdg-\* suite of programs

The `xdg-*` suite of programs, such as [xdg-open(1)](https://man.archlinux.org/man/xdg-open.1.en) [and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [xdg-mime(1)](https://man.archlinux.org/man/xdg-mime.1.en)[, can be used to query and manage the default applications for various MIME types. For details, refer to the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [XDG/Software](https://wiki.gentoo.org/wiki/XDG/Software) page.

#### Manually editing mimeapps.list files

$XDG\_CONFIG\_HOME/mimeapps.list (previously, \~/.config/mimeapps.list and $XDG\_DATA\_HOME/applications/mimeapps.list) is typically used to specify associations between MIME types and applications.  The location of mimeapps.list files and their precedence is specified in the "[Association between MIME types and applications](https://standards.freedesktop.org/mime-apps-spec/mime-apps-spec-1.0.html)" freedesktop.org standard.

**`~/.config/mimeapps.list`**

**Set Qutebrowser as the default browser**

```
x-scheme-handler/http=org.qutebrowser.qutebrowser.desktop
x-scheme-handler/https=org.qutebrowser.qutebrowser.desktop
```
A particular desktop environment (DE) might also support $XDG\_CONFIG\_HOME/$desktop-mimeapps.list, to allow associations to be set per-DE.

### Via the mailcap(5) file

The [mailcap(5)](https://man.archlinux.org/man/mailcap.5.en) [file can be used by email software (and sometimes by other software) to specify associations between MIME types and applications. Refer to the man page for details.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)    

## See also

- [XDG/Base Directories](https://wiki.gentoo.org/wiki/XDG/Base_Directories) — standard directories specified by [freedesktop.org](https://freedesktop.org) (formerly the [X Desktop Group](https://wiki.gentoo.org/wiki/XDG))
- [XDG/Software](https://wiki.gentoo.org/wiki/XDG/Software) — command-line programs for managing and using default applications for particular [MIME](https://en.wikipedia.org/wiki/MIME) types
