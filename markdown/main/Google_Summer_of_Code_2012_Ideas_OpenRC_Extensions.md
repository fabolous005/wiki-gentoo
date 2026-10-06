<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/OpenRC_Extensions | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/OpenRC Extensions -->
---
title: Google Summer of Code/2012/Ideas/OpenRC Extensions
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/OpenRC_Extensions
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-02"
fingerprint: "9a16bae887a18bac"
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/OpenRC Extensions

From Gentoo Wiki

\< [Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) | [2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) | [Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [OpenRC Extensions]

OpenRC is the default init system in Gentoo, it provides a large deal of features while staying mostly agnostic to the underlying implementation on /sbin/init.

The project aims to be a constructive criticism to the systemd approach by providing the few interesting features not already implemented by OpenRC as stand alone modules allowing integrator not to need to bend their system layouts to accomodate the init system.

Desired extensions include:

- A mechanism by which init scripts can configure OpenRC to detect runtime failures, log them and respond to them. The key response we want to enable is to give regular init scripts respawn functionality like we have in /etc/inittab
- Oom-killer protection via /proc/\*/oom\_adj
- The ability to perform some sort of maintenance action on a timer (e.g. restart)



| Contacts | Required Skills | 
|---|---|
|  |  | 

## Daemons in Gentoo Prefix with OpenRC

Objective:

- Port OpenRC to Gentoo Prefix to organize daemons.


Abstract:

- I am going to take the development of prefix support in OpenRC
- Deploy OpenRC to work with baselayout in Prefix and
- Extend Prefix with the long-waited feature of services daemons.

#### Mailing List Archives

[gentoo-soc - report 7.16-7.23: improving OpenRC heroxbd@×××××.com Tue, 24 Jul 2012 09:06:29](https://archives.gentoo.org/gentoo-soc/message/f768d78d3bf0d1e1069af2a8c9b82d50) 

**Contacts:**
