<!-- source: https://wiki.gentoo.org/wiki/IPv6_router_guide | group: Gentoo Wiki (Main) | wiki-title: IPv6 router guide -->
---
title: IPv6 router guide
url: https://wiki.gentoo.org/wiki/IPv6_router_guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-11"
categories: ['net-misc']
fingerprint: beacc558bda4a9c1
license: CC BY-SA 4.0
---

# IPv6 router guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This guide provides details on setting up a Gentoo Linux system as an [IPv6](https://wiki.gentoo.org/wiki/IPv6) router.

## Installation

### Kernel

Any kernels version v2.6.0 or higher can support [IPv6](https://www.linux-ipv6.org/).

`root #``emerge --ask sys-kernel/gentoo-sources`
**Required IPv6 options**

### Emerge

`root #``emerge --ask sys-apps/iproute2``root #``emerge --ask net-misc/radvd`
#### Additional software

There are a few packages which specifically deal with IPv6 items. Most of these are located in the [net-misc](https://packages.gentoo.org/categories/net-misc) category.

| Package | Description | 
|---|---|
| [net-misc/radvd](https://packages.gentoo.org/packages/net-misc/radvd) | Router advertisement daemon | 
| [net-misc/dhcpd](https://packages.gentoo.org/packages/net-misc/dhcpd) | ISC DHCP server, DHCPv4 and DHCPv6 capability | 
| [net-misc/dibbler](https://packages.gentoo.org/packages/net-misc/dibbler) | DHCPv6 server | 
| [net-misc/ipv6calc](https://packages.gentoo.org/packages/net-misc/ipv6calc) | Converts an IPv6 address to a compressed format | 
| [dev-perl/Socket6](https://packages.gentoo.org/packages/dev-perl/Socket6) | IPv6 related part of the C socket.h defines and structure manipulators | 

### Confirming IPv6 status

If IPv6 is enabled, the loopback device should show an IPv6 address:

`root #``ip -6 addr show lo````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
```
## Configuration

### Obtaining an address and prefix

dhcpcd can be used to obtain a single, host only, /128 IPv6 address for the *WAN* interface, and a /64 IPv6 prefix for the *LAN* interface.

**`/etc/dhcpcd.conf`**

**Request a IPv6 prefix for*eth0.lan* and *eth0.management* to be routed publicly with *eth0.wan*.**

### Enable forwarding

IPv6 packet forwarding must be enabled in kernel before a system can function as a router, this can be done using sysctl:

`root #``sysctl -w net.ipv6.conf.all.forwarding=1`
To assign IPv6 addresses to clients, the IPv6 specification allows both methods, stateless and stateful IP assignment. The IPv6 Stateless Address Autoconfiguration uses a process called Router Advertisement and allows clients to obtain an IP and a default route by simply bringing an interface up. It is called "stateless" because there is no record of IPs assigned and the host they are assigned to. Stateful assignment is handled by DHCPv6. It is "stateful" because the server keeps a state of the clients who have requested IPs and received them.

### Stateless configuration

Stateless configuration is easily accomplished using the Router Advertisement Daemon, or radvd:

/etc/radvd.conf is used to configure radvd, and is not created by default. If the IPv6 prefix configuration is left empty, the already assigned or configured IPv6 prefix is used:

**`/etc/radvd.conf`**

**Router Advertisement (RA) configuration for the*eth0.lan* interface.**

### Stateful configuration

To have a stateful configuration, install and configure [net-misc/dibbler](https://packages.gentoo.org/packages/net-misc/dibbler).

`root #``emerge --ask dibbler`
Configure the dibbler client by editing /etc/dibbler/client.conf.

Now start the dibbler client, and configure it to start at boot:

`root #````
rc-service dibbler-client start
```
`root #````
rc-update add dibbler-client default
```
### Service

#### OpenRC

To start radvd and start it on boot:

`root #````
rc-service radvd start
```
`root #````
rc-update add radvd default
```
## DNS setup

### IPv6 and DNS

Just as DNS for IPv4 uses A records, DNS for IPv6 uses AAAA records. (This is because IPv4 is an address space of 2^32 while IPv6 is an address space of 2^128). For reverse DNS, the INT standard is deprecated but still widely supported. ARPA is the latest standard. Support for the ARPA format will be described here.

### BIND configuration

Recent versions of BIND include excellent IPv6 support. This section will assume at least minimal knowledge about the configuration and use of BIND. We will assume that bind is not running in a chroot. If this assumption is wrong, simply append the chroot prefix to most of the paths in the following section.

First add entries for both forward and reverse DNS zone files in /etc/bind/named.conf.

**`/etc/bind/named.conf`**

**named.conf entries**

Now zone files and entries will need added for all hosts:

**`/etc/bind/pri/ipv6-rules.com`**

**`/etc/bind/pri/ipv6-rules.com.arpa`**

### DJBDNS configuration

There are currently some third-party patches available to the [net-dns/djbdns](https://packages.gentoo.org/packages/net-dns/djbdns) package that allow it to do IPv6 name serving. DJBDNS can be installed with these patches by emerging it with `ipv6` in the USE variable.

`root #``emerge --ask net-dns/djbdns`
After djbdns is installed, it can be setup by running tinydns-setup and answering a few questions about which IP addresses to bind to, where to install tinydns, etc.

`root #``tinydns-setup`
Assuming tinydns has been installed into /var/tinydns, edit /var/tinydns/root/data. This file will contain all the data needed to get tinydns handling DNS for the IPv6 delegation.

Lines prefixed with a `6` will have both an AAAA and a PTR record created. Those prefixed with a `3` will only have an AAAA record created. Besides manually editing the data file, it is possible to use the scripts add-host6 and add-alias6 to add new entries. After changes are made to the data file, simply run `make` from /var/tinydns/root. This will create /var/tinydns/root/data.cfb, which tinydns will use as its source of information for DNS requests.

## IPv6 clients

### Using radvd

Clients behind this router should now be able to connect to the rest of the net via IPv6. If using radvd, configuring hosts should be as easy as bringing the interface up. (This is probably already done by the net.ethX init scripts).

`root #````
ip link set eth0 up
```
`root #``ip addr show eth0````
1: eth0: <BROADCAST,MULTICAST,UP> mtu 1400 qdisc pfifo_fast qlen 1000
    link/ether 00:01:03:2f:27:89 brd ff:ff:ff:ff:ff:ff
    inet6 2001:470:1f00:296:209:6bff:fe06:b7b4/128 scope global
       valid_lft forever preferred_lft forever
    inet6 fe80::209:6bff:fe06:b7b4/64 scope link
       valid_lft forever preferred_lft forever
    inet6 ff02::1/128 scope global
       valid_lft forever preferred_lft forever
```
Should this not work ensure that the IPv6 firewall is allowing ICMPv6 packets through:

`root #``ip6tables -A INPUT -p icmpv6 -j ACCEPT`
## Troubleshooting

### Package is missing IPv6 support

Packages will typically emerge with the `ipv6` *USE* flag, but if IPv6 is not working on a specific program, checking that it is built with that is a good first step.

## See Also

- [IPv6](https://wiki.gentoo.org/wiki/IPv6) — the most recent version of the [Internet Protocol](https://en.wikipedia.org/wiki/Internet_Protocol) (IP)
- [IPv6 tunnels](https://wiki.gentoo.org/wiki/IPv6_tunnels)

## External resources

There are many excellent resources online pertaining to IPv6.

- [www.ipv6.org](http://www.ipv6.org/) - General IPv6 information
- [www.linux-ipv6.org/](http://www.linux-ipv6.org/) - USAGI project
- [www.deepspace6.net](http://www.deepspace6.net/) - Linux/IPv6 site
- [www.kame.net](http://www.kame.net/) - \*BSD implementation
- [RFC 4861 - Neighbor Discovery for IP version 6 (IPv6)](https://www.rfc-editor.org/rfc/rfc4861.html)
- [RFC 4862 - IPv6 Stateless Address Autoconfiguration](https://www.rfc-editor.org/rfc/rfc4862.html)

On IRC, try the [#ipv6](ircs://irc.libera.chat/#ipv6) ([webchat](https://web.libera.chat/#ipv6)) channel on [Libera.Chat](https://www.libera.chat/). Connect to the Libera.Chat servers using an IPv6 enabled client by connecting to irc.ipv6.libera.chat.
