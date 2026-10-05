<!-- source: https://wiki.gentoo.org/wiki/Nftables/Examples | group: Gentoo Wiki (Main) | wiki-title: Nftables/Examples -->
---
title: Nftables/Examples
url: https://wiki.gentoo.org/wiki/Nftables/Examples
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-29"
fingerprint: e1a2990945815871
license: CC BY-SA 4.0
---

# Nftables/Examples

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

On this page several example nftable configurations can be found. The first two examples are skeletons to illustrate how nftables works. The third and fourth exmaple show how, using nftables, rules can be simplified by combining IPv4 and IPv6 in the generic IP table 'inet'. The fifth example shows how nftables can be combined with bash scripting.

## Basic routing firewall

The following is an example of nftables rules for a basic IPv4 firewall that:

1. Only allows packets from LAN to the firewall machine
2. Only allows packets
  1. From LAN to WAN
  2. From WAN to LAN for connections established by LAN.

For forwarding between WAN and LAN to work, it needs to be enabled with:

`root #``sysctl -w net.ipv4.ip_forward = 1`
**`/etc/nftables/nftables_firewall`**

## Basic NAT

The following is an example of nftables rules for setting up basic Network Address Translation (NAT) using masquerade. If we have a static IP, it would be slightly faster to use source nat (SNAT) instead of masquerade. This way the router would replace the source with a predefined IP, instead of looking up the outgoing IP for every packet.

**`/etc/nftables/nftables_nat`**

## Typical workstation (separate IPv4 and IPv6)

This is an example of a simple rule set that may be used by a typical workstation or other end user device. It defaults to dropping packets that do not match any of the rules, uses connection tracking to accept packets established or related to traffic initiated by the host, and accepts all ICMP (see note). Further, it assumes that we want to be able to connect to the machine via SSH.

While counter is used in this example, it isn't required if we're not interested in packet counts. Just omit counter from any rule.

**`/etc/nftables/nftables.rules`**

## Typical workstation (combined IPv4 and IPv6)

As for the previous example, but uses the inet family to apply rules to both IPv4 and IPv6 packets. So, only one table needs to be maintained.

**`/etc/nftables/nftables.rules`**

## Stateful router example

The following is an example of nftables configuration script for a stateful router.

**`/home/rt/scripts/nft.sh`**

```
#!/bin/bash
nft="/sbin/nft";
# ruleset, masquerade and full reject support are available starting with Linux Kernel 3.18
${nft} flush ruleset;
replaced [[]] with [[:|]]
export LAN_IN=enp3s6
export LAN_ML=enp2s0
export WAN=ppp0
LAN_INLOCALNET=192.168.1.0/24
LAN_MLNET=10.52.0.0/14
MLIP=10.54.1.101
TORRENT_PORT_WAN=55414
TRACKER_TORRENT_PORT_WAN=4949
TORRENT_PORT_LAN=55413
MAC[2]=00:23:45:67:89:ab
...
MAC[20]=00:fe:dc:ba:98:76
${nft} -f /etc/nftables/ipv4-filter;
${nft} -f /etc/nftables/ipv4-nat;
# BANNED
${nft} add rule filter input meta iifname ${WAN} ip saddr 121.12.242.43 drop;
# Drop locals from internet
${nft} add rule filter input meta iifname ${WAN} ip saddr \
        { 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12 } drop;
# Drop invalid
${nft} add rule filter input ct state invalid drop;
${nft} add rule filter input meta iif lo ct state new accept;
${nft} add rule filter input meta iif ${LAN_ML} ip saddr ${LAN_MLNET} ct state new accept;
${nft} add rule filter input meta iif ${LAN_IN} ip saddr ${LAN_INLOCALNET} ct state new accept;
${nft} add rule filter input ip protocol tcp tcp dport \
        { ${TORRENT_PORT_LAN}, ${TORRENT_PORT_WAN}, \
                ${TRACKER_TORRENT_PORT_WAN} } ct state new accept;
${nft} add rule filter input ip protocol udp udp dport \
        { ${TORRENT_PORT_LAN}, ${TORRENT_PORT_WAN}, \
                ${TRACKER_TORRENT_PORT_WAN} } ct state new accept;
${nft} add rule filter input meta iifname ${WAN} ip protocol tcp ct state new tcp dport 80 accept;
# torrent port forwarding example
${nft} add rule nat prerouting meta iifname ${WAN} tcp dport ${TORRENT_PORT_LAN} \
        dnat 192.168.1.10:${TORRENT_PORT_LAN}
${nft} add rule filter forward meta iifname ${WAN} meta oif ${LAN_IN} ip daddr 192.168.1.10 \
        tcp dport ${TORRENT_PORT_LAN} ct state new accept;
${nft} add rule filter input ip saddr != ${LAN_INLOCALNET} ct state new drop;
${nft} add rule filter forward meta iif ${LAN_ML} ct state new drop;
${nft} add rule filter forward meta iifname ${WAN} ct state new drop;
${nft} add rule filter input ct state established,related accept;
${nft} add rule nat postrouting oif ${LAN_ML} ip saddr ${LAN_INLOCALNET} snat ${MLIP};
${nft} add rule nat postrouting oifname ${WAN} ip saddr ${LAN_INLOCALNET} masquerade;
${nft} add rule filter forward ct state established,related accept;
# Give internet access to internal LAN addresses
for i in {2..20}
do
if grep 1 /var/www/myhost/htdocs/payment/192.168.1.$i > /dev/null;
then
        ${nft} add rule filter forward ether saddr ${MAC[$i]} ip saddr 192.168.1.$i \
                ct state new accept;
        echo ACCEPT 192.168.1.$i ALL;
else
        ${nft} add rule filter forward ether saddr ${MAC[$i]} ip saddr 192.168.1.$i \
                meta oif ${LAN_ML} ct state new accept;
        echo ACCEPT 192.168.1.$i ${LAN_ML};
fi
done
# Policies
${nft} add rule filter input drop;
${nft} add rule filter forward drop;
${nft} add rule filter output accept;
/etc/init.d/nftables save;
```
## Panic Stop

The following example is a panic stop for an OpenRC init subsystem.

May be used if under distributed denial of service (DDoS) or an intrusion.

`root #``/etc/init.d/nftables panic`
or using a custom panic stop by running this nftables commands:

**`/etc/nftables/rules/panic-stop.nft`**

**Custom Panic Stop nftables command file**

`root #``nft -f /etc/nftables/rules/panic-stop.nft`
## Passing shell variables to a nft command file

The following example is the passing of variable values to a a nft command file.

Useful for shell logic in selecting interface or dynamic port.

**`/etc/nftables/rules/variable-settings.nft`**

**Variable-passing nft command file**

Then execute:

`root #``export MY_PORT=22``root #``nft -D $MY_PORT -f /etc/nftables/rules/variable-settings.nft`

Now all incoming SSH/TCP connections are blocked.

Forgetting that -D $MY\_PORT will result in an error:

`root #``nft -f /etc/nftables/rules/variable-passing.nft````
/etc/nftables/rules/variable-passing.nft:5:28-34: Error: unknown identifier 'MY_PORT'
                tcp dport $MY_PORT drop
                           ^^^^^^^
```
Alternatively can do inline assignment directly as:

`root #``nft -D MY_PORT=22 -f /etc/nftables/rules/variable-settings.nft`

And supports multiple variables:

`root #``nft -D MY_PORT=22 -D $WAN_INTF -f /etc/nftables/rules/variable-settings.nft`


## Restrict packets to a process

Example shows how to restrict packets to specific processes using [SELinux](https://wiki.gentoo.org/wiki/SELinux):

**`/etc/nftables/rules/selinux.nft`**

**Restricting packets to a process**

## See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules)
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples]
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
- [Security Handbook](https://wiki.gentoo.org/wiki/Security_Handbook) — valuable guidance on Gentoo Linux security and cybersecurity in general.
- [Iptables](https://wiki.gentoo.org/wiki/Iptables) — a program used to configure and manage the kernel's netfilter modules.
