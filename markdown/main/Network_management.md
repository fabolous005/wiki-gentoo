<!-- source: https://wiki.gentoo.org/wiki/Network_management | group: Gentoo Wiki (Main) | wiki-title: Network management -->
---
title: Network management
url: https://wiki.gentoo.org/wiki/Network_management
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-05"
fingerprint: b3435aa9ca3033cc
license: CC BY-SA 4.0
---

# Network management

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

This article describes possibilities for managing the network stack. Gentoo provides several tools for bringing up networking interfaces and managing network connections. In addition, tools are available for managing dial up modem connections and for managing [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) connections and network authentication.

## Overview

After booting a Linux kernel, by default, all network interfaces are down, so something else is needed to automatically bring them up, set static addresses, obtain DHCP leases on dynamic addresses, configure routes, DNS, etc. These are the processes covered by the term "network management". Either [netifrc](https://wiki.gentoo.org/wiki/Netifrc) or [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) are usually used for this on Gentoo, or in simple situations just installing [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) will suffice.

Other specific tools are used for network authentication, [PPP](https://wiki.gentoo.org/wiki/PPP) connections, [VPN](https://wiki.gentoo.org/wiki/VPN) connections, etc.

Network management is often accomplished in Gentoo using [netifrc](https://wiki.gentoo.org/wiki/Netifrc) (the net.\* scripts described in the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Networking/Introduction)). Work is also ongoing to provide a new networking stack as part of [OpenRC](https://wiki.gentoo.org/wiki/OpenRC). When using only static interfaces, it is possible to try this out by emerging OpenRC with the `newnet`[USE flag](https://wiki.gentoo.org/wiki/USE_flag) and configuring /etc/conf.d/network and /etc/conf.d/staticroute.

**`/etc/portage/package.use`**

**Disabling netifrc and newnet**

```
sys-apps/openrc -netifrc -newnet
```
## Comparison of provided functionality

Gentoo provides several tools for managing the network stack. Some perform overall management, while others mainly perform specific sub functions, but may also perform overall management.

| Software | Manage interfaces | IP layer Including static addresses, routes, DNS | DHCP | WPA Wireless network authentication | 802.1X Wired network authentication | PPP | GUI |  |  | 
|---|---|---|---|---|---|---|---|---|---|
| Network management |  |  |  |  |  |  |  |  |  | 
| [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) | Yes | Yes | No This turned out to be a busybox dependency. Netifrc now supports net-misc/dhcpcd, net-misc/dhcp, and sys-apps/busybox. | No | No | No | No Can use gui of wpa\_supplicant |  |  | 
| [DHCPCD](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD) | Yes | Yes | Yes | No | No | No | Yes See [dhcpcd-ui](https://wiki.gentoo.org/wiki/Dhcpcd-ui) article |  |  | 
| [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) | Yes | Yes | Yes As of version 1.20 | No | No | No | Yes |  |  | 
| [systemd-networkd](https://wiki.gentoo.org/wiki/Systemd-networkd) | Yes | Yes | Yes | No | No | No | No |  |  | 
| Network authentication |  |  |  |  |  |  |  |  |  | 
| [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) | Yes | No | No | Yes | Yes | No | Yes USE flag "gui" provides wpa\_gui |  |  | 
| [iwd](https://wiki.gentoo.org/wiki/Iwd) | Yes | No | Yes As of version 0.19 | Yes | No | No | No |  |  | 
| Point-to-point protocol (PPP) |  |  |  |  |  |  |  |  |  | 
| [net-dialup/wvdial](https://packages.gentoo.org/packages/net-dialup/wvdial) | No | No | No | No | No | Yes | No |  |  | 
| [net-dialup/rp-pppoe](https://packages.gentoo.org/packages/net-dialup/rp-pppoe) | No | No | No | No | No | Yes | No |  |  | 
| [net-dialup/ppp](https://packages.gentoo.org/packages/net-dialup/ppp) | No | No | No | No | No | Yes | No |  |  | 

## Comparison of network managers

There are different solutions for overall management of network connections. The differences between them are as such:

| Software | Ethernet | Wifi | DSL | Modem | WiMAX | 3G | VPN | GUI | Boot time | 
|---|---|---|---|---|---|---|---|---|---|
| [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) | Yes | Yes | Yes | Yes | Yes | Yes | Yes | No Can use gui of wpa\_supplicant | Yes | 
| [DHCPCD](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD) | Yes | Yes | No | No | No | No | No | Yes See [dhcpcd-ui](https://wiki.gentoo.org/wiki/Dhcpcd-ui) article | Yes | 
| [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) | Yes | Yes | Yes | No | Yes | Yes | Yes | Yes | Yes | 
| [ConnMan](https://wiki.gentoo.org/index.php?title=ConnMan&action=edit&redlink=1) | Yes | Yes | Unknown | Unknown | Unknown | Unknown | Yes | Yes A fatal error occurred in the #info parser function | Yes | 
| [systemd-networkd](https://wiki.gentoo.org/wiki/Systemd-networkd) | Yes | Yes | Unknown | Unknown | Unknown | Unknown | Yes | No | Yes | 

## See also

- [Dependency behavior (OpenRC)](https://wiki.gentoo.org/wiki/OpenRC#Dependency_behavior)
- [How to keep classic network interface naming](https://wiki.gentoo.org/wiki/Udev#Optional:_Disable_or_override_predictable_network_interface_naming)
- [Sysfs](https://wiki.gentoo.org/wiki/Sysfs#Usage) — Which network interfaces are on the computer?
- [VPN](https://wiki.gentoo.org/wiki/VPN) — a list of some VPN options available in Gentoo Linux.
