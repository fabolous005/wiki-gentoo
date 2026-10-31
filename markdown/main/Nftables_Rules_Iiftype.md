<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Iiftype | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Iiftype -->
---
title: Nftables/Rules/Iiftype
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Iiftype
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-30"
fingerprint: "32b8cc5cbb2c4bc0"
license: CC BY-SA 4.0
---

# Nftables/Rules/Iiftype

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The iiftype and oiftype expressions match packets by input and output network interface type, respectively.

## Syntax

```
[meta] iiftype <type>
[meta] iiftype { <type> [, <type> ...] }
[meta] iiftype != <type>
[meta] iiftype != { <type> [, <type> ...] }
[meta] oiftype <type>
[meta] oiftype { <type> [, <type> ...] }
[meta] oiftype != <type>
[meta] oiftype != { <type> [, <type> ...] }
```
The meta keyword is optional for both expressions. It explicitly qualifies the metadata expression without changing its evaluation.

## Evaluation

The iiftype expression matches the input interface type; oiftype matches the output interface type.

Interface types are 16-bit values corresponding to Linux ARPHRD\_\* definitions. nftables provides symbolic names for commonly used types:

| Type | Value | Description | 
|---|---|---|
| ether | 0x0001 | Ethernet (ARPHRD\_ETHER) | 
| ppp | 0x0200 | Point-to-Point Protocol (ARPHRD\_PPP) | 
| ipip | 0x0300 | IPv4-in-IPv4 tunnel (ARPHRD\_TUNNEL) | 
| ipip6 | 0x0301 | IPv6-in-IPv4 tunnel (ARPHRD\_TUNNEL6) | 
| loopback | 0x0304 | Loopback interface (ARPHRD\_LOOPBACK) | 
| sit | 0x0308 | Simple Internet Transition (SIT) tunnel (ARPHRD\_SIT) | 
| ipgre | 0x030a | GRE tunnel (ARPHRD\_IPGRE) | 

The expressions accept a single type or a set block of comma-separated types. The != operator negates the match.

Additional interface types can be specified numerically. Use nft describe iiftype to display the available symbolic constants and their values:

`root #``nft describe iiftype````
meta expression, datatype iface_type (network interface)
pre-defined symbolic constants (in hexadecimal):
        ether                           0x0001
        ppp                             0x0200
        ipip                            0x0300
        ipip6                           0x0301
        loopback                        0x0304
        sit                             0x0308
        ipgre                           0x030a
```
For a list of interface type ID, execute:

## Usage

The iiftype and oiftype expressions belong inside a rule in a chain. Combine them with other expressions to restrict matching by interface type.

Accept packets arriving through Ethernet interfaces:

**`/etc/nftables/rules.d/10-ethernet-input.nft`**

```
iiftype ether accept
```
Drop packets arriving through PPP interfaces:

**`/etc/nftables/rules.d/20-ppp-input.nft`**

```
iiftype ppp drop
```
Match packets arriving through Ethernet or loopback interfaces:

**`/etc/nftables/rules.d/30-interface-types.nft`**

```
iiftype { ether, loopback } accept
```
Match packets leaving through Ethernet interfaces:

**`/etc/nftables/rules.d/40-ethernet-output.nft`**

```
oiftype ether accept
```
Match the same input interface type using either syntax form:

**`/etc/nftables/rules.d/50-equivalent-iftype.nft`**

```
iiftype ether accept
meta iiftype ether accept
```
Both forms match the same interface type. The meta keyword does not change the match.

## Related expressions

- [iif](https://wiki.gentoo.org/wiki/Nftables/Rules/Iif) — matches packets by input network interface index.
- [iifname](https://wiki.gentoo.org/wiki/Nftables/Rules/Iifname) — matches packets by input network interface name.
- [iiftype] — match packets by input and output network interface type, respectively.
- iifkind — matches the input interface kind
- iifgroup — matches the input interface group
- oif — matches the output interface index
- [oiftype](https://wiki.gentoo.org/wiki/Nftables/Rules/Oiftype) — match packets by input and output network interface type, respectively.
- oifname — matches the output interface name
- oifkind — matches the output interface kind
- oifgroup — matches the output interface group

## Kernel

The relevant Linux Kconfig options are:

- CONFIG\_NF\_TABLES — nftables framework
- CONFIG\_NFT\_META — nftables packet metadata expression support; builds the nft\_meta module when configured as a module

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — the Linux packet-handling framework
