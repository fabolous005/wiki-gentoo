<!-- source: https://wiki.gentoo.org/wiki/River | group: Gentoo Wiki (Main) | wiki-title: River -->
---
title: River
url: https://wiki.gentoo.org/wiki/River
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-09"
fingerprint: ba07065902975fcd
license: CC BY-SA 4.0
---

# River

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**River** is a non-monolithic [wlroots](https://wiki.gentoo.org/wiki/Wlroots)-based [Wayland compositor](https://wiki.gentoo.org/wiki/Wayland_compositor) written in [Zig](https://wiki.gentoo.org/wiki/Zig). River allows the use of a separate compatible [window manager](https://wiki.gentoo.org/wiki/Window_manager) to define window arrangement, window decorations, keybindings and other behavior.


## Installation


### USE flags

The available USE flags may be retrieved with the equery utility from [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit):

`user $``equery uses gui-wm/river`

### Emerge

River is currently not provided by the main Gentoo repository, but is available from the [GURU](https://wiki.gentoo.org/wiki/Project:GURU) repository.

Enable the GURU repository via [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

`root #``eselect repository enable guru`
Once enabled, sync it using emaint with the `--repo` option:

`root #``emaint sync --repo guru`
After the repository has been synced, install [gui-wm/river](https://packages.gentoo.org/packages/gui-wm/river):

`root #``emerge --ask gui-wm/river`

## Configuration

The behavior of river is largely dictated by the window manager. General purpose tools for monitor configuration are supported.

The configuration of input devices may be handled by the window manager itself or a separate tool.

The available options are:


### Files

River starts by executing the init file. The primary purpose of this file is to launch the window manager. By default, river will look for it at $XDG\_CONFIG\_HOME/river/init. If `XDG_CONFIG_HOME` is not set, river will search for the file at \~/.config/river/init.

Per the authors, the init file can be any executable script, not just a shell script. For example, it can be [written in Lua 5.4](https://gist.github.com/FollieHiyuki/f598db7c548f3397e2a68e4340ac9fdc).

Usually, the window manager can be used for init:

`user $``river -c <window manager>`

### Terminal emulator

River does not have a recommended [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator). The default configuration uses [foot](https://wiki.gentoo.org/wiki/Foot):

Other Wayland-native terminal emulators include [Alacritty](https://wiki.gentoo.org/wiki/Alacritty), [x11-terms/alacritty](https://packages.gentoo.org/packages/x11-terms/alacritty), and [Kitty](https://wiki.gentoo.org/wiki/Kitty), [x11-terms/kitty](https://packages.gentoo.org/packages/x11-terms/kitty), which work natively with Wayland if the `KITTY_ENABLE_WAYLAND` environment variable is set to `1`.


### Status bar

River does not have a built-in status bar. [Waybar](https://wiki.gentoo.org/wiki/Waybar) has [modules](https://github.com/Alexays/Waybar/wiki/Module:-River) for showing river mode, tags, windows, and layout:

`root #``emerge --ask gui-apps/waybar`
The authors also recommended the following status bars:


### Brightness

[dev-libs/light](https://packages.gentoo.org/packages/dev-libs/light) can be used to adjust backlight and brightness.


### Sound volume

On pure [ALSA](https://wiki.gentoo.org/wiki/ALSA) systems, sound volume can be changed with [alsamixer(1)](https://man.archlinux.org/man/alsamixer.1.en) [or](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [amixer(1)](https://man.archlinux.org/man/amixer.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

To raise volume by 10%:

`user $``amixer set "Master" 10%+`
To lower volume by 10%:

`user $``amixer set "Master" 10%-`
To toggle mute:

`user $``amixer set "Master" toggle`
Alternatively, if [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) is being used (including via [PipeWire](https://wiki.gentoo.org/wiki/PipeWire)'s PulseAudio emulation), volume can be controlled with [pactl(1)](https://man.archlinux.org/man/pactl.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

To raise volume by 5%:

`user $``pactl set-sink-volume @DEFAULT_SINK@ +5%`
To lower volume by 5%:

`user $``pactl set-sink-volume @DEFAULT_SINK@ -5%`

### Taking screenshots

To add simple screenshot support, use the [gui-apps/grim](https://packages.gentoo.org/packages/gui-apps/grim) package:

`root #``emerge --ask gui-apps/grim`
Area selection support can be added via [gui-apps/slurp](https://packages.gentoo.org/packages/gui-apps/slurp):

`root #``emerge --ask gui-apps/slurp`
For clipboard support, a handler like [gui-apps/wl-clipboard](https://packages.gentoo.org/packages/gui-apps/wl-clipboard) is necessary:

`root #``emerge --ask gui-apps/wl-clipboard`
With the preceding installed, the following commands can be used:

Screenshot display to clipboard:

`user $` `grim - | wl-copy`
Screenshot area to clipboard:

`user $``grim -g "$(slurp)" - | wl-copy`
Screenshot display and save to $HOME/Pictures:

`user $``grim ${HOME}/Pictures/$(date +'%s.png')`
Screenshot area and save to $$HOME/Pictures:

`user $``grim -g "$(slurp)" ${HOME}/Pictures/$(date +'%s.png')`

### Setting a wallpaper

The wallpaper can be set with [gui-apps/swaybg](https://packages.gentoo.org/packages/gui-apps/swaybg), using a command like:

**`~/.config/river/init`**

**Setting the wallpaper**

```
 -m fill -i /home/larry/wallpapers/Awesome_Gentoo_Wallpaper.png &
```

## Usage

River can be started from a tty or terminal with:

`user $``dbus-run-session river`
### Configurations and key bindings

All key bindings are managed by the window manager.


### Recommended software

Besides the recommended terminal emulators and status bars mentioned above, the river authors have  listed [other useful software](https://codeberg.org/river/wiki/src/branch/main/pages/useful-software.md).


## Troubleshooting

Refer to [the river wiki](https://codeberg.org/river/wiki) for FAQs and information about other issues.


## See also

- [bspwm](https://wiki.gentoo.org/wiki/Bspwm) — a lightweight, tiling, minimalist [window manager](https://wiki.gentoo.org/wiki/Window_manager) written in C, which organizes its windows as nodes of a binary tree.
- [dwm](https://wiki.gentoo.org/wiki/Dwm) — a dynamic [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11) from [suckless.org](https://suckless.org/).
- [Sway](https://wiki.gentoo.org/wiki/Sway) — an open-source [wlroots](https://wiki.gentoo.org/wiki/Wlroots)-based [Wayland compositor](https://wiki.gentoo.org/wiki/Wayland_compositor) that is designed to be compatible with the [i3](https://wiki.gentoo.org/wiki/I3) window manager.
- [Wayland](https://wiki.gentoo.org/wiki/Wayland) — a [communication protocol](https://en.wikipedia.org/wiki/communication_protocol) between a [display server](https://en.wikipedia.org/wiki/display_server) and its clients


## External resources

- [Separating the Wayland Compositor and Window Manager](https://isaacfreund.com/blog/river-window-management/) - "The new 0.4.0 release of river ... splits the window manager into a separate program. There are already many window managers compatible with river."
