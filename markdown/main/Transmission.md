<!-- source: https://wiki.gentoo.org/wiki/Transmission | group: Gentoo Wiki (Main) | wiki-title: Transmission -->
---
title: Transmission
url: https://wiki.gentoo.org/wiki/Transmission
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: "3e88c15d2906b9e5"
license: CC BY-SA 4.0
---

# Transmission

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Transmission ([net-p2p/transmission](https://packages.gentoo.org/packages/net-p2p/transmission)) is a fast, easy, and free BitTorrent client.

## Installation

### USE flags


| [appindicator](https://packages.gentoo.org/useflags/appindicator) | Build in support for notifications using the libindicate or libappindicator plugin | 
| [cli](https://packages.gentoo.org/useflags/cli) | Build command-line client | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable assertions | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Use the following command to install transmission-daemon:

`root #``emerge --ask net-p2p/transmission`
## Configuration

Before changing configuration remember to stop the transmission-daemon first.

### As a user

Before making any changes stop the user service.

`user $``killall transmission-daemon`
When using transmission as a user service, configuration can be changed in the \~/.config/transmission-daemon/settings.json file.

### As a system service

Before making any changes stop the system service.

`root #``rc-service transmission-daemon stop`
When using transmission as a system service, configuration can be changed in the /var/lib/transmission/config/settings.json file.

To allow all LAN machines to connect, change the rpc-whitelist field to: "192.168.\*.\*".

If changing the rpc-password field, after starting the service the plaintext password will be hashed and prefixed with a '{'.

## Usage

### As a user

### As a system service

`root #````
rc-service transmission-daemon start
```
`root #````
rc-update add transmission-daemon default
```
To test if the service is running, you can open up the transmission web gui: http://\<IP\_ADDRESS>:9091/transmission

## Clients

[net-p2p/transmission](https://packages.gentoo.org/packages/net-p2p/transmission) offers several official clients, which can be installed with corresponding USE flags. Enable them and rebuild package.

There also third-party clients that can communicate with transmission daemon, like [net-p2p/tremc](https://packages.gentoo.org/packages/net-p2p/tremc).
