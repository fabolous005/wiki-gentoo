<!-- source: https://wiki.gentoo.org/wiki/Pidgin | group: Gentoo Wiki (Main) | wiki-title: Pidgin -->
---
title: Pidgin
url: https://wiki.gentoo.org/wiki/Pidgin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-25"
fingerprint: "7e01d94c408279d8"
license: CC BY-SA 4.0
---

# Pidgin

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Pidgin is an easy to use and free chat client that supports AIM, Google Talk, ICQ, IRC, XMPP, and more chat networks all at once.

## Installation

### USE flags


| [+gstreamer](https://packages.gentoo.org/useflags/+gstreamer) | Enables voice and video sessions | 
| [+gui](https://packages.gentoo.org/useflags/+gui) | Enable support for a graphical user interface | 
| [+xscreensaver](https://packages.gentoo.org/useflags/+xscreensaver) | Use X screensaver protocol extension to monitor idle/active status based on mouse/keyboard events | 
| [aqua](https://packages.gentoo.org/useflags/aqua) | Include support for the Mac OS X Aqua (Carbon/Cocoa) GUI | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [eds](https://packages.gentoo.org/useflags/eds) | Enable support for Evolution-Data-Server (EDS) | 
| [gadu](https://packages.gentoo.org/useflags/gadu) | Enable Gadu Gadu protocol support | 
| [gnutls](https://packages.gentoo.org/useflags/gnutls) | Prefer net-libs/gnutls as SSL/TLS provider (ineffective with USE=-ssl) | 
| [groupwise](https://packages.gentoo.org/useflags/groupwise) | Enable Novell Groupwise protocol support | 
| [idn](https://packages.gentoo.org/useflags/idn) | Enable support for Internationalized Domain Names | 
| [meanwhile](https://packages.gentoo.org/useflags/meanwhile) | Enable meanwhile support for Sametime protocol | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Build finch, console interface | 
| [networkmanager](https://packages.gentoo.org/useflags/networkmanager) | Enable net-misc/networkmanager support | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [perl](https://packages.gentoo.org/useflags/perl) | Add optional support/bindings for the Perl language | 
| [prediction](https://packages.gentoo.org/useflags/prediction) | Enable Contact Availability Prediction plugin | 
| [python](https://packages.gentoo.org/useflags/python) | Build libgnt (GLib Ncurses Toolkit used by finch) with python scripting support | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Add support for the Simple Authentication and Security Layer | 
| [spell](https://packages.gentoo.org/useflags/spell) | Add dictionary support | 
| [tcl](https://packages.gentoo.org/useflags/tcl) | Add support the Tcl language | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tk](https://packages.gentoo.org/useflags/tk) | Add support for Tk GUI toolkit | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [zephyr](https://packages.gentoo.org/useflags/zephyr) | Enable Zephyr protocol support | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

`root #``emerge --ask net-im/pidgin`
## Configuration

Pidgin's [guide](https://developer.pidgin.im/wiki/Using%20Pidgin) is a complete guide through the process of configuring every aspect of pidgin.

## Usage

Pidgin comes with a GUI based on GTK that lets you interact with different protocols and stay connected to different servers as described in the Pidgin's [guide](https://developer.pidgin.im/wiki/Using%20Pidgin)

## Plugins

Pidgin has a community of third-party developed plugins. You can find out more on the third party plugins at the [official wiki page](https://developer.pidgin.im/wiki/ThirdPartyPlugins).

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-im/pidgin`
### Configuration and logs

After unmerging, for a full clean-up, be sure to remove old logs and individual user configuration files from their default location in each pidgin user's home directory:

`user $``rm -rf ~/.purple`
## See also

- [Irssi](https://wiki.gentoo.org/wiki/Irssi) — a powerful text-mode IRC client for connecting to internet relay chat (IRC) networks.
- [WeeChat](https://wiki.gentoo.org/wiki/WeeChat) — a light, extensible, actively maintained, well documented, highly featured text-mode [IRC](https://wiki.gentoo.org/wiki/IRC) client.

## External resources

- [FAQ (official)](https://developer.pidgin.im/wiki/FAQ) - A list of Frequently Asked Questions concerning Pidgin.
- [Using Pidgin (official)](https://developer.pidgin.im/wiki/Using%20Pidgin) -Pidgin's usage and configurations.
