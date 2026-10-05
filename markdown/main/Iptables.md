<!-- source: https://wiki.gentoo.org/wiki/Iptables | group: Gentoo Wiki (Main) | wiki-title: Iptables -->
---
title: iptables
url: https://wiki.gentoo.org/wiki/Iptables
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-08"
fingerprint: f784e95db4f339e4
license: CC BY-SA 4.0
---

# iptables

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


iptables is a program used to configure and manage the kernel's netfilter modules. It should be replaced with its successor [nftables](https://wiki.gentoo.org/wiki/Nftables).

## Installation

### Prerequisites

First off, configure the kernel with netfilter support. To allow adding rules based on IP filtering like black listing IP addresses based on a live feed [\[1\]](https://forums.gentoo.org/viewtopic-t-863121.html), do not forget to add [IPSet](https://wiki.gentoo.org/wiki/IPSet) support to the kernel and merge the [net-firewall/ipset](https://packages.gentoo.org/packages/net-firewall/ipset) package.

### Kernel

Kernel configuration required by iptables depends on the intended use case.

#### Client

For client computers some basic options need to be activated in the kernel. This configuration does not provide network address translation or any other high sophisticated features. In "Network packet filtering framework" only the tables "filter" are needed with connection tracking support and with `REJECT` target support.

**Kernel settings for client**

#### Router

Activate the following kernel options:

**Kernel settings for router**

One can setup the IPv6 support category as modular (*\<M>*) to be safe and enable almost all Netfilter sub-categories as well. Or, enable only what is needed and leave the other modules unset. A number of settings are almost always needed:

- *IP virtual server support* core components (scheduler are certainly optional)
- *IP: Netfilter Configuration* support
- *IPv6: Netfilter Configuration* for IPv6 support
- *IP set support* for IP filtering based on IP, MAC, ports
- pick up what is needed in *Core Netfilter Configuration* with at least:
  - Netfilter: NFQEUE, LOG;
  - Connection tracking: flow, mark, events, netlink;
  - Netfilter Xtables: NFQEUE, LOG, conn{bytes,mark,state}, state helper with Xtables match: conn{bytes,mark,state}...

### USE flags


| [conntrack](https://packages.gentoo.org/useflags/conntrack) | Build against net-libs/libnetfilter\_conntrack when enables the connlabel matcher | 
| [netlink](https://packages.gentoo.org/useflags/netlink) | Build against libnfnetlink which enables the nfnl\_osf util | 
| [nftables](https://packages.gentoo.org/useflags/nftables) | Support nftables kernel interface | 
| [pcap](https://packages.gentoo.org/useflags/pcap) | Build against net-libs/libpcap which enables the nfbpf\_compile util | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

Install iptables:

`root #``emerge --ask net-firewall/iptables`
## Firewall

### First run

For some services such as [sshguard](https://wiki.gentoo.org/wiki/Sshguard) and [fail2ban](https://wiki.gentoo.org/wiki/Fail2ban) a running firewall is mandatory. First save a blank firewall rule set and start the firewall.

#### IPv4

`root #````
rc-service iptables save
```
`root #````
rc-service iptables start
```
To start on boot:

`root #``rc-update add iptables default`
#### IPv6

`root #````
rc-service ip6tables save
```
`root #````
rc-service ip6tables start
```
To start on reboot:

`root #``rc-update add ip6tables default`
### General rules

To create firewall rules, the iptables or ip6tables commands in the next set of examples will be defined through `ipt=$(type -p iptables)` or `ipt=$(type -p ip6tables)`. As these commands are deprecated in favor of [Nftables](https://wiki.gentoo.org/wiki/Nftables) and the nft command, by default both are symlinks to xtables-legacy-multi; the symlink target can be specified via eselect iptables.

When the rules are saved, they are usually stored in /var/lib/iptables/rules-save or /var/lib/ip6tables/rules-save. This allows the firewall service to reload the rules at boot time.

Let's begin with a little example:

`root #``"$ipt" -P INPUT DROP`
This will implement a fairly strong firewall: it will drop every packet that will be sent to the host (as this matches the INPUT chain).

The following examples show how firewall rules are further generated.

### Stateless firewall

Traditional firewalls use stateless firewall rules like so:

`root #``"$ipt" -A INPUT --dport 80 -j ACCEPT`
That simply allows the local port 80 to accept traffic (`--dport` configures the destination port), which usually implies HTTP servers as those generally listen on port 80).

### Stateful firewall

In a stateful firewall approach, the previous example would be handled like so:

`root #````
"$ipt" -P INPUT DROP
```
`root #````
"$ipt" -A INPUT -i eth0 -p tcp --dport 80 --syn -m conntrack --ctstate NEW                 -j ACCEPT
```
`root #````
"$ipt" -A INPUT                                 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```
By default, everything will be dropped like a hot potato. However, incoming traffic might be accepted based on the connection state of the packets (starting with NEW and further allowing all established/related traffic). Performance-wise, it would even be better to place the last line before the second to avoid going into complicated filtering chains for already related and established connections.

This is how a stateful firewall operates to avoid opening unneeded holes and accept in/outbound packets based on the state of the packets.

### GeoIP country blocking rules

This approach allows matching packets based on source or destination geographic location. Entire countries can be matched for logging or traffic can be dropped entirely.

Install the Xtables addon for iptables:

`root #``emerge -a net-firewall/xtables-addons`
Download the GeoIP database:

`root #``mkdir /root/geoip``root #``cd /root/geoip``root #``/lib64/xtables-addons/xt_geoip_dl`
Convert the GeoIP csv file to packed format for xt\_geoip:

`root #``mkdir -p /usr/share/xt_geoip``root #``/lib64/xtables-addons/xt_geoip_build -D /usr/share/xt_geoip`
Identify an IP address for testing purposes. One method is to `nslookup example.com` then `whois example.com` to verify the IPv4 address is in the desired country. Ping the address to verify it responds, then add the following rules to verify they are matching the desired country and working as expected:

```
# block INPUT if IPv4 matches
# note that -I will insert the rule at the beginning of the chain, applying it first
iptables -I INPUT -m geoip -i eth0 --src-cc BY,RU -j DROP
iptables -I INPUT -m geoip -i eth0 --dst-cc BY,RU -j DROP
# block FORWARD if IPv4 matches src or dst address
# note that -I will insert the rule at the beginning of the chain, applying it first
iptables -I FORWARD -m geoip -i eth0 --src-cc BY,RU -j DROP
iptables -I FORWARD -m geoip -i eth0 --dst-cc BY,RU -j DROP
```
### Generating firewall rules

#### Generating firewall rules for client

A script as simple as shown below should be sufficient for most client computers. Store it in a safe place such as \~/firewall. It is only needed for first-time initialization of the firewall rules.

An example of a more sophisticated rule set with logging is shown in [this forum discussion](https://forums.gentoo.org/viewtopic-p-7578926.html#7578926).

#### Generating firewall rules for server

This section will try to build up your above script with a set of rules for common external-facing services. Append these to \~/firewall.

I highly recommend adding ssh rules below if you are working on a remote server through ssh.

After saving your desired firewall rules.

`root #````
chmod 744 ~/firewall
```
`root #````
~/firewall
```
This will load your firewall rules into iptables and ip6tables.

`root #````
/etc/init.d/iptables save
```
`root #````
/etc/init.d/ip6tables save
```
Will save your iptables and ip6tables so they are available the next time iptables service is loaded.

`root #````
rc-service iptables start
```
`root #````
rc-service ip6tables start
```
`root #````
rc-update add iptables default
```
`root #````
rc-update add ip6tables default
```
If you need to add a rule. Run it in the command prompt (like individual rules in \~/firewall).

Also add it to \~/firewall if you are sure if you ever reset your firewall, you want those settings back in.

Once satisfied run:

`root #````
/etc/init.d/iptables save
```
`root #````
/etc/init.d/ip6tables save
```
Also. If anything ever goes drastically wrong. You may reset your firewall settings by running \~/firewall, proceeded by the above save.

## Show firewall rules and status

### IPv4

`root #``iptables -L -n`
Print all rules (similar to iptables-save)ː

`root #``iptables -S`
Like every other iptables command, it applies to the specified table (of which `filter` is the default), so NAT rules get listed byː

`root #````
iptables -t nat -L -n
```
`root #````
iptables -t nat -S
```
### IPv6

`root #``ip6tables -L -n`
Print all rules (similar to ip6tables-save)ː

`root #``ip6tables -S`
Like every other ip6tables command, it applies to the specified table (of which `filter` is the default), so NAT rules get listed byː

`root #````
ip6tables -t nat -L -n
```
`root #````
ip6tables -t nat -S
```
## Migration to nftables

nftables is a modern framework for the Netfilter subsytem, offering dual stack configuration (same rules for IPV4 and IPV6) and other features unavailable in iptables.

All tools to export and translate to nftables are part of the iptables package. The migration requires the steps below, in general<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, besides the installation and configuration instructions from [nftables](https://wiki.gentoo.org/wiki/Nftables).

1. emerge iptables with USE flag nftables to add necessary tools
2. export iptables rules to a file
3. translate exported iptables rules to nftables rules
4. replace iptables with nftables

`root #``iptables-save > iptables-rules.txt``root #``iptables-restore-translate -f iptables-rules.txt >nftables-rules.txt``root #``cat nftables-rules.txt``root #````
nft -c -f nftables-rules.txt
```
If you are certain that the machine will either revert to iptables in case of errors or work correctly with the translated nftables rules:

`root #````
/etc/init.d/iptables stop
```
`root #````
nft -f nftables-rules.txt
```
`root #````
/etc/init.d/nftables start
```
Third party tools, e.g. [net-firewall/ufw](https://packages.gentoo.org/packages/net-firewall/ufw), do not support nftables and will call iptables by default. In many such cases, it doesn't seem possible to dispose of iptables, at least at the moment. Otherwise, if you are certain no packages remain that depend on iptables, removing the [iptables](https://packages.gentoo.org/useflags/iptables) [USE flag (default for](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2), which is required by @system) should allow removing the iptables package.

If iptables has been installed with the flag [nftables](https://packages.gentoo.org/useflags/nftables)[, it is possible to use:](https://wiki.gentoo.org/wiki/USE_flag)

`root #``eselect iptables set xtables-nft-multi`
This will forward every call to `iptables` to `iptables-nft`, a translation layer that will operate transparently for the calling application.

[Raspberry Pi](https://wiki.gentoo.org/wiki/Raspberry_Pi) users will probably need to follow this procedure when using kernel 6.18.y with [net-firewall/ufw](https://packages.gentoo.org/packages/net-firewall/ufw).

## See also

- [iptables (Security Handbook)](https://wiki.gentoo.org/wiki/Security_Handbook/Firewalls_and_Network_Security#iptables)
- [nftables](https://wiki.gentoo.org/wiki/Nftables) — a Linux packet filtering framework for the Netfilter subsystem, providing a unified interface for configuring packet filtering, connection tracking, NAT, and related network functionality.

## External resources

- [Linux Firewalls Using iptables](http://borg.uu3.net/iptables/iptables-intro.html)
- [Forums posting with ip6tables -A INPUT -s fe80::/10 -p ipv6-icmp -j ACCEPT](https://forums.gentoo.org/viewtopic-p-7654940.html#7654940)
- [firewall-mv](https://cgit.gentoo.org/user/mv.git/tree/net-firewall/firewall-mv)
- [IPv6](https://en.wikipedia.org/wiki/IPv6)
