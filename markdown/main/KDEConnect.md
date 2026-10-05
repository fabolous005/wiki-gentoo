<!-- source: https://wiki.gentoo.org/wiki/KDEConnect | group: Gentoo Wiki (Main) | wiki-title: KDEConnect -->
---
title: KDEConnect
url: https://wiki.gentoo.org/wiki/KDEConnect
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-16"
fingerprint: cc91521878f9786e
license: CC BY-SA 4.0
---

# KDEConnect

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**KDEConnect** is an application that lets two devices shares clipboard, files, and other information. It is similar to Apple's AirDrop in functionality. It is mainly used between a phone and a computer to easily share files and notifications.

A KDEConnect implementation for Gnome can be found at [gnome-extra/gnome-shell-extension-gsconnect](https://packages.gentoo.org/packages/gnome-extra/gnome-shell-extension-gsconnect).

## Installation

### USE flags


| [X](https://packages.gentoo.org/useflags/X) | Enable remote input mousepad/shareinputdevices plugins relying on X11 libs | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Enable system volume control plugin using media-libs/libpulse | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [telephony](https://packages.gentoo.org/useflags/telephony) | Enable telephony plugin using kde-frameworks/modemmanager-qt | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask kde-misc/kdeconnect`
### Additional software

To use this application, KDEConnect needs to be installed on the two devices which will communicate.

## Troubleshooting

### Use KDEConnect with a firewall

If your computer has a firewall (like [net-firewall/nftables](https://packages.gentoo.org/packages/net-firewall/nftables)), then the ports 1714-1764 needs to be open.
For [Nftables](https://wiki.gentoo.org/wiki/Nftables), the following snippet can be used.

**`/etc/nftables.rules.d/kdeconnect.rules`**

**Allow TCP and UDP for KDEConnect**

```
#! /sbin/nft -f
table inet filter {
        chain input {
                tcp dport 1714-1764 accept comment "accept KDE Connect functionality"
                udp dport 1714-1764 accept comment "accept KDE Connect functionality"
        }
}
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose kde-misc/kdeconnect`
## See also

- [KDE](https://wiki.gentoo.org/wiki/KDE) — a free software community, producing a wide range of applications including the popular Plasma desktop environment.
