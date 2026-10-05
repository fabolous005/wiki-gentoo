<!-- source: https://wiki.gentoo.org/wiki/Utelnetd | group: Gentoo Wiki (Main) | wiki-title: Utelnetd -->
---
title: Utelnetd
url: https://wiki.gentoo.org/wiki/Utelnetd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-23"
fingerprint: a08fa31935874bf2
license: CC BY-SA 4.0
---

# Utelnetd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**utelnetd** is a small and efficient stand alone [Telnet](https://en.wikipedia.org/wiki/Telnet) server daemon. Telnet can be useful for several applications, such as a backup when updating [SSH](https://wiki.gentoo.org/wiki/SSH), and configuring industrial grade routers via console cables.

## Installation

### Emerge

Install [net-misc/utelnetd](https://packages.gentoo.org/packages/net-misc/utelnetd):

`root #``emerge --ask utelnetd`
## Configuration

### Service

To automatically start utelnetd at boot:

`root #``rc-update add utelnetd default`
To start it immediately:

`root #``rc-service utelnetd start`
## Testing

`user $``telnet localhost`
`telnet` is provided by `net-misc/telnet-bsd`.
