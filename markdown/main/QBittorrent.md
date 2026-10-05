<!-- source: https://wiki.gentoo.org/wiki/QBittorrent | group: Gentoo Wiki (Main) | wiki-title: QBittorrent -->
---
title: QBittorrent
url: https://wiki.gentoo.org/wiki/QBittorrent
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-29"
fingerprint: "9689a25b1f0a4fd5"
license: CC BY-SA 4.0
---

# QBittorrent

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**qBittorrent** is an open source alternative BitTorrent client.

qBittorrent aims for a polished and intuitive interface, that is similar to the µTorrent UI. Additionally, qBittorrent runs and provides the same features on all major platforms (FreeBSD, Linux, macOS, OS/2, Windows). qBittorrent features a WebUI that can be used when running a headless server, and alternative WebUIs are also available.

qBittorrent is based on the Qt toolkit and libtorrent-rasterbar library.

## Installation

### USE flags


| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable support for notifications and power-management features via D-Bus | 
| [+gui](https://packages.gentoo.org/useflags/+gui) | Enable support for a graphical user interface | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [webui](https://packages.gentoo.org/useflags/webui) | Install qBittorrent Web UI (qbittorrent-nox) | 

### Emerge

To install qBittorrent:

`root #``emerge --ask net-p2p/qbittorrent`
## Configuration

Typically, the inbuilt GUI is used to manage settings, though the configuration files can be manually edited (with appropriate care).

### Files

- \~/.config/qBittorrent/qBittorrent.conf - main configuration file for a user.

### Service for webui

The qBittorrent WebUI service can be started when system starts or reboots. See the [WebUI](https://wiki.gentoo.org/wiki/QBittorrent#WebUI) section for how to access and use the WebUI.

Note that the availability of the WebUI is dependent on the [webui](https://packages.gentoo.org/useflags/webui) [USE flag being set.](https://wiki.gentoo.org/wiki/USE_flag) 

#### OpenRC

`root #``rc-update add qbittorrent default`
To be able to see the temporary password, log in and set a permanent auth solution, use [sudo](https://wiki.gentoo.org/wiki/Sudo) or [doas](https://wiki.gentoo.org/wiki/Doas) to run the qbittorrent-nox command-line client as the `qbittorrent` user, e.g.:

`root #``sudo -u qbittorrent qbittorrent-nox`
#### systemd

`root #``systemctl enable qbittorrent`
## Usage

### Invocation

qBittorrent offers both a GUI to be run on a graphical desktop, and a WebUI that can be accessed from a web browser.

#### GUI

To start the qBittorrent GUI, simply run:

`user $``qbittorrent`
#### WebUI

For WebUI, the USE flag [webui](https://packages.gentoo.org/useflags/webui) [has to be added. To open qBittorrent in](https://wiki.gentoo.org/wiki/USE_flag) *WebUI* mode, run the following:

`user $``qbittorrent-nox`
The WebUI can be accessed using a [web browser](https://wiki.gentoo.org/wiki/Category:Web_browser). Assuming the WebUI is running on the same system, a socket address `localhost:8080` should work. The *default* qBittorrent port is :8080

## Tips

### Resetting login credentials

Should WebUI login credentials be lost, the login credentials can be reset without **losing** any configuration or torrents. Before performing a reset, qBittorrent should be **stopped** first to avoid the process overwriting the configuration:

#### OpenRC

`root #``rc-service qbittorrent stop`
#### systemd

`root #``systemctl stop qbittorrent`
Edit the configuration file:

**`"~/.config/qBittorrent/qBittorrent.conf"`**

Save the *qBittorrent.conf* and start qBittorrent WebUI in terminal. The output should look like, as shown below:

`user $``qbittorrent-nox`
WebUI will be started shortly after internal preparations. Please wait...
\*\*\*\*\*\*\*\* Information \*\*\*\*\*\*\*\*
To control qBittorrent, access the WebUI at: http://localhost:8080
The WebUI administrator username is: admin
The WebUI administrator password was not set. A temporary password is provided for this session: NKB7ALQ6M
You should set your own password in program preferences.

Access the WebUI with the temporary password provided in the output and change the login credentials there.

## Removal

qBittorrent can be removed by unmerging it:

`root #``emerge --ask --depclean --verbose net-p2p/qbittorrent`
## See also

[BitTorrent](https://wiki.gentoo.org/wiki/BitTorrent) — a decentralized file sharing protocol.

## External resources

- [https://github.com/qbittorrent/qBittorrent/wiki/Explanation-of-Options-in-qBittorrent](https://github.com/qbittorrent/qBittorrent/wiki/Explanation-of-Options-in-qBittorrent) - Detailed explanation on settings options
