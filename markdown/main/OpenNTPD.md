<!-- source: https://wiki.gentoo.org/wiki/OpenNTPD | group: Gentoo Wiki (Main) | wiki-title: OpenNTPD -->
---
title: OpenNTPD
url: https://wiki.gentoo.org/wiki/OpenNTPD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-29"
fingerprint: f3a334707f808be6
license: CC BY-SA 4.0
---

# OpenNTPD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

OpenNTPD is a lightweight [NTP](https://wiki.gentoo.org/wiki/Network_Time_Protocol) server ported from OpenBSD.

## Installation

### USE flags


### Emerge

Install OpenNTPD:

`root #``emerge --ask net-misc/openntpd`
### Service

To ask OpenNTPD to attempt clock sync on startup, edit /etc/conf.d/ntpd:

**`/etc/conf.d/ntpd`**

**Gentoo OpenNTPD configuration file**

Note that if the clock drift is sufficiently large, you may first need to set the time manually using date(1).

#### OpenRC

Start the ntp daemon:

`root #``/etc/init.d/ntpd start`
Add the ntp daemon to the default runlevel:

`root #``rc-update add ntpd default`
#### systemd

To enable and start the service now:

`root #``systemctl enable --now ntpd`
## Configuration

The default configuration file for OpenNTPD:

**`/etc/ntpd.conf`**

**Gentoo's default configuration**

For further information see the man page:

`user $``man ntpd.conf`
### Server

**`/etc/ntpd.conf`**

**OpenNTPD listening on 192.0.2.1 address while syncing with multiple servers**

`root #``rc-service ntpd restart`
### Client

**`/etc/ntpd.conf`**

**OpenNTPD syncing with multiple servers**

Alternatively sync the client to a server on the local network:

**`/etc/ntpd.conf`**

**OpenNTPD syncing with a single server on the local network**

`root #``rc-service ntpd restart`
## Checking the daemon operation

[net-misc/openntpd](https://packages.gentoo.org/packages/net-misc/openntpd) provides the /usr/sbin/ntpctl program to display information about the running ntpd daemon:

`root #``ntpctl -s all````
4/4 peers valid, clock unsynced, clock offset is 1048.342ms
peer
   wt tl st  next  poll          offset       delay      jitter
185.19.184.35 0.gentoo.pool.ntp.org
    1 10  2   23s   34s        41.650ms   190.744ms   227.001ms
31.14.131.188 1.gentoo.pool.ntp.org
    1 10  2    2s   31s        15.834ms   132.072ms    92.419ms
212.45.144.3 2.gentoo.pool.ntp.org
    1 10  1    8s   33s        42.733ms   216.937ms   212.899ms
188.213.165.209 3.gentoo.pool.ntp.org
    1 10  2   19s   30s        45.672ms   228.337ms   240.018ms
```
The output shows how many servers the daemon is syncing with, the status of the system clock (synced/unsynced) and the clock offset. For further information see the man page:

`user $``man ntpctl`
## See also

- [Chrony](https://wiki.gentoo.org/wiki/Chrony) — a versatile implementation of the [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) (NTP).
- [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) — used to synchronize the [system time](https://wiki.gentoo.org/wiki/System_time) with other devices over the network.
- [System time](https://wiki.gentoo.org/wiki/System_time) — is used in Unix systems to keep track of time.
- [Home router](https://wiki.gentoo.org/wiki/Home_router) — how to turn an old Gentoo machine into a router for connecting a home network to the Internet.
