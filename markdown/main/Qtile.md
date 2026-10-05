<!-- source: https://wiki.gentoo.org/wiki/Qtile | group: Gentoo Wiki (Main) | wiki-title: Qtile -->
---
title: Qtile
url: https://wiki.gentoo.org/wiki/Qtile
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-23"
fingerprint: b41655591896f7c5
license: CC BY-SA 4.0
---

# Qtile

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Qtile** is an open-source tiling [window manager](https://wiki.gentoo.org/wiki/Window_manager) that is written in, and extended with, the [Python](https://wiki.gentoo.org/wiki/Python) programming language.

It can be used as an X11 window manager or a Wayland compositor.

## Installation

### USE flags

Qtile currently has no package-specific USE flags.

Apply these USE flags before emerging Qtile:

`root #``echo x11-libs/cairo X glib opengl svg >> /etc/portage/package.use/qtile`
### Emerge

Then emerge Qtile:

`root #``emerge --ask x11-wm/qtile`
## Qtile as a Wayland Compositor

These packages are found in the overlay [wayland-desktop](https://github.com/bsd-ac/wayland-desktop), please refer to [Wayland Desktop Landscape](https://wiki.gentoo.org/wiki/Wayland_Desktop_Landscape) for more information about said overlay.

Emerge pywlroots :

`root #``emerge --ask dev-python/pywlroots`
### Support for X11 applications (XWayland)

Emerge XWayland:

`root #``emerge --ask x11-base/xwayland`
## Configuration

### Starting as an X11 window manager

Start Qtile using a [display manager](https://wiki.gentoo.org/wiki/Display_manager) or the startx command.

If want to use startx and want [elogind](https://wiki.gentoo.org/wiki/Elogind) support, setup ConsoleKit and create the following file:

**`~/.xinitrc`**

```
exec dbus-launch --sh-syntax --exit-with-session qtile start
```
### Starting as a Wayland compositor

Start qtile from a command line using:

`user $``qtile start -b wayland`
Or start Qtile using [display manager](https://wiki.gentoo.org/wiki/Display_manager) by creating a session file:

**`/usr/share/wayland-sessions/qtile-wayland.desktop`**

```
[Desktop Entry]
Name=Qtile (Wayland)
Comment=Qtile Session
Exec=qtile start -b wayland
Type=Application
Keywords=wm;tiling
```
### Configuration file

Qtile can be customized by editing the config file in \~/.config/qtile/config.py.
This file is generated when there is no present configuration file.
If Qtile is running while configuring it, restart Qtile using its default keybind, `Super`+`Ctrl`+`R` to apply changes.

The default configuration file can be found at [https://github.com/qtile/qtile/blob/master/libqtile/resources/default\_config.py](https://github.com/qtile/qtile/blob/master/libqtile/resources/default_config.py).

To check if the changes are written correctly:

`user $``python3 -m py_compile ~/.config/qtile/config.py`
