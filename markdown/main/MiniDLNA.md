<!-- source: https://wiki.gentoo.org/wiki/MiniDLNA | group: Gentoo Wiki (Main) | wiki-title: MiniDLNA -->
---
title: MiniDLNA
url: https://wiki.gentoo.org/wiki/MiniDLNA
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-05"
fingerprint: c58a2f4311e139f1
license: CC BY-SA 4.0
---

# MiniDLNA

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**MiniDLNA** (also known as ReadyMedia) is a media server aiming to be DLNA/UPnP-AV compliant.

## Installation

### USE flags


### Emerge

Install [net-misc/minidlna](https://packages.gentoo.org/packages/net-misc/minidlna):

`root #``emerge --ask net-misc/minidlna`
## Configuration

Configuration is controlled by /etc/minidlna.conf these settings should be changed.

**`/etc/minidlna.conf`**

### Automatic file discovery

MiniDLNA supports [inotify](https://en.wikipedia.org/wiki/Inotify) monitoring to automatically discover new files. To enable inotify support the kernel needs to have inotify support enabled.

**Enabling inotify support**

And `inotify=yes` set in the MiniDLNA configuration file.

**`/etc/minidlna.conf`**

### Manual database rebuild

`root #``rc-service minidlna stop && minidlnad -R`
### Port forwarding

MiniDLNA uses UDP port 1900 & TCP port 8200.

### Permissions

The service runs as user minidlna. Thus directories and files like the database in *db\_dir* or the log file in *log\_dir* need correct ownership. Owner must be minidlna.

### Discovery Problems

- do not use spaces for *friendly\_name*

### Service

#### OpenRC

Start MiniDLNA:

`root #``rc-service minidlna start`
Start MiniDLNA at boot:

`root #``rc-update add minidlna default`
#### systemd

Start MiniDLNA:

`root #``systemctl start minidlna`
Start MiniDLNA at boot:

`root #``systemctl enable minidlna`
