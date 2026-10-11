<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Ip_saddr | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Ip saddr -->
---
title: Nftables/Rules/Ip saddr
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Ip_saddr
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: "70acb94af03bdbd0"
license: CC BY-SA 4.0
---

# Nftables/Rules/Ip saddr

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The ip saddr expression matches the source IPv4 address in the packet header.

## Evaluation

The ip saddr expression compares the packet source address with the specified address, prefix, or set. The expression evaluates to true when the address matches.

The expression does not terminate rule evaluation.

## Placement

The ip saddr expression belongs in a rule expression, typically before its statements or verdict.

Also may be used as expression for a nested IP headers as well as a payload statement.

## Syntax

```
    ip saddr <address>
    ip saddr <address>/<prefix>
    ip saddr { <address>, ... }
    ip saddr @<set>
    ip saddr != <address>
```
## Examples

Allow traffic from a trusted IPv4 address:

**`/etc/nftables/rules/main.nft`**

```
ip saddr 192.0.2.10 accept
```
Allow traffic from a subnet:

**`/etc/nftables/rules/main.nft`**

```
ip saddr 192.0.2.0/24 accept
```
Match multiple addresses:

**`/etc/nftables/rules/main.nft`**

```
ip saddr { 192.0.2.10, 192.0.2.20 } accept
```
Match addresses in a named set:

**`/etc/nftables/rules/main.nft`**

```
set trusted_ipv4 {
        type ipv4_addr
        elements = { 192.0.2.10, 192.0.2.0/24 }
    }
    ip saddr @trusted_ipv4 accept
```
Use the source address as a meter key for per-address rate limiting:

**`/etc/nftables/rules/main.nft`**

```
set ssh_meter {
        type ipv4_addr
        flags dynamic
        timeout 1m
    }
    ct state new tcp dport 22 update @ssh_meter {
        ip saddr limit rate over 3/minute
    } drop
```
Combine the expression with other expressions to match destination addresses, protocols, or transport-layer ports.

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
- [Nftables/Rules/Ip6 saddr](https://wiki.gentoo.org/wiki/Nftables/Rules/Ip6_saddr) — expression matches the source IPv6 address in the packet header.
- [Nftables/Rules/Meter](https://wiki.gentoo.org/wiki/Nftables/Rules/Meter) — applies stateful expressions separately to each selector value or combination of values.
- [Nftables/Rules/Limit](https://wiki.gentoo.org/wiki/Nftables/Rules/Limit) — matches packets or bytes within a specified rate.
