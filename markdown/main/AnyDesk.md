<!-- source: https://wiki.gentoo.org/wiki/AnyDesk | group: Gentoo Wiki (Main) | wiki-title: AnyDesk -->
---
title: AnyDesk
url: https://wiki.gentoo.org/wiki/AnyDesk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-18"
fingerprint: "42abb570fb5981f7"
license: CC BY-SA 4.0
---

# AnyDesk

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**AnyDesk** is a proprietary (closed source), software for remote access and support over the Internet similar to [TeamViewer](https://wiki.gentoo.org/wiki/TeamViewer).

## Installation

### USE flags


### Emerge

Accept AnyDesk TOS license.

**`/etc/portage/package.license`**

`root #``emerge --ask net-misc/anydesk`
### Service

AnyDesk service can be run manually with

`user $``anydesk --service`
AnyDesk service can also be turned on so that it runs every time the system boots.

#### OpenRC

Start AnyDesk during system start:

`root #``rc-update add anydesk default`
Start the AnyDesk daemon now:

`root #``rc-service anydesk start`
#### systemd

Start AnyDesk during system start:

`root #``systemctl enable anydesk`
Start the AnyDesk daemon now:

`root #``systemctl start anydesk`
## Troubleshooting

If [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) is not used, AnyDesk may not be able to receive connection. In case it is undesirable to install [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), AnyDesk can be run with PULSE\_SERVER=0 environment variable:

`user $``PULSE_SERVER=0 anydesk --service`
systemd users can add this into the AnyDesk service

`root #``systemctl edit anydesk`
**`/etc/systemd/system/anydesk.service.d/override.conf`**

openrc users can export this from the conf.d/anydesk file:

**`/etc/conf.d/anydesk`**

## See also

- [TigerVNC](https://wiki.gentoo.org/wiki/TigerVNC) — a client/server software package allowing remote network access to graphical desktops.
- [TeamViewer](https://wiki.gentoo.org/wiki/TeamViewer) — a proprietary (closed source), all-in-one solution for remote access and support over the Internet.
