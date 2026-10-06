<!-- source: https://wiki.gentoo.org/wiki/Ddclient | group: Gentoo Wiki (Main) | wiki-title: Ddclient -->
---
title: ddclient
url: https://wiki.gentoo.org/wiki/Ddclient
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-06-28"
fingerprint: "5614974a1f116abc"
license: CC BY-SA 4.0
---

# ddclient

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


[ddclient](https://sourceforge.net/p/ddclient/wiki/Home/) is a tool to update dynamic DNS services like DynDNS or no-ip. It runs as daemon and supports many services.

## Installation

### Emerge

`root #``emerge --ask net-dns/ddclient`
## Configuration

The configuration is done in the file /etc/ddclient/ddclient.conf. It must not be world or group readable.

### no-ip.com

Take a look an example configuration for no-ip.com:

FILE **`/etc/ddclient/ddclient.conf`**

```
use=web, web=checkip.dyndns.com/, web-skip='IP Address'
protocol=dyndns2
server=dynupdate.no-ip.com
login=your_username (typically your email)
password=your_password
your_domain_in_noip.com
```
### Automatic start

To start the ddclient daemon:

`root #``rc-service ddclient start`
To have the client start at boot:

`root #``rc-update add ddclient default`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-dns/ddclient`
