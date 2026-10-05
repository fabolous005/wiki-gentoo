<!-- source: https://wiki.gentoo.org/wiki/Remmina | group: Gentoo Wiki (Main) | wiki-title: Remmina -->
---
title: Remmina
url: https://wiki.gentoo.org/wiki/Remmina
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-31"
fingerprint: "2e032958d09eb9e4"
license: CC BY-SA 4.0
---

# Remmina

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Remmina** is a remote desktop client with support for various protocols, e.g. RDP, VNC, and SPICE.

## Installation

### USE flags


| [+appindicator](https://packages.gentoo.org/useflags/+appindicator) | Build in support for notifications using the libindicate or libappindicator plugin | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [crypt](https://packages.gentoo.org/useflags/crypt) | Add support for encryption -- using mcrypt or gpg where applicable | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [gvnc](https://packages.gentoo.org/useflags/gvnc) | Enable GVNC plugin using gtk-vnc, suitable for KVM and Vino servers | 
| [keyring](https://packages.gentoo.org/useflags/keyring) | Enable support for freedesktop.org Secret Service API password store | 
| [kwallet](https://packages.gentoo.org/useflags/kwallet) | Enable KDE Wallet plugin | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [rdp](https://packages.gentoo.org/useflags/rdp) | Enables RDP/Remote Desktop support | 
| [spice](https://packages.gentoo.org/useflags/spice) | Support connecting to SPICE-enabled virtual machines | 
| [ssh](https://packages.gentoo.org/useflags/ssh) | Enable support for SSH/SFTP protocol | 
| [vnc](https://packages.gentoo.org/useflags/vnc) | Enable VNC (remote desktop viewer) support | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [webkit](https://packages.gentoo.org/useflags/webkit) | Add support for the WebKit HTML rendering/layout engine | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

`root #``emerge --ask net-misc/remmina`
## Usage

### RDP

First, ensure Remmina is built with [net-misc/remmina\[rdp\]](https://packages.gentoo.org/packages/net-misc/remmina).

To connect to a RDP session, click the + in the top left, give it a name, and in the Protocol dropdown, choose `RDP - Remote Desktop Protocol`.

Then, put the IP or domain name with port in the Server input box, enter the username and password, then click `Save and Connect` to save the server.
