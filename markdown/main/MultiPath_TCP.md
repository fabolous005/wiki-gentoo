<!-- source: https://wiki.gentoo.org/wiki/MultiPath_TCP | group: Gentoo Wiki (Main) | wiki-title: MultiPath TCP -->
---
title: MultiPath TCP
url: https://wiki.gentoo.org/wiki/MultiPath_TCP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-26"
fingerprint: ee9504d619b54ba6
license: CC BY-SA 4.0
---

# MultiPath TCP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

MultiPath TCP (MPTCP) is an effort towards enabling the simultaneous use of several IP-addresses/interfaces by a modification of TCP that presents a regular TCP interface to applications, while in fact spreading data across several subflows. Benefits of this include better resource utilization, better throughput and smoother reaction to failures.

## Installation =

### Kernel

Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item. --->
   Networking options --->
     \[\*\] TCP/IP networking [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INET\</code> to find this item.
       \[\*\]   MPTCP: Multipath TCP [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MPTCP\</code> to find this item.
       \[\*\]     MPTCP: IPv6 support for Multipath TCP [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MPTCP\_IPV6\</code> to find this item.



### Emerge

`root #``emerge --ask net-misc/mptcpd`


## Services

### systemd

`root #``systemctl enable avahi-daemon.service``root #``systemctl start avahi-daemon.service`
### OpenRC

TODO: There is no OpenRC init script for this yet.
