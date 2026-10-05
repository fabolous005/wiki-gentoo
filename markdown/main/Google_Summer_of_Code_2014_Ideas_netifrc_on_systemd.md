<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2014/Ideas/netifrc_on_systemd | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2014/Ideas/netifrc on systemd -->
---
title: Google Summer of Code/2014/Ideas/netifrc on systemd
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2014/Ideas/netifrc_on_systemd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "978398f40a0143ef"
license: CC BY-SA 4.0
---

# Google Summer of Code/2014/Ideas/netifrc on systemd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In the past year, what was the classical network system on Gentoo, known as *oldnet*, has been split to an independent package for improving maintenance, and is now known as *netifrc*.

*netifrc* provides a lot of advanced networking functionality (VLANs, bridging, bonding, pppoe, ipv6, ADSL, CCW and more), but is still dependant on OpenRC.
It would be nice for *netifrc* to remain a viable contender against other advanced network configuration systems, such as netctl.

For this project, you would implement a compatibility layer for *netifrc* usage of OpenRC functionality (this is started already as function.sh for systemd users in Gentoo), as well as provide systemd services to run each *net.$IFACE* service, via symlinks, similar to the existing net.lo to net.iface symlinks. This project would also encourage usage of *netifrc* OUTSIDE of Gentoo, so while any applicant should be prepared to start with *netifrc* on Gentoo, testing on a non-Gentoo distribution towards the end of the project should be included in any project proposal.





| Contacts | Required Skills | 
|---|---|
|  |  |
