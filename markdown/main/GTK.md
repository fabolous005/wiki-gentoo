<!-- source: https://wiki.gentoo.org/wiki/GTK | group: Gentoo Wiki (Main) | wiki-title: GTK -->
---
title: GTK
url: https://wiki.gentoo.org/wiki/GTK
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-19"
fingerprint: d6019278d0a6b9c4
license: CC BY-SA 4.0
---

# GTK

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**GTK** is a toolkit for creating graphical user interfaces. It is part of the GNU Project and maintained by [GNOME](https://wiki.gentoo.org/wiki/GNOME).

## Installation

Though GTK can be installed with the [emerge](https://wiki.gentoo.org/wiki/Emerge) command, it will usually be pulled in as a dependency by an [ebuild](https://wiki.gentoo.org/wiki/Ebuild).

If installing external software on Gentoo that requires GTK, write an ebuild to manage the installation of that software with its GTK dependency.

### GTK 4

GTK 4 is provided by [gui-libs/gtk](https://packages.gentoo.org/packages/gui-libs/gtk).

The [gtk4](https://packages.gentoo.org/useflags/gtk4) [USE flag is provided by packages that have optional GTK 4 support.](https://wiki.gentoo.org/wiki/USE_flag)

#### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [aqua](https://packages.gentoo.org/useflags/aqua) | Include support for the Mac OS X Aqua (Carbon/Cocoa) GUI | 
| [broadway](https://packages.gentoo.org/useflags/broadway) | Enable the GDK Broadway backend. | 
| [cloudproviders](https://packages.gentoo.org/useflags/cloudproviders) | Enable GtkPlacesSidebar to access cloud services | 
| [colord](https://packages.gentoo.org/useflags/colord) | Use x11-misc/colord for color management in printing | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [gstreamer](https://packages.gentoo.org/useflags/gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [sysprof](https://packages.gentoo.org/useflags/sysprof) | Enable profiling data capture support using dev-util/sysprof-capture | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vulkan](https://packages.gentoo.org/useflags/vulkan) | Add support for 3D graphics and computing via the Vulkan cross-platform API | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### GTK 3 and GTK 2

GTK 3 and GTK 2 are provided by the [x11-libs/gtk+](https://packages.gentoo.org/packages/x11-libs/gtk+) package.

Some applications may optionally be built with support for GTK by enabling their [gtk](https://packages.gentoo.org/useflags/gtk) [USE flag](https://wiki.gentoo.org/wiki/USE_flag).

There are also [gtk3](https://packages.gentoo.org/useflags/gtk3) [and](https://wiki.gentoo.org/wiki/USE_flag) [gtk2](https://packages.gentoo.org/useflags/gtk2) [USE flags.](https://wiki.gentoo.org/wiki/USE_flag)

#### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [aqua](https://packages.gentoo.org/useflags/aqua) | Include support for the Mac OS X Aqua (Carbon/Cocoa) GUI | 
| [broadway](https://packages.gentoo.org/useflags/broadway) | Enable the GDK Broadway backend | 
| [cloudproviders](https://packages.gentoo.org/useflags/cloudproviders) | Enable GtkPlacesSidebar to access cloud services | 
| [colord](https://packages.gentoo.org/useflags/colord) | Use x11-misc/colord for color management in printing | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [sysprof](https://packages.gentoo.org/useflags/sysprof) | Enable profiling data capture support using dev-util/sysprof-capture | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vim-syntax](https://packages.gentoo.org/useflags/vim-syntax) | Pulls in related vim syntax scripts | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 

## Configuration

One way of determining which version of GTK is used by a program is to check the output of [ldd(1)](https://man.archlinux.org/man/ldd.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``ldd $(command -v amsynth) | grep gtk`
libgtk-x11-2.0.so.0 => /usr/lib64/libgtk-x11-2.0.so.0 (0x00007ff535d8e000)

This shows that amsynth uses GTK 2.

Some environments have their own settings influencing the appearance of GTK applications. For example, GNOME has settings in the `org.gnome.desktop.interface` [dconf](https://en.wikipedia.org/wiki/dconf) schema (amongst others). Tools available for directly working with such settings include:

- [GSettings](https://wiki.gentoo.org/wiki/Gsettings) - Command-line tool for editing [dconf(1)](https://man.archlinux.org/man/dconf.1.en)[GConf](https://en.wikipedia.org/wiki/GConf)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.
- [lxde-base/lxappearance](https://packages.gentoo.org/packages/lxde-base/lxappearance) - Part of the [LXDE](https://wiki.gentoo.org/wiki/LXDE) desktop environment; works for GTK 2 apps also (dark theme).

### GTK 4

[gtk4-query-settings(1)](https://man.archlinux.org/man/gtk4-query-settings.1.en) [can be used to list current settings.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

GTK 4 uses the file \~/.config/gtk-4.0/settings.ini by default for user settings. For example:

**`~/.config/gtk-4.0/settings.ini`**

Note that:

GTK reads default values for settings from settings.ini files in /etc/gtk-4.0, $XDG\_CONFIG\_DIRS/gtk-4.0 and $XDG\_CONFIG\_HOME/gtk-4.0. These files must be valid key files, and have a section called `[Settings]`. Themes can also provide default values for settings by installing a settings.ini file next to their gtk.css file.[\[2\]](https://wiki.gentoo.org#cite_note-2)


The `GTK_THEME` environment variable can be used when debugging<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. For example, to specify that the dark Adwaita theme should be used by programs subsequently opened in a shell session:

`user $``export GTK_THEME=Adwaita:dark`
For a list of GTK 4 properties that can be set via a gtk-4.0/settings.ini file, refer to [this page](https://docs.gtk.org/gtk4/class.Settings.html#properties).

### GTK 3

GTK 3 uses the file \~/.config/gtk-3.0/settings.ini by default for user settings. For example:

**`~/.config/gtk-3.0/settings.ini`**

To specify default settings for all users, modify /etc/gtk-3.0/settings.ini. For example:

**`/etc/gtk-3.0/settings.ini`**

The [x11-libs/gtk+](https://packages.gentoo.org/packages/x11-libs/gtk+) version 3 package provides the gtk-query-settings(1) utility for printing the name and value of all GtkSettings properties.

### GTK 2

GTK 2 uses the file \~/.gtkrc-2.0 for user settings. Example file:

**`~/.gtkrc-2.0`**

The `GTK2_RC_FILES` environment variable can be set to point to a gtkrc-2.0 file containing settings for a particular GTK 2 application.

## Troubleshooting

### GTK Inspector

Both GTK 4 and GTK 3 support an interactive debugger known as the GTK Inspector. It can be enabled by setting the `GTK_DEBUG` environment variable to `interactive` when running a GTK-based program, e.g.:

`user $``GTK_DEBUG=interactive d-spy`
### Mouse scrolling doesn't work

With the gtk-3 applications under fvwm or other window manager / desktop environments, mouse scrolling sometime doesn't work as it should or at all. At the same time, it work with the applications using other toolkit environments like Qt, which imply this is not a system or Xorg configuration issue. To fix that issue, to add the following environmental variable should work, as example:

**`~/.bashrc`**

```
export GDK_CORE_DEVICE_EVENTS=1
```
For more information, see [GTK applications (sometimes?) do not scroll correctly, lacks focus?](https://bugzilla.redhat.com/show_bug.cgi?id=1226465).

## Additional software

[Search packages.gentoo.org](https://packages.gentoo.org/packages/search?q=gtk) for packages to use with GTK, such as themes or language bindings.

### Themes

- [x11-themes/arc-theme](https://packages.gentoo.org/packages/x11-themes/arc-theme) - flat theme with transparent elements for GTK 2/3/4

### Development tools

- [Cambalache](https://gitlab.gnome.org/jpu/cambalache) - RAD tool for GTK4 and GTK3
- [dev-util/gnome-builder](https://packages.gentoo.org/packages/dev-util/gnome-builder) - Integrated Development Environment for GNOME
- [dev-util/glade](https://packages.gentoo.org/packages/dev-util/glade) - RAD tool for GTK3, unmaintained

### Language bindings

- [dev-libs/gjs](https://packages.gentoo.org/packages/dev-libs/gjs) - Javascript bindings for GNOME
- [dev-cpp/gtkmm](https://packages.gentoo.org/packages/dev-cpp/gtkmm) - C++ interface for GTK
- [dev-python/pygobject](https://packages.gentoo.org/packages/dev-python/pygobject) - Python bindings for GObject based libraries such as GTK

## See also

- [GNOME](https://wiki.gentoo.org/wiki/GNOME) — a feature-rich desktop environment provided by the [GNOME project](https://www.gnome.org).

## External resources

- [GTK 4.0 documentation](https://docs.gtk.org/gtk4/index.html)
- [GTK 3.0 documentation](https://docs.gtk.org/gtk3/index.html)
- [x11-libs/wxGTK](https://packages.gentoo.org/packages/x11-libs/wxGTK) - GTK version of [wxWidgets](https://wxwidgets.org/), a cross-platform C++ GUI toolkit
