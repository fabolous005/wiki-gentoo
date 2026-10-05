<!-- source: https://wiki.gentoo.org/wiki/Xrdp | group: Gentoo Wiki (Main) | wiki-title: Xrdp -->
---
title: xrdp
url: https://wiki.gentoo.org/wiki/Xrdp
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-07"
fingerprint: b840b7199985f1f6
license: CC BY-SA 4.0
---

# xrdp

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**xrdp** is an open source [RDP](https://en.wikipedia.org/wiki/RDP) server that supports many session runners.

One of the runners, xorgxrdp, starts a standalone [X11](https://wiki.gentoo.org/wiki/X11) server. In this guide we'll use it.

## Features

- Two-way clipboard transfer
- Drive redirection (mount local client drives on remote machine)
- [TLS](https://en.wikipedia.org/wiki/TLS) encryption
- Proxying RDP and [VNC](https://en.wikipedia.org/wiki/VNC)
- Reconnect to an existing session

## Installation

### Emerge

Enable the [dilfridge](https://repos.gentoo.org/#dilfridge) repository:

`root #````
eselect repository enable dilfridge
```
`root #````
emaint sync -r dilfridge
```
Install xorgxrdp:

`root #``emerge --ask net-misc/xorgxrdp`
## Configuration

The configuration file is installed into /etc/xrdp/xrdp.ini, the configuration file for session management parameters into /etc/xrdp/sesman.ini.

The default system-wide startwm.sh respects $XSESSION variable set in /etc/profile. Without additional configuration, xrdp will start Xorg with the startup of /etc/xrdp/xrdp.ini.

The default per-user windows manager startup script is \~/startwm.sh

### Example Window Manager Xrdp startup configuration

Change file name of per-user windows manager startup script to something more explicit and make it a dot file.

**`/etc/xrdp/sesman.ini`**

```
[Globals]
; Give in relative path to user's home directory
; UserWindowManager=startwm.sh
UserWindowManager=.start_xrdpwm.sh
```
Example startup script for [Xfce](https://wiki.gentoo.org/wiki/Xfce).

**`$HOME/.start_xfcewm.sh`**

**Startup script for Xfce4**

```
#!/bin/sh
# Setup XRDP specific config directory / environment variables
# so that the XRDP desktop can have different parameters from 
# any possible local / physical display Xdisplay
[ -d $HOME/.config/xfce4-xrdp ] && mkdir -p $HOME/.config/xfce4-xrdp
[ -d $HOME/.cache/xfce4-xrdp ]  && mkdir -p $HOME/.cache/xfce4-xrdp
export XDG_CONFIG_HOME=$HOME/.config/xfce4-xrdp
export XDG_CACHE_HOME=$HOME/.cache/xfce4-xrdp
exec xfce4-session
```
### Service

To start xrdp at boot, run the following commands:

#### OpenRC

`root #````
rc-service xrdp start
```
`root #````
rc-update add xrdp default
```
#### systemd

`root #````
systemctl start xrdp.service
```
`root #````
systemctl enable xrdp.service
```
## Audio support

Enable the [GURU](https://wiki.gentoo.org/wiki/GURU) repository:

`root #````
eselect repository enable guru
```
`root #````
emaint sync -r guru
```
Install the xrdp module for [Pipewire](https://wiki.gentoo.org/wiki/Pipewire):

`root #``emerge --ask media-sound/pipewire-module-xrdp`
The module will load automatically via XDG autostart if xrdp session is detected.
