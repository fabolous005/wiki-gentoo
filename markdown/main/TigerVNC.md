<!-- source: https://wiki.gentoo.org/wiki/TigerVNC | group: Gentoo Wiki (Main) | wiki-title: TigerVNC -->
---
title: TigerVNC
url: https://wiki.gentoo.org/wiki/TigerVNC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-21"
fingerprint: "90dab55adb00b8d7"
license: CC BY-SA 4.0
---

# TigerVNC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**TigerVNC** is a client/server software package allowing remote network access to graphical desktops.

## Installation

### USE flags


| [+drm](https://packages.gentoo.org/useflags/+drm) | Build with DRM support | 
| [+opengl](https://packages.gentoo.org/useflags/+opengl) | Add support for OpenGL (3D graphics) | 
| [+server](https://packages.gentoo.org/useflags/+server) | Build TigerVNC server | 
| [+viewer](https://packages.gentoo.org/useflags/+viewer) | Build TigerVNC viewer | 
| [dri3](https://packages.gentoo.org/useflags/dri3) | Build with DRI3 support | 
| [gnutls](https://packages.gentoo.org/useflags/gnutls) | Prefer net-libs/gnutls as SSL/TLS provider (ineffective with USE=-ssl) | 
| [java](https://packages.gentoo.org/useflags/java) | Build TigerVNC Java viewer | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pwquality](https://packages.gentoo.org/useflags/pwquality) | Use dev-libs/libpwquality for password quality checking in vncpasswd | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 

### Emerge

`root #``emerge --ask net-misc/tigervnc`
#### Additional Software

The following package can be installed to integrate the VNC server into Xorg:

`root #``emerge --ask net-misc/tigervnc-xorg-module`
## User Session Configuration

The easiest way to use TigerVNC as a server is to run the **x0vncserver** component with the user's X session:

**`~/.xinitrc`**

The password file can be defined with:

`user $````
mkdir ~/.config/tigervnc
```
`user $````
vncpasswd ~/.config/tigervnc/passwd
```
`user $``chmod 600 ~/.config/tigervnc/passwd`
### Localhost Session

A VNC server can be started on localhost instead of on the network, allowing it to be forwarded over SSH, this can be accomplished with:

**`~/.xinitrc`**

A SSH connection can be made, forwarding `127.0.0.1:5900` on the destination machine to port *5900* on the client:

`user $``ssh -L 5900:127.0.0.1:5900 larry@remoteMachine`
## Single Server Configuration

This configuration allows remote control of the **entire** Xorg X11 server. [net-misc/tigervnc-xorg-module](https://packages.gentoo.org/packages/net-misc/tigervnc-xorg-module) is required.

Create the TigerVNC config file for Xorg X11:

`root #``mkdir -p /etc/X11/xorg.conf.d`
**`/etc/X11/xorg.conf.d/40-vnc.conf`**

Create /etc/X11/vncpasswd

`root #``vncpasswd /etc/X11/vncpasswd`
## Multiple Server Configuration

Login as 'normal' user. The following steps can be taken for any user who wishes to configure the VNC server for remote connection.

Set a password:

`user $``vncpasswd`
Start the server giving it an unused display number (for example :1 or :2):

`user $``vncserver :N`
If desired, use a VNC client on either a local or remote machine to test the connection.

Once finished, kill the running vncserver by pressing C-c.

### Displays

Setup the displays in the TigerVNC configuration file:

**`/etc/tigervnc/vncserver.users`**

Typically the value of `:0` will be used for the server's own X display. This is why the example above starts by using the `:1` display handle.

Setup the displays for OpenRC. This step is not required for systemd. Substitute each '`user`' value below with the name of a user who will be running the VNC server on the machine:

**`/etc/conf.d/tigervnc`**

```
DISPLAYS="user:1 user2:2"
```
### Desktop environments

To setup the default desktop environment, add it to `session=` (or uncomment one from below):

**`/etc/tigervnc/vncserver-config-defaults`**

Each user who will be running a VNC server can override this configuration by adding it to `~/.config/tigervnc/config`. There is a file `/etc/tigervnc/vncserver-config-mandatory` where the system administrator can override user's config. `~/.config/tigervnc/xstartup` is no longer supported and the current server ignores it.

## Configuration

### Service

#### OpenRC

This example assumes 2 displays, :1 and :2

Create one link for every display:

`root #````
ln -s tigervnc /etc/init.d/tigervnc.1
```
`root #````
ln -s tigervnc /etc/init.d/tigervnc.2
```
Start the server(s):

`root #````
rc-service tigervnc.1 start
```
`root #``rc-service tigervnc.2 start`
Start the server(s) at startup:

`root #````
rc-update add tigervnc.1 default
```
`root #``rc-update add tigervnc.2 default`
Even having only one display requires creating a symlink.

#### systemd

Start the server:

`root #``systemctl enable vncserver@:<display>.service`
for each `:display` in `/etc/tigervnc/vncserver.users`

## Usage

### Connecting

`user $``vncviewer server:1`
#### Connect over ssh with high resolution

`user $````
vncviewer -Fullcolor -QualityLevel 9 -via user@remotehost localhost:1
```
`user $````
vncviewer -Fullcolor -QualityLevel 9 -via user2@remotehost localhost:2
```
## See also

- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
