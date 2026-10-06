<!-- source: https://wiki.gentoo.org/wiki/Display_manager | group: Gentoo Wiki (Main) | wiki-title: Display manager -->
---
title: Display manager
url: https://wiki.gentoo.org/wiki/Display_manager
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-09"
fingerprint: a0e7b71b0b9183f4
license: CC BY-SA 4.0
---

# Display manager

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

*Not to be confused with[window manager](https://wiki.gentoo.org/wiki/Window_manager).*

**Resources**

A **display manager** (**DM**), sometimes known as **login manager**, presents the user with a graphical login screen to start a GUI session, either [X](https://wiki.gentoo.org/wiki/Xorg) or [Wayland](https://wiki.gentoo.org/wiki/Wayland).

A display manager is not mandatory. X or Wayland can be started from a [shell](https://wiki.gentoo.org/wiki/Shell) in a [VT](https://wiki.gentoo.org/wiki/Terminal_emulator), but a DM can provide extra or useful functionality.

For how to run X without a DM, see [X without Display Manager](https://wiki.gentoo.org/wiki/X_without_Display_Manager).

## Available software

Some display managers are listed below.

| Name | Package | Type | Description | 
|---|---|---|---|
| CDM | [x11-misc/cdm](https://packages.gentoo.org/packages/x11-misc/cdm) | Console | Minimalistic. | 
| [GNOME/gdm](https://wiki.gentoo.org/wiki/GNOME/gdm) | [gnome-base/gdm](https://packages.gentoo.org/packages/gnome-base/gdm) | X / Wayland | Often used with GNOME. | 
| [greetd](https://wiki.gentoo.org/wiki/Greetd) | [gui-apps/gtkgreet](https://packages.gentoo.org/packages/gui-apps/gtkgreet) [gui-apps/tuigreet](https://packages.gentoo.org/packages/gui-apps/tuigreet) [gui-apps/qtgreet::wayland-desktop](https://gpo.zugaina.org/Overlays/wayland-desktop/gui-apps/qtgreet) | Wayland | Frontends for [greetd](https://wiki.gentoo.org/wiki/Greetd). TUIGreetd runs in console. | 
| [LightDM](https://wiki.gentoo.org/wiki/LightDM) | [x11-misc/lightdm](https://packages.gentoo.org/packages/x11-misc/lightdm) | X | Lightweight, and customizable via greeters. | 
| [LXDM](https://wiki.gentoo.org/wiki/LXDE) | [lxde-base/lxdm](https://packages.gentoo.org/packages/lxde-base/lxdm) | X | LXDE Display Manager. | 
| ly | [x11-misc/ly::guru](https://gpo.zugaina.org/Overlays/guru/x11-misc/ly) | Console | Lightweight TUI display manager. | 
| [Qingy](https://wiki.gentoo.org/wiki/Qingy) | [sys-apps/qingy](https://packages.gentoo.org/packages/sys-apps/qingy) | Console | getty replacement. | 
| [SDDM](https://wiki.gentoo.org/wiki/SDDM) | [x11-misc/sddm](https://packages.gentoo.org/packages/x11-misc/sddm) | X / Wayland | Modern, fast DM aiming to be simple and beautiful. Highly customizable, eye candy display manager from [KDE](https://wiki.gentoo.org/wiki/KDE). | 
| [SLiM](https://wiki.gentoo.org/wiki/SLiM) | [x11-misc/slim](https://packages.gentoo.org/packages/x11-misc/slim) | X | Requires only a few dependencies. | 
| WDM | [x11-misc/wdm](https://packages.gentoo.org/packages/x11-misc/wdm) | X | Modification of XDM. | 
| [XDM](https://wiki.gentoo.org/wiki/XDM) | [x11-apps/xdm](https://packages.gentoo.org/packages/x11-apps/xdm) | X | X.Org's DM. | 

## Configuration

In all major Linux operating systems, display managers are started automatically on boot. In order for this to happen automatically, a script must be added to the init system's appropriate runlevel. Examples for [OpenRC](https://wiki.gentoo.org/wiki/Display_manager#OpenRC) and [systemd](https://wiki.gentoo.org/wiki/Display_manager#systemd) are provided below.

### OpenRC

Under most circumstances, the [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) init system (Gentoo's default [init system](https://wiki.gentoo.org/wiki/Init_system)) will be used to start the display manager. The following examples will set [SDDM](https://wiki.gentoo.org/wiki/SDDM) as the display manager, adjust as necessary for other display managers.

If [gui-libs/display-manager-init](https://packages.gentoo.org/packages/gui-libs/display-manager-init) is not present, emerge it with:

`root #``emerge --ask gui-libs/display-manager-init`
The configuration file should be modified to use [SDDM](https://wiki.gentoo.org/wiki/SDDM):

**`/etc/conf.d/display-manager`**

**Set SDDM as the display manager**

```
CHECKVT=7
DISPLAYMANAGER="sddm"
```
To start the chosen display manager on boot, add the *display-manager* to the system's *default* runlevel:

`root #``rc-update add display-manager default`
To start the *display-manager* immediately, run:

`root #``rc-service display-manager start`
If the *display-manager* service can be run manually, but does not start automatically when booting, it may be beneficial to run:

`root #``rc-update --update`
### systemd

If using [systemd](https://wiki.gentoo.org/wiki/Systemd) as the init system, first locate the chosen \<display-manager>.service file.

To start [SDDM](https://wiki.gentoo.org/wiki/SDDM) on boot, enable the service:

`root #``systemctl enable sddm.service`
To start [SDDM](https://wiki.gentoo.org/wiki/SDDM) immediately, run:

`root #``systemctl start sddm.service`
## See also

- [Desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) — provides a list of desktop environments available in Gentoo.
- [Login](https://wiki.gentoo.org/wiki/Login) — logging in to a shell, and setting up the default environment.
- [Window manager](https://wiki.gentoo.org/wiki/Window_manager) — manages the creation, manipulation, and destruction of on-screen windows and window decorations in a GUI environment.
- [Xorg/Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) — explains what Xorg is, how to install it, and the various configuration options.
- [X without Display Manager](https://wiki.gentoo.org/wiki/X_without_Display_Manager) — describes how to start an X11 session without a display manager
