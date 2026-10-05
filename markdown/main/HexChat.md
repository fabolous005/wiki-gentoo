<!-- source: https://wiki.gentoo.org/wiki/HexChat | group: Gentoo Wiki (Main) | wiki-title: HexChat -->
---
title: HexChat
url: https://wiki.gentoo.org/wiki/HexChat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-25"
fingerprint: de005a0e01e3f8e2
license: CC BY-SA 4.0
---

# HexChat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**HexChat** is a graphical IRC client based on XChat. It is written using the GTK+ windowing framework.

## Installation

### USE flags


| [+gtk](https://packages.gentoo.org/useflags/+gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [libcanberra](https://packages.gentoo.org/useflags/libcanberra) | Enable sound event support using media-libs/libcanberra | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [perl](https://packages.gentoo.org/useflags/perl) | Add optional support/bindings for the Perl language | 
| [plugin-checksum](https://packages.gentoo.org/useflags/plugin-checksum) | Build Checksum plugin (needs plugins) | 
| [plugin-fishlim](https://packages.gentoo.org/useflags/plugin-fishlim) | Build FiSHLiM plugin (needs plugins ) | 
| [plugin-sysinfo](https://packages.gentoo.org/useflags/plugin-sysinfo) | Build SysInfo plugin (needs plugins) | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [theme-manager](https://packages.gentoo.org/useflags/theme-manager) | Build the theme manager (mono) | 

### Emerge

Emerge hexchat:

`root #``emerge --ask net-irc/hexchat`
## Configuration

### Files

HexChat stores user specific configuration files in the \~/.config/hexchat/ directory.

Settings can be modified graphically via the Settings tab or using the /set command from the command-line interface.

### SSL

In HexChat, enable SSL connections in Network List (`Ctrl`+`s`)> Libera.Chat > Edit.

### Auto-reconnect

In order to automatically reconnect after sleep/hibernate mode, enter the /set net\_ping\_timeout 31 command.
