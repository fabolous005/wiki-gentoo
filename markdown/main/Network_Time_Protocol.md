<!-- source: https://wiki.gentoo.org/wiki/Network_Time_Protocol | group: Gentoo Wiki (Main) | wiki-title: Network Time Protocol -->
---
title: Network Time Protocol
url: https://wiki.gentoo.org/wiki/Network_Time_Protocol
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-20"
fingerprint: f56d7f36e0892ad0
license: CC BY-SA 4.0
---

# Network Time Protocol

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Network Time Protocol (NTP) is used to synchronize the [system time](https://wiki.gentoo.org/wiki/System_time) with other devices over the network. This  happens in a client-server model.

## Implementations

Following implementations of the Network Time Protocol are currently available:

| Name | Package | Description | 
|---|---|---|
| [chrony](https://wiki.gentoo.org/wiki/Chrony) | [net-misc/chrony](https://packages.gentoo.org/packages/net-misc/chrony) | Versatile implementation of the Network Time Protocol. | 
| clockspeed | [net-misc/clockspeed](https://packages.gentoo.org/packages/net-misc/clockspeed) | Simple Network Time Protocol (NTP) client. | 
| [ntp](https://wiki.gentoo.org/wiki/Ntp) | [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) | Suite of tools utilizing Network Time Protocol. | 
| ntpsec | [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec) | NTP reference implementation, refactored. | 
| [openntpd](https://wiki.gentoo.org/wiki/Openntpd) | [net-misc/openntpd](https://packages.gentoo.org/packages/net-misc/openntpd) | Lightweight NTP server ported from OpenBSD. | 
| sntpd | [net-misc/sntpd](https://packages.gentoo.org/packages/net-misc/sntpd) | NTP (RFC-1305 and RFC-4330) client and server for unix(like) systems. | 

## Sync system clock on boot

It is important for most systems to keep accurate time, so it is usual to set up an NTP time synchronization solution to adjust system time on boot. This can be essential for some systems that do not have an [RTC](https://wiki.gentoo.org/wiki/System_time#Software_clock_vs_Hardware_clock).

The handbook gives an example of an easy way to [setup time synchronization](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools#Suggested:_Time_synchronization).

## See also

- [Pi4 Stratum 1 Time Server](https://wiki.gentoo.org/wiki/Pi4_Stratum_1_Time_Server) — setting up a [Raspberry Pi 4/5](https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install) as a Stratum 1 Time Server
- [System time](https://wiki.gentoo.org/wiki/System_time) — is used in Unix systems to keep track of time.
