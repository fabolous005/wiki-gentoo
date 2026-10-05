<!-- source: https://wiki.gentoo.org/wiki/Wayfire | group: Gentoo Wiki (Main) | wiki-title: Wayfire -->
---
title: Wayfire
url: https://wiki.gentoo.org/wiki/Wayfire
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-05"
fingerprint: b680155e0ab7b9c5
license: CC BY-SA 4.0
---

# Wayfire

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Wayfire is a Wayland compositor inspired by Compiz and based on [wlroots](https://wiki.gentoo.org/wiki/Wlroots).

Wayfire aims to create a customizable, extendable and lightweight environment without sacrificing its appearance. It features a lot of graphical effects, such as the desktop cube, wobbly windows, fire animation, fish eye, workspace scale view and window rotations, among many other features.

## Installation

### USE flags


| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [+gles3](https://packages.gentoo.org/useflags/+gles3) | Enable OpenGL ES 3.x Features. | 
| [X](https://packages.gentoo.org/useflags/X) | Enable support for X11 applications (XWayland). | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vulkan](https://packages.gentoo.org/useflags/vulkan) | Add support for 3D graphics and computing via the Vulkan cross-platform API | 

### Emerge

`root #``emerge --ask gui-wm/wayfire`
## Configuration

The default configuration file is \~/.config/wayfire.ini but Wayfire also supports loading custom configuration files. A graphical interface for configuration is provided by the optional package [gui-apps/wcm](https://packages.gentoo.org/packages/gui-apps/wcm).

- ![](https://wiki.gentoo.org/images/thumb/f/f1/Wayfire1.png/120px-Wayfire1.png) Wayfire demo
- ![](https://wiki.gentoo.org/images/thumb/8/87/Wayfire2.png/120px-Wayfire2.png) Wayfire demo
- ![](https://wiki.gentoo.org/images/thumb/7/7b/Wayfire3.png/120px-Wayfire3.png) Wayfire demo
- ![](https://wiki.gentoo.org/images/thumb/9/9d/Wayfire4.png/120px-Wayfire4.png) Wayfire demo

### Manual configuration

Copy over the default configuration from /usr/share/wayfire like so:

`user $``cp /usr/share/wayfire/wayfire.ini ~/.config/`
#### Plugins

Wayfire plugins can be enabled and disabled via adding and removing entries in the `plugins =` in the **\[core\]**

For example, to remove the `wrot` plugin which allows for window rotation, comment it out in \~/.config/wayfire.ini by adding `#` as the *first* character of the line:

**`~/.config/wayfire.ini`**

```
[core]
plugins = \
  alpha \
  animate \
  autostart \
  command \
  cube \
  decoration \
  expo \
  fast-switcher \
  fisheye \
  foreign-toplevel \
  grid \
  gtk-shell \
  idle \
  invert \
  move \
  oswitch \
  place \
  resize \
  switcher \
  vswitch \
  window-rules \
  wm-actions \
  wobbly \
#  wrot \
  zoom
```
To install extra plugins, emerge the [gui-libs/wayfire-plugins-extra](https://packages.gentoo.org/packages/gui-libs/wayfire-plugins-extra) package:

`root #``emerge --ask gui-libs/wayfire-plugins-extra`
#### Shortcuts

External shortcuts in Wayfire are managed by the `command` plugin. A custom keybinding requires two things:

- A `binding_`- or `repeatable_binding_`-prefixed variable.
- A `command_`-prefixed variable.

The `repeatable_binding_` prefix assumes that the key in question is going to be held down.

Available meta keys include:

- `<super>`
- `<alt>`
- `<shift>`

As an example, to add a keybinding to start Firefox by pressing `<super>-<shift>-1`:

**`~/.config/wayfire.ini`**

```
binding_firefox = <super> <shift> KEY_1
command_firefox = firefox
```
The order and name of the variables doesn't matter, as long as the `binding_`- and `command_`-prefixed variables have the same suffix, e.g. `binding_x`, `command_x`. Note that an underscore, `_`, must be used as the separator.

## Usage

Wayfire is only a Wayland compositor and does not provide the full capabilities expected from a [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment). It is best used alongside [gui-apps/wf-shell](https://packages.gentoo.org/packages/gui-apps/wf-shell), which adds among other features a GTK3-based status bar and wallpaper support.

Wayfire needs additional applications which implement other parts of the XDG specifications from Freedesktop, such as desktop notifications, application launchers, screenshotting, screen recording and screen locking among other important necessities; refer to the [List of software for Wayland](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland) article for examples.

### Supported Wayland protocols

For information about Wayland protocols supported by Wayfire, refer to the [Wayfire/Wayland protocols](https://wiki.gentoo.org/wiki/Wayfire/Wayland_protocols) page.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose gui-wm/wayfire`
## See also

- [Wayland](https://wiki.gentoo.org/wiki/Wayland) — a [communication protocol](https://en.wikipedia.org/wiki/communication_protocol) between a [display server](https://en.wikipedia.org/wiki/display_server) and its clients
- [List of software for Wayland](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland) — various desktop related packages for Wayland

## External resources

- [Official wiki home](https://github.com/WayfireWM/wayfire/wiki/)
- [pywayfire](https://github.com/WayfireWM/pywayfire) - Python binding
