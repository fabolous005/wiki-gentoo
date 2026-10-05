<!-- source: https://wiki.gentoo.org/wiki/Nftables/Ruleset | group: Gentoo Wiki (Main) | wiki-title: Nftables/Ruleset -->
---
title: Nftables/Ruleset
url: https://wiki.gentoo.org/wiki/Nftables/Ruleset
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: a5bd98fe670f210d
license: CC BY-SA 4.0
---

# Nftables/Ruleset

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Introduction

## Ruleset

nft command files may include other command files with include.

The include command allows a ruleset to be split into an administrator-defined directory hierarchy. nft does not impose a particular directory layout.

For example, a large homelab or enterprise ruleset might be organized as::

`root #``ls -R /etc/nftables/rules`
/etc/nftables/rules/main.nft
/etc/nftables/rules/filter/filter.nft
/etc/nftables/rules/filter/filter-bad-packets.nft
/etc/nftables/rules/filter/pre-routing/filter-prerouting.nft
/etc/nftables/rules/filter/postrouting/filter-postrouting.nft
/etc/nftables/rules/filter/ingress/filter-ingress.nft
/etc/nftables/rules/filter/output/filter-output.nft
/etc/nftables/rules/filter/output/lo/filter-output.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan-icmp.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan-tcp.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan-udp.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan-icmp.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan-tcp.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan-udp.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz-icmp.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz-tcp.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz-udp.nft
/etc/nftables/rules/filter/input/filter-input.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan-icmp.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan-tcp.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan-udp.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan-icmp.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan-tcp.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan-udp.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz-icmp.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz-tcp.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz-udp.nft
/etc/nftables/rules/filter/input/lo/filter-input.nft
/etc/nftables/rules/filter/forward/forward-input.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan-icmp.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan-tcp.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan-udp.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan-icmp.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan-tcp.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan-udp.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz-icmp.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz-tcp.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz-udp.nft
/etc/nftables/rules/filter/forward/lo/forward-input.nft
/etc/nftables/rules/filter/forward/forward-output.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan-icmp.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan-tcp.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan-udp.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan-icmp.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan-tcp.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan-udp.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz-icmp.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz-tcp.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz-udp.nft
/etc/nftables/rules/filter/forward/lo/forward-output.nft

## Tables

Tables provide the top-level namespace for nftables objects.

A table belongs to a family such as ip, ip6, inet, bridge, netdev, and arp.

Tables contain:

- chains;
- sets;
- maps; and
- flowtables.


Rules belong to chains, while sets, maps, and flowtables can be referenced by rules.

Sample table syntax

### Address families

### Table declaration

### Table management

## Rules

A rule is an ordered element of a chain.

Rules contain expressions.

Expressions determine whether a packet or other networking state matches.

Expressions are followed by statements and verdicts that determine what processing occurs.

For rule syntax and the components of a rule, see [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules).

#### Adding rules

The following command adds a **rule** to the **chain** called *input\_filter*, on the *base\_table* **table**, dropping all incoming traffic to port 80:

`root #``nft add rule inet base_table input_filter tcp dport 80 drop`
See [Nftables/Ruleset/Rules](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Rules) for details.

## Chains

Chains belong to a table and may be either base chains or regular chains.

For chain types, declaration, properties, lifecycle, relationships, and processing semantics, see [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain).

## Sets

Sets contain collections of values that can be matched by rules.

Sets are useful for:

- large numbers of IP addresses;
- groups of ports treated as a single set; and
- interfaces sharing a conceptual network boundary, such as a LAN, DMZ, or failover WAN.


They can reduce rule duplication when the same match applies to multiple addresses, ports, interfaces, or other values.

### Named sets

### Anonymous sets

### Set elements

### Intervals

## Maps

Maps associate keys with values and allow rules to use a matched value to select another value or action.

### Named maps

### Anonymous maps

### Map elements

## Counters

## Quotas

## Flowtables

Flowtables provide:

- flow-based forwarding for established network flows;
- bypassing portions of the conventional packet-processing path; and
- hardware offload where supported.

## Objects

## NAT

## Commands

### add

### create

### delete

### list

### flush

### rename

### replace

### Examples

### See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset]
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules)
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
