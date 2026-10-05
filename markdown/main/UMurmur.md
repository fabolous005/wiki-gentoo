<!-- source: https://wiki.gentoo.org/wiki/UMurmur | group: Gentoo Wiki (Main) | wiki-title: UMurmur -->
---
title: uMurmur
url: https://wiki.gentoo.org/wiki/UMurmur
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-07"
fingerprint: a01a156a3d1389f7
license: CC BY-SA 4.0
---

# uMurmur

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

uMurmur is a minimalistic server for Mumble designed to run on embedded systems (DD-WRT or OpenWRT) or older PC hardware. It does not include the Qt framework, which makes it a much smaller package and lighter on system resources.

## Installation

### USE flags


### Emerge

Install uMurmur via:

`root #``emerge --ask net-voip/umurmur`
## Configuration

For the most up to date details on configuration visit the [configuration article](https://github.com/umurmur/umurmur/wiki/Configuration) on uMurmur's GitHub page.

In short, configuration is as follows.

### Files

- /etc/conf.d/umurmurd - File that controls what options are passed to the umurmurd daemon upon starting or restarting.
- /etc/umurmur/umurmur.conf - Primary configuration file. Used to set channel names, IP addresses, group names, password, etc.

Modify the primary configuration file as desired. Be sure to set a password if the service should be private:

**`/etc/umurmur/umurmur.conf`**

### Services

#### OpenRC

After configuration is in a working state, start the server:

`root #``rc-service umurmurd start`
To have the daemon start each time the system boots add it to the default runlevel:

`root #``rc-update add umurmurd default`
To restart the service:

`root #``rc-service umurmurd restart`
#### Systemd

For systemd users, run the following command to start the daemon:

`root #``systemctl start umurmurd.service`
To have the daemon start on system boot issue:

`root #``systemctl enable umurmurd.service`
To restart the service:

`root #``systemctl restart umurmurd.service`
## See also

- [Mumble](https://wiki.gentoo.org/wiki/Mumble) — an open source, cross platform, low-latency, high quality voice over IP (VoIP) client.
