<!-- source: https://wiki.gentoo.org/wiki/Herbstluftwm | group: Gentoo Wiki (Main) | wiki-title: Herbstluftwm -->
---
title: Herbstluftwm
url: https://wiki.gentoo.org/wiki/Herbstluftwm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-22"
fingerprint: "96007b7fc8961be0"
license: CC BY-SA 4.0
---

# Herbstluftwm

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

herbstluftwm is a manual tiling [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11). It supports both tiling and floating windows as well as virtual desktops (tags) and immediate reloading of configuration files without the need to restart the window manager. Herbstluftwm also allows the user to split their screen space into multiple monitors, allowing for a setup where one monitor can have multiple virtual desktops visible at a time. Configuration is done exclusively using herbstclient which will send commands to a running herbstluftwm via Xlib.

## Installation

### USE flags


| [+doc](https://packages.gentoo.org/useflags/+doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

To install herbstluftwm using Portage simply run this command:

`root #``emerge --ask x11-wm/herbstluftwm`
### Additional software

The default configuration ships with a script (panel.sh) which depends on [x11-misc/dzen](https://packages.gentoo.org/packages/x11-misc/dzen).

Install dzen if you want to use the default configuration provided by Herbstluftwm:

`root #``emerge --ask x11-misc/dzen`
#### Rofi

Rofi is a window switcher, application launcher and dmenu replacement.

| [+drun](https://packages.gentoo.org/useflags/+drun) | Enable desktop file run dialog | 
| [+windowmode](https://packages.gentoo.org/useflags/+windowmode) | Enable normal window mode | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

To add a keybind (`Alt`+`d`) for Rofi modify the autostart file:

**`~/.config/herbstluftwm/autostart`**

**Adding a keybind for Rofi**

```
 keybind $Mod-d spawn rofi -show drun
```
#### Polybar

[Polybar](https://wiki.gentoo.org/wiki/Polybar) is a fast, easy-to-use and highly configurable status bar.

| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [i3wm](https://packages.gentoo.org/useflags/i3wm) | Add support for i3 window manager | 
| [ipc](https://packages.gentoo.org/useflags/ipc) | Add support for Inter-Process Messaging | 
| [mpd](https://packages.gentoo.org/useflags/mpd) | Add support for Music Player Daemon | 
| [network](https://packages.gentoo.org/useflags/network) | Enable network support | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

To automatically launch Polybar from your autostart file add a spawn command:

**`~/.config/herbstluftwm/autostart`**

**Launching Polybar automatically**

```
 spawn polybar --config=~/.config/polybar/config.ini
```
## Starting herbstluftwm

To start herbstluftwm, either use a [display manager](https://wiki.gentoo.org/wiki/Display_manager) or the command startx.
To use startx with [elogind](https://wiki.gentoo.org/wiki/Elogind) support, setup elogind and create the following file:

**`~/.xinitrc`**

## Configuration

The default configuration file is a good starting point for users who want to write their own configuration. First create the necessary directory for the configuration file:

`user $``mkdir -p ~/.config/herbstluftwm`
Now copy the default configuration file from /etc/xdg/herbstluftwm/autostart into the directory created above:

`user $``cp /etc/xdg/herbstluftwm/autostart ~/.config/herbstluftwm/`
The \~/.config/herbstluftwm/autostart file is a regular shell script (similar to [Bspwm](https://wiki.gentoo.org/wiki/Bspwm)) that by default will be executed by Bash.
For all list of all commands that herbstclient can send see the section "COMMANDS" in the [man page](https://herbstluftwm.org/herbstluftwm.html#COMMANDS).

### Autostart programs

**`~/.config/herbstluftwm/autostart`**

```
# Automatically start these programs
spawn syncthing
spawn dunst
spawn feh --bg-scale ~/Pictures/wallpaper.png
```
Herbstluftwm also provides tab-completion for herbstclient commands for both [Bash](https://wiki.gentoo.org/wiki/Bash) and [Zsh](https://wiki.gentoo.org/wiki/Zsh).

Bash users need to source the /etc/bash\_completion.d/herbstclient\_completion file from inside their \~/.bashrc.

**`~/.bashrc`**

**Enabling herbstclient tab-completion for Bash**

```
# Add this to the end of your ~/.bashrc
source /etc/bash_completion.d/herbstclient_completion
```
For Zsh tab-completion should be enabled by default, if it is not, activate it with [these instructions](https://wiki.gentoo.org/wiki/Zsh#app-shells.2Fzsh-completions).

## See also

- [Bspwm](https://wiki.gentoo.org/wiki/Bspwm) - a lightweight, tiling, minimalist window manager
- [Polybar](https://wiki.gentoo.org/wiki/Polybar) - a fast and easy-to-use status bar
