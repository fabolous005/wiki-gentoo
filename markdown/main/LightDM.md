<!-- source: https://wiki.gentoo.org/wiki/LightDM | group: Gentoo Wiki (Main) | wiki-title: LightDM -->
---
title: LightDM
url: https://wiki.gentoo.org/wiki/LightDM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-19"
fingerprint: e260d25938b639d6
license: CC BY-SA 4.0
---

# LightDM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**LightDM** is a cross-desktop [display manager](https://wiki.gentoo.org/wiki/Display_manager) whose aim is to be the standard display manager for the X server.

The key features (as listed by upstream) include:

- A well-defined greeter API allowing multiple GUIs.
- Support for all display manager use cases, with plugins where appropriate.
- Low code complexity.
- Fast performance.

## Installation

### USE flags


| [+gnome](https://packages.gentoo.org/useflags/+gnome) | Add GNOME support | 
| [+gtk](https://packages.gentoo.org/useflags/+gtk) | Pull in the gtk+ greeter | 
| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [non-root](https://packages.gentoo.org/useflags/non-root) | Use non-root user by default | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [vala](https://packages.gentoo.org/useflags/vala) | Enable bindings for dev-lang/vala | 

### Emerge

Install [x11-misc/lightdm](https://packages.gentoo.org/packages/x11-misc/lightdm):

`root #``emerge --ask x11-misc/lightdm`
## Configuration

The (global) configuration file for LightDM can be found at /etc/lightdm/lightdm.conf. Upon successful authentication, LightDM starts an Xsession through the following configuration option (enabled by default):

**`/etc/lightdm/lightdm.conf`**

### GTK

The greeter allows to select logging into one of the installed type of sessions through the icon in the Panel on top of the display. This makes it unnecessary to configure the session startup through user profile configurations. For example, [xfce-base/xfce4-session](https://packages.gentoo.org/packages/xfce-base/xfce4-session) installs /usr/share/xsessions/xfce.desktop which enables the greeter to offer the [Xfce](https://wiki.gentoo.org/wiki/Xfce) session. The default is an Xsession which relies on user profile files.

The GTK greeter configuration can be modified by manually editing the following file:

/etc/lightdm/lightdm-gtk-greeter.conf

### Qt

Currently Gentoo does not support the Qt greeter since it was moved out into it's own project at [https://github.com/surlykke/qt-lightdm-greeter](https://github.com/surlykke/qt-lightdm-greeter) however USE flag support is in the [x11-misc/lightdm](https://packages.gentoo.org/packages/x11-misc/lightdm) package if someone wanted to add the greeter support.

### Boot service

#### OpenRC

##### With display-manager

`root #``emerge --ask gui-libs/display-manager-init`
Set LightDM as the default display manager:

**`/etc/conf.d/display-manager`**

```
DISPLAYMANAGER="lightdm"
```
To start LightDM on boot, add dbus and display-manager to the default runlevel. [dbus](https://wiki.gentoo.org/wiki/Dbus) is necessary because LightDM depends on it to pass messages:

`root #````
rc-update add dbus default
```
`root #``rc-update add display-manager default`
To start LightDM now:

`root #````
rc-service dbus start
```
`root #``rc-service display-manager start`
##### With the deprecated xdm init script

Set LightDM as the default display manager:

**`/etc/conf.d/xdm`**

```
DISPLAYMANAGER="lightdm"
```
To start LightDM on boot, add dbus and xdm to the default runlevel. [dbus](https://wiki.gentoo.org/wiki/Dbus) is necessary because LightDM depends on it to pass messages:

`root #``rc-update add dbus default``root #``rc-update add xdm default`
To start LightDM now:

`root #``/etc/init.d/dbus start``root #``/etc/init.d/xdm start`
#### systemd

To start LightDM on boot:

`root #``systemctl enable lightdm`
To start LightDM now:

`root #``systemctl start lightdm`
### Command-line tool

LightDM includes a command-line tool, dm-tool, which can be used to switch user sessions, lock the current [seat](https://wiki.gentoo.org/wiki/Multiseat), etc. To see a list of available commands, use the `--help` option:

`user $``dm-tool --help`
For example, to lock the current seat:

`user $``dm-tool lock`
## Tips

### Running commands at log-in

A user can run some programs automatically when logging in using LightDM by adding commands in \~/.xprofile, which will be sourced by LightDM. For example:

**`~/.xprofile`**

```
# Starting redshift, setting the dpi with xrandr and set the brightness to 50% with xbacklight
xrandr --dpi 192 &
redshift-gtk &
xbacklight -set 50 &
```
### Unlock GNOME Keyring

To unlock your GNOME Keyring ([gnome-base/gnome-keyring](https://packages.gentoo.org/packages/gnome-base/gnome-keyring)) automatically on login, edit /etc/pam.d/lightdm to look as follows. Note: Lines ending with the comment `#keyring` should be added.

**`/etc/pam.d/lightdm`**

### Locking the screen with elogind after suspend or sleep

For security, it is good practice to lock the screen after [elogind](https://wiki.gentoo.org/wiki/Elogind) triggers suspend or sleep. This can be done easily by doing the following:

Install light-locker:

`root #``emerge --ask x11-misc/light-locker`
Start light-locker after the X server has started by putting light-locker & into either an \~/.xprofile or \~/.xinitrc file.

**`~/.xprofile`**

```
# Starting light-lock with X session
light-locker &
```
Create a lock.sh file under /lib64/elogind/system-sleep/ (be sure to add execute permissions to the file):

`root #``chmod +x /lib64/elogind/system-sleep/lock.sh`
## Troubleshooting

### LightDM crashes upon first login if hostname changes during login

In some cases LightDM may crash when trying to log in for the first time if the hostname changes in the time between the boot and login ([launchpad bug #1677058](https://bugs.launchpad.net/ubuntu/+source/lightdm-gtk-greeter/+bug/1677058)).

This may be encountered if [net-misc/networkmanager](https://packages.gentoo.org/packages/net-misc/networkmanager) is using the default settings to obtain the hostname from DHCP server and the hostname differs from the default one set on boot.

To disable NetworkManager hostname setting behavior, set the following line in `[main]` section of /etc/NetworkManager/NetworkManager.conf:

**`/etc/NetworkManager/NetworkManager.conf`**

### LightDM fails to launch with Nvidia GPU

Users with Nvidia GPUs may encounter failures when using LightDM ([GitHub issue #263](https://github.com/canonical/lightdm/issues/263)).

A workaround for this issue involves editing /etc/lightdm/lightdm.conf and adding the line `logind-check-graphical=false` within the `[LightDM]` section.

**`/etc/lightdm/lightdm.conf`**

## See also

- [SDDM](https://wiki.gentoo.org/wiki/SDDM) — a modern [display manager](https://wiki.gentoo.org/wiki/Display_manager) that supports both the [X server](https://wiki.gentoo.org/wiki/X_server) and the [Wayland](https://wiki.gentoo.org/wiki/Wayland) protocol.
