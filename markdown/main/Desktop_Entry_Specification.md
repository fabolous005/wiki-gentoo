<!-- source: https://wiki.gentoo.org/wiki/Desktop_Entry_Specification | group: Gentoo Wiki (Main) | wiki-title: Desktop Entry Specification -->
---
title: Desktop Entry Specification
url: https://wiki.gentoo.org/wiki/Desktop_Entry_Specification
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: "5142f45bf8bf9c6c"
license: CC BY-SA 4.0
---

# Desktop Entry Specification

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The **Desktop Entry Specification**, which describes the layout of .desktop files, is an [XDG](https://wiki.gentoo.org/wiki/XDG) specification designed to allow a standardized way of configuring a program's integration into a [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment), determining its appearance in the desktop environment's menu, how it should be launched, etc.

This specification is produced by freedesktop.org and is utilised by various desktop environments, including [Gnome](https://wiki.gentoo.org/wiki/Gnome), [KDE Plasma](https://wiki.gentoo.org/wiki/KDE_Plasma), and [Xfce](https://wiki.gentoo.org/wiki/Xfce), as well as related software.

## Example .desktop file

The following is an example of the formatting found in a .desktop file, for reference:

**`/usr/share/applications/larry.desktop`**

```
[Desktop Entry]
Version=1.0
Type=Application
Name=Larry the Cow
GenericName=Larry
Comment=Larry the Cow is a fictional desktop application, it can be used to do things on the computer
Icon=larry
TryExec=/usr/bin/larry
Exec=/usr/bin/larry
Terminal=false
```
## Working with .desktop files

### Syntax validation for .desktop files

The [official validation tool](https://www.freedesktop.org/wiki/Software/desktop-file-utils/) for .desktop files is distributed with the package [dev-util/desktop-file-utils](https://packages.gentoo.org/packages/dev-util/desktop-file-utils)

This can be installed by running:

`root #``emerge --ask dev-util/desktop-file-utils`
It can be used by running:

`user $``desktop-file-validate yourfile.desktop`
### Update .desktop file database

The [dev-util/desktop-file-utils](https://packages.gentoo.org/packages/dev-util/desktop-file-utils) package also provides the update-desktop-database command which can update the desktop file database when making changes to .desktop files. This is useful for updating the desktop environments application menu for example.

### Executable bit in .desktop files

.desktop files in /usr/share/applications/ should have consistent executable bits.

As of 2017-06-16 many ebuilds (mostly KDE) create executable .desktop files ([bug #621966](https://bugs.gentoo.org/show_bug.cgi?id=621966)).

Look for executable .desktop files on the system with:

`user $``find /usr/share/applications/ -executable -type f`
Please report any violations upstream.

Software should not ship a .desktop file with the executable bit set. The user can set the bit on demand where it is needed.

## Troubleshooting

Report bugs in **desktop-file-validate** on [https://gitlab.freedesktop.org/xdg/desktop-file-utils/issues](https://gitlab.freedesktop.org/xdg/desktop-file-utils/issues)

- [Validation in some cases seems not correct ...](https://forums.gentoo.org/viewtopic-t-1065444-start-5.html) (should be reported upstream)
- "desktop-file-validate claims OnlyShowIn is deprecated" [https://gitlab.freedesktop.org/xdg/desktop-file-utils/issues/52](https://gitlab.freedesktop.org/xdg/desktop-file-utils/issues/52)

## See also

## External resources

- [https://devmanual.gentoo.org/eclass-reference/desktop.eclass](https://devmanual.gentoo.org/eclass-reference/desktop.eclass)
- [https://lists.freedesktop.org/archives/xdg/2017-June/thread.html#13920](https://lists.freedesktop.org/archives/xdg/2017-June/thread.html#13920) - Discussion regarding the executable bit on .desktop files
