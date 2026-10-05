<!-- source: https://wiki.gentoo.org/wiki/PipeWire/gentoo-pipewire-launcher | group: Gentoo Wiki (Main) | wiki-title: PipeWire/gentoo-pipewire-launcher -->
---
title: PipeWire/gentoo-pipewire-launcher
url: https://wiki.gentoo.org/wiki/PipeWire/gentoo-pipewire-launcher
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-03"
fingerprint: be2beaf22a03ab94
license: CC BY-SA 4.0
---

# PipeWire/gentoo-pipewire-launcher

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Those not willing or able to use the [OpenRC](https://wiki.gentoo.org/wiki/OpenRC)/[systemd](https://wiki.gentoo.org/wiki/Systemd) user services for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) and [WirePlumber](https://wiki.gentoo.org/wiki/WirePlumber) can instead use gentoo-pipewire-launcher.

- `XDG_RUNTIME_DIR`

This variable is usually set automatically by systemd-logind or [elogind](https://wiki.gentoo.org/wiki/Elogind). [seatd](https://wiki.gentoo.org/wiki/Seatd) on the other hand does need some [manual steps](https://man.sr.ht/~kennylevinsen/seatd/). Users **only need to set it manually** if not using a session/seat manager. Those systems like the ones using seatd or those that don't use any of these seat management daemons will need to set `XDG_RUNTIME_DIR` manually, e.g. in \~/.bash\_profile, \~/.zprofile, etc.


```
# Ensure XDG_RUNTIME_DIR is set
if test -z "$XDG_RUNTIME_DIR"; then
    export XDG_RUNTIME_DIR=$(mktemp -d /tmp/$(id -u)-runtime-dir.XXX)
fi
```
- `DBUS_SESSION_BUS_ADDRESS`

This variable is usually set automatically by desktop environments such as GNOME or KDE. Note that a D-Bus session bus is *not* what is typically being provided by the system D-Bus service, `dbus`, which is instead providing a *system* bus for things like hardware events, etc.

For further information, refer to [this section of the "D-Bus" page](https://wiki.gentoo.org/wiki/D-Bus#Manual).

gentoo-pipewire-launcher is a convenience script for systems not running systemd, e.g. OpenRC systems. It will only be installed if the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag is not enabled.](https://wiki.gentoo.org/wiki/USE_flag)

As documented in the gentoo-pipewire-launcher(1) man page, the gentoo-pipewire-launcher script starts:

- a PipeWire server;
- a WirePlumber session manager, required to make use of PipeWire servers;
- a pipewire-pulse PipeWire server, required for PulseAudio compatibility.

Before doing so, the script terminates any existing PipeWire or WirePlumber instances. For further details, refer to the comments in the script.

gentoo-pipewire-launcher should be started in an environment which has the `DBUS_SESSION_BUS_ADDRESS` environment variable set appropriately, i.e. within the context of the program started by dbus-launch or dbus-run-session.

gentoo-pipewire-launcher supports logging, via ${XDG\_CONFIG\_HOME}/gentoo-pipewire-launcher.conf. The variables `GENTOO_PIPEWIRE_LOG`, `GENTOO_PIPEWIRE_PULSE_LOG`, and `GENTOO_WIREPLUMBER_LOG` can be used to specify the absolute path of a file to which logs should be written. If these variables are not set, log output will go to /dev/null.

gentoo-pipewire-launcher sources the gentoo-pipewire-launcher.conf file, such that the conf file can be used to add variables (e.g. `PIPEWIRE_DEBUG=4`) to the environment of the PipeWire and WirePlumber processes it starts.

Gentoo's PipeWire package installs the /etc/xdg/autostart/pipewire.desktop autostart file. However, not all GUI environments make use of autostart files:  Plasma, GNOME, XFCE and Cinnamon do, but various window managers (such as Fluxbox) do not. Environments which make use of autostart files *must not* start PipeWire from some other location (e.g. the configuration file for that environment).

If XDG autostart is not being used, a call to gentoo-pipewire-launcher needs to be added to whichever file is used for starting programs at [window manager](https://wiki.gentoo.org/wiki/Window_manager) startup, e.g. for i3:

**`~/.config/i3/config`**

For [Hyprland](https://wiki.gentoo.org/wiki/Hyprland), edit \~/.config/hypr/hyprland.lua to add:

**`~/.config/hypr/hyprland.lua`**

For [Sway](https://wiki.gentoo.org/wiki/Sway), edit \~/.config/sway/config to add:

**`~/.config/sway/config`**

For [dwm](https://wiki.gentoo.org/wiki/Dwm), edit \~/.dwm/dwmrc to add:

**`~/.dwm/dwmrc`**

```
 &
```
For [Wayfire](https://wiki.gentoo.org/wiki/Wayfire), edit \~/.config/wayfire.ini to add:

**`~/.config/wayfire.ini`**

To restart PipeWire and WirePlumber under OpenRC, e.g. to pick up configuration changes, run gentoo-pipewire-launcher with the `restart` argument to have it first shut down the existing instances from within the relevant D-Bus session:

`user $````
nohup gentoo-pipewire-launcher restart &
```
Using nohup allows the terminal to be closed without terminating gentoo-pipewire-launcher. Where output is directed might depend on the implementation of nohup; depending on the shell, nohup might be a builtin or an external command (e.g. [nohup(1)](https://man.archlinux.org/man/nohup.1.en)[), so check the shell's documentation.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)
