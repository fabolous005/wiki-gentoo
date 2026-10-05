<!-- source: https://wiki.gentoo.org/wiki/Upmpdcli | group: Gentoo Wiki (Main) | wiki-title: Upmpdcli -->
---
title: upmpdcli
url: https://wiki.gentoo.org/wiki/Upmpdcli
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-16"
fingerprint: bc513e781beb95b7
license: CC BY-SA 4.0
---

# upmpdcli

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**upmpdcli** is a free and open source UPnP media renderer front-end for Music Player Daemon (MPD). It allows MPD to be controlled with a UPnP control point (e.g. [BubbleUPnP](https://forum.xda-developers.com/showthread.php?t=1118891)). upmpdcli also allows a single UPnP library to be shared between UPnP control points/renderers and MPD, meaning MPD can be run without a database.

## Prerequisites

This article assumes that [MPD](https://wiki.gentoo.org/wiki/MPD) and a UPnP media server such as [Gerbera](https://wiki.gentoo.org/wiki/Gerbera) have been previously configured.

## Installation

### USE Flags


### Emerge

`root #``emerge --ask media-sound/upmpdcli`
## Configuration

Below is a snippet of the default upmpdcli configuration file. Options prefixed with `#` are commented out because they are set to default values. The options that need to be set are `upnpiface` or `upnpip` and `mpdhost`. If the UPnP or MPD ports have been changed from their defaults, then `upnpport` and `mpdport` should be set accordingly.

**`/etc/upmpdcli.conf`**

upmpdcli relies on MPD's curl input plugin which should be enabled by default. It can be explicitly enabled by adding the following to the MPD configuration file:

**`/etc/mpd.conf`**

Restart MPD for the changes to take effect:

`root #``rc-service mpd restart`
### Service

#### OpenRC

Start upmpdcli:

`root #``rc-service upmpdcli start`
Start upmpdcli at boot:

`root #``rc-update add upmpdcli default`
#### systemd

Start upmpdcli:

`root #``systemctl start upmpdcli`
Start upmpdcli at boot:

`root #``systemctl enable upmpdcli`
## See also

- [MPD](https://wiki.gentoo.org/wiki/MPD) — a flexible, server-side application for playing music.

## External resources

- [upmpdcli](https://www.lesbonscomptes.com/upmpdcli/upmpdcli-or-mpdupnp.html) - MPD and UPnP
