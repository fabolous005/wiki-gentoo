<!-- source: https://wiki.gentoo.org/wiki/Dhcpcd | group: Gentoo Wiki (Main) | wiki-title: Dhcpcd -->
---
title: dhcpcd
url: https://wiki.gentoo.org/wiki/Dhcpcd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-02"
fingerprint: "9a5b691d199a89cd"
license: CC BY-SA 4.0
---

# dhcpcd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**D**ynamic **H**ost **C**onfiguration **P**rotocol **C**lient **D**aemon ([net-misc/dhcpcd](https://packages.gentoo.org/packages/net-misc/dhcpcd)) is a popular DHCP client capable of handling both IPv4 and IPv6 configuration.

## Installation

### USE flags


| [+embedded](https://packages.gentoo.org/useflags/+embedded) | Embed the definitions of dhcp options in the dhcpcd executable | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [ipv6](https://packages.gentoo.org/useflags/ipv6) | Add support for IP version 6 | 
| [privsep](https://packages.gentoo.org/useflags/privsep) | Enable support for privilege separation | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

Use the following command to install dhcpcd:

`root #``emerge --ask net-misc/dhcpcd`
## Configuration

### Files

All dhcpcd configuration can be set in the /etc/dhcpcd.conf file, but for most installations dhcpcd will work out of the box presuming most computers nowadays are behind a router or access point running a DHCP server. Though man 5 dhcpcd.conf<sup>[\[1\]](https://wiki.gentoo.org#cite_note-manpage-1)</sup> will be helpful in case advanced configuration is required.

### Static IP addresses

In case the network interface card should be configured with a [static IP address](https://wiki.gentoo.org/wiki/Static_routing), add their data to /etc/dhcpcd.conf.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-manpage-1)</sup> The following is an example of manually adding a static address, routes, and DNS by editing DHCPCD's configuration file using a text editor of choice:

**`/etc/dhcpcd.conf`**

### Static values for domain/search in resolv.conf

**`/etc/dhcpcd.conf`**

### IPv6 Prefix Request

**`/etc/dhcpcd.conf`**

**Request a prefix for*eth0.lan* and *eth0.management* to be routed publicly with *eth0.wan*.**

## Usage

### Invocation

`root #``dhcpcd --help````
usage: dhcpcd	[-146ABbDdEGgHJKLMNPpqTV]
		[-C, --nohook hook] [-c, --script script]
		[-e, --env value] [-F, --fqdn FQDN] [-f, --config file]
		[-h, --hostname hostname] [-I, --clientid clientid]
		[-i, --vendorclassid vendorclassid] [-j, --logfile logfile]
		[-l, --leasetime seconds] [-m, --metric metric]
		[-O, --nooption option] [-o, --option option]
		[-Q, --require option] [-r, --request address]
		[-S, --static value]
		[-s, --inform address[/cidr[/broadcast_address]]]
 [--inform6]		[-t, --timeout seconds] [-u, --userclass class]
		[-v, --vendor code, value] [-W, --whitelist address[/cidr]] [-w]
		[--waitip [4 | 6]] [-y, --reboot seconds]
		[-X, --blacklist address[/cidr]] [-Z, --denyinterfaces pattern]
		[-z, --allowinterfaces pattern] [--inactive] [interface] [...]
       dhcpcd	-n, --rebind [interface]
       dhcpcd	-k, --release [interface]
       dhcpcd	-U, --dumplease interface
       dhcpcd	--version
       dhcpcd	-x, --exit [interface]
```
### Run it as a service

See [Network management using DHCPCD](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD).

### Manually starting dhcpcd

dhcpd can be manually started on a specific interface, such as enp1s0 with:

`root #``dhcpcd enp1s0`
Be sure to replace `enp1s0` in the command above with the appropriate network interface name.

### Renew a lease

To renew the lease on enp1s0, **--rebind** or **-n** can be used:

`root #``dhcpcd -n enp1s0`
### Release a lease

To release a lease on enp1s0, **--release** or **-k** can be used:

`root #``dhcpcd -k enp1s0`
## Troubleshooting

- [dhcpcd not working for IPv6 (#CONFIG\_PACKET is not set)](https://forums.gentoo.org/viewtopic-t-1068686.html)
- If /etc/resolv.conf does not update with VPN's DNS, consider installing [net-dns/openresolv](https://packages.gentoo.org/packages/net-dns/openresolv).

## See also

- [The Handbook's recommendations on handling network interfaces](https://wiki.gentoo.org/wiki/Handbook:AMD64/Networking/Introduction)
- [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) — Gentoo's default framework for configuring and [managing network](https://wiki.gentoo.org/wiki/Network_management) interfaces on systems running [OpenRC](https://wiki.gentoo.org/wiki/OpenRC).
- [Network management using DHCPCD](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD) — explains how to use dhcpcd for complete network stack management.

## External resources

- [Dhcpcd on the Arch Wiki](https://wiki.archlinux.org/index.php/Dhcpcd)
- [Forums post: No Internet access when update to dhcpcd 6.10.0](https://forums.gentoo.org/viewtopic-p-7870084.html#7870084)
- [DHCP](https://en.wikipedia.org/wiki/DHCP) on Wikipedia
- [RFC 2131 - Dynamic Host Configuration Protocol](https://tools.ietf.org/html/rfc2131)

## References
