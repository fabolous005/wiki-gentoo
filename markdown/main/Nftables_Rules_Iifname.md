<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Iifname | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Iifname -->
---
title: nftables/Rules/Iifname
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Iifname
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-28"
fingerprint: "3aadc15cf92929c1"
license: CC BY-SA 4.0
---

# nftables/Rules/Iifname

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The iifname expression matches packets by input network interface name.

## Syntax

```
[meta] iifname <interface>
[meta] iifname { <interface> [, <interface> ...] }
[meta] iifname != <interface>
[meta] iifname != { <interface> [, <interface> ...] }
```
The meta keyword is optional for iifname; both forms are functionally equivalent. It explicitly qualifies the metadata expression without changing its evaluation.

## Evaluation

The iifname expression matches packets by input interface name. Unlike iif, the interface need not exist when the rule is installed.

Interface renaming changes the match. Deleting and recreating an interface with the same name preserves the name-based match.

The expression accepts a single interface name or a set of comma-separated names.

The != operator negates the match.

A trailing \* wildcard matches an interface-name prefix. For example, iifname "enp\*" matches names beginning with enp.

List the available devices using:

`root #``ip -o link show | awk -F': ' '{print $1, $2}'`
1 lo
2 enp3s0
3 wlp2s0

These indices are assigned by the kernel and may change when interfaces are added or removed.

## Usage

The iifname expression belongs inside a rule in a chain. Combine it with other expressions to restrict matching to packets arriving on a particular interface.

Accept packets arriving on the loopback interface:

**`/etc/nftables/rules/10-loopback.nft`**

```
iifname lo accept
```
Accept packets arriving on either of two interfaces:

**`/etc/nftables/rules/20-trusted-interfaces.nft`**

```
iifname { "eth0", "eth1" } accept
```
Drop packets arriving on any interface other than eth0:

**`/etc/nftables/rules/30-restrict-input.nft`**

```
iifname != "eth0" drop
```
Match interfaces whose names begin with enp:

**`/etc/nftables/rules/40-ethernet-interfaces.nft`**

```
iifname "enp*" accept
```
Match the same interface using either syntax form:

**`/etc/nftables/rules/50-equivalent-iifname.nft`**

```
iifname eth0 accept
meta iifname eth0 accept
```
Both forms match the same interface name. The meta keyword does not change the match.

## Related expressions

- [iif](https://wiki.gentoo.org/wiki/Nftables/Rules/Iif) — matches the input interface index
- iiftype — matches the input interface hardware type
- iifkind — matches the input interface kind
- iifgroup — matches the input interface group
- oif — matches the output interface index
- oifname — matches the output interface name

## Kernel

The relevant Linux Kconfig options are:

- CONFIG\_NF\_TABLES — nftables framework
- CONFIG\_NFT\_META — nftables packet metadata expression support; builds the nft\_meta module when configured as a module

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — the Linux packet-handling framework
