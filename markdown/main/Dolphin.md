<!-- source: https://wiki.gentoo.org/wiki/Dolphin | group: Gentoo Wiki (Main) | wiki-title: Dolphin -->
---
title: Dolphin
url: https://wiki.gentoo.org/wiki/Dolphin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-12"
fingerprint: a262deda6ea77a74
license: CC BY-SA 4.0
---

# Dolphin

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Dolphin** is [KDE](https://wiki.gentoo.org/wiki/KDE)'s file manager that allows navigating and browsing the contents of hard drives, USB sticks, SD cards, and more. Creating, moving, or deleting files and folders is simple and fast. It is written in C++ and [Qt](https://wiki.gentoo.org/wiki/Qt).

## Installation

### USE flags


| [+handbook](https://packages.gentoo.org/useflags/+handbook) | Enable handbooks generation for packages by KDE | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [semantic-desktop](https://packages.gentoo.org/useflags/semantic-desktop) | Cross-KDE support for semantic search and information retrieval | 
| [telemetry](https://packages.gentoo.org/useflags/telemetry) | Send anonymized usage information to upstream so they can better understand our users | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Emerge Dolphin:

`root #``emerge --ask kde-apps/dolphin`
### Tips

To get archive extraction capabilities within the contextual menu, [kde-apps/ark](https://packages.gentoo.org/packages/kde-apps/ark) can be installed.

For thumbnails to show on audio files, enable the 'taglib' USE flag for [kde-apps/kio-extras](https://packages.gentoo.org/packages/kde-apps/kio-extras). Then within the Dolphin Configuration > Interface > Previews, enable the "Audio files" option.

## Troubleshooting

### Missing Associations when opening a file in Dolphin

When using Dolphin through some Window Managers (e.g. [Hyprland](https://wiki.gentoo.org/wiki/Hyprland)) without [kde-plasma/plasma-workspace](https://packages.gentoo.org/packages/kde-plasma/plasma-workspace) installed, opening a file may bring up a blank "Choose Applications" menu, even if compatible applications are installed. However, manually specifying an application still works.

This may further be confirmed by checking the logs and/or stdout of the offending application, and looking for a message similar to this:

```
"applications.menu"  not found in  QList("/home/youruser/.config/menus", "/etc/xdg/menus")
```
This happens because no application has created the freedesktop.org Menu File<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, which is responsible for populating this list. Usually, a desktop environment will handle this without user intervention- but in this case, the file is missing.

To fix this issue, first source a menu file, then ensure the $XDG\_MENU\_PREFIX variable matches, then regenerate the KDE's cache of .desktop/MIME .XML files.

#### Sourcing the Menu File

Unfortunately, there is no good solution to sourcing a menu file.

- KDE provides a Menu File in [kde-plasma/plasma-workspace](https://packages.gentoo.org/packages/kde-plasma/plasma-workspace)- specifically, `plasma-applications.menu`- which will play nicely with the KDE suite, including Dolphin. However, you may not want to emerge this package, as doing so will also install a complete Plasma workspace (if basic). Additionally, consider that this file is made for Plasma- and as such, if the Window Manager environment doesn't work well with Plasma's configs, this may not be the best option.
- Most other Desktop Environments provide their own menu files, e.g. [gnome-base/gnome-menus](https://packages.gentoo.org/packages/gnome-base/gnome-menus), which can be used independent of their parent Desktop Environment.
- There are several tools that can generate a menu file, such as [x11-misc/menumaker](https://packages.gentoo.org/packages/x11-misc/menumaker). However, these tools are usually tailor-made for a small set of Window Managers, and may not work
- Alternately, Menu Files can be created by-hand. These menu files are a variant on XML files<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. An example of a flat menu without categories can be found here [\[1\]](https://github.com/mid-kid/config/blob/942efdde89bfd3f72b9a93ba91ee0c670c017811/i3/.config/menus/applications.menu). An editor for Menu Files may be helpful, such as [kde-plasma/kmenuedit](https://packages.gentoo.org/packages/kde-plasma/kmenuedit).

#### Regenerating & Loading the Menu File

First, check your $XDG\_MENU\_PREFIX variable. Its value has to match the prefix on the menu file, or vice-versa. If they do not match, you must change one or the other. Make sure this is set for your entire login session.

Then, add a line to your session init scripts to run the command `kbuildsycoca6`. This will regenerate the cache.

Finally, log out and log back in.

## See also

- [File managers](https://wiki.gentoo.org/wiki/File_managers) — a computer program that allows for the manipulation of files and directories on a computer's [filesystem](https://wiki.gentoo.org/wiki/Filesystem).
- [PCManFM](https://wiki.gentoo.org/wiki/PCManFM) — a powerful yet lightweight file manager application, default file manager of [LXDE](https://wiki.gentoo.org/wiki/LXDE).
- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland))
