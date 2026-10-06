<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/oldnet_on_systemd | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2013/Ideas/oldnet on systemd -->
---
title: Google Summer of Code/2013/Ideas/oldnet on systemd
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/oldnet_on_systemd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "970eb0441cc157ff"
license: CC BY-SA 4.0
---

# Google Summer of Code/2013/Ideas/oldnet on systemd

From Gentoo Wiki

\< [Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) | [2013](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013) | [Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [oldnet on systemd]

It has been proposed in the past that oldnet splits from OpenRC, and becomes an independent package, for ease of maintenance. At the same time, oldnet is a viable contender against other advanced network configuration systems, such as netctl. For this project, you would implement a compatibility layer for oldnet's usage of OpenRC functionality, and provide a systemd service to run each net.$IFACE service, via symlinks, similar to the existing net.lo to net.iface symlinks. This project would also encourage usage of oldnet OUTSIDE of Gentoo, so while any applicant should be prepared to start with oldnet on Gentoo, testing on a non-Gentoo distribution towards the end of the project should be included in any project proposal.



| Contacts | Required Skills | 
|---|---|
|  |  |
