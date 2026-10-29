<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Iif | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Iif -->
---
title: Nftables/Rules/Iif
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Iif
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-28"
fingerprint: "3aadcd4cd809e9c0"
license: CC BY-SA 4.0
---

# Nftables/Rules/Iif

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The iif expression matches packets by input network interface index.

## Syntax

```
[meta] iif <interface>
[meta] iif { <interface> [, <interface> ...] }
[meta] iif != <interface>
[meta] iif != { <interface> [, <interface> ...] }
```
The meta keyword is optional for iif; both forms are functionally equivalent. It explicitly qualifies the metadata expression without changing its evaluation.

## Evaluation

The iif expression compares the packet's input interface index against the specified interface index. Interface names resolve to their indices when the rule is installed; consequently, the named interface must exist at that time.

Interface-index matching persists across interface renames. Deleting and recreating an interface can assign a different index, however, causing the rule to match differently.

The iif expression can be a single device index or a set block of comma-separated device indexes.

List the available devices using:

`root #``ip -o link show | awk -F': ' '{print $1, $2}'`
1 lo
2 enp3s0
3 wlp2s0

These indices are assigned by the kernel and may change when interfaces are added or removed.

## Usage

The iif expression belongs inside a rule in a chain. Combine it with other expressions to restrict matching to packets arriving on a particular interface.

Accept packets arriving on the loopback interface:

**`/etc/nftables/rules.d/10-loopback.nft`**

```
iif lo accept
```
Accept packets arriving on either of two interfaces:

**`/etc/nftables/rules.d/20-trusted-interfaces.nft`**

```
iif { eth0, eth1 } accept
```
Drop packets arriving on any interface other than eth0:

**`/etc/nftables/rules.d/30-restrict-input.nft`**

```
iif != eth0 drop
```
Match the same interface using either syntax form:

**`/etc/nftables/rules.d/40-equivalent-iif.nft`**

```
iif eth0 accept
meta iif eth0 accept
```
Both forms match the same interface index. The meta keyword does not change the match.

## Related expressions

- [iifname](https://wiki.gentoo.org/index.php?title=Nftables/Rules/iifname&action=edit&redlink=1) — matches the input interface name
- iiftype — matches the input interface hardware type
- iifkind — matches the input interface kind
- iifgroup — matches the input interface group
- oif — matches the output interface index
- oifname — matches the output interface name

## Kernel

The relevant Linux Kconfig options are:

- `CONFIG_NF_TABLES` - nftables framework
- `CONFIG_NFT_META` - nftables packet metadata expression support; builds the nft\_meta module when configured as a module

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — the Linux packet-handling framework
