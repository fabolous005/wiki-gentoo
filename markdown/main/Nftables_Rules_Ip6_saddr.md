<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Ip6_saddr | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Ip6 saddr -->
---
title: nftables/Rules/Ip6 saddr
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Ip6_saddr
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: "70acb94af03bdbd2"
license: CC BY-SA 4.0
---

# nftables/Rules/Ip6 saddr

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The ip6 saddr expression matches the source IPv6 address in the packet header.

## Evaluation

The ip6 saddr expression compares the packet source address with the specified address, prefix, or set. The expression evaluates to true when the address matches.

The expression does not terminate rule evaluation.

## Placement

The ip6 saddr expression belongs in a rule expression, typically before its statements or verdict.

## Syntax

```
    ip6 saddr <address>
    ip6 saddr <address>/<prefix>
    ip6 saddr { <address>, ... }
    ip6 saddr @<set>
    ip6 saddr != <address>
```
## Examples

Allow traffic from a trusted IPv6 address:

**`/etc/nftables/rules/main.nft`**

```
ip6 saddr 2001:db8::10 accept
```
Allow traffic from an IPv6 subnet:

**`/etc/nftables/rules/main.nft`**

```
ip6 saddr 2001:db8:1::/48 accept
```
Match multiple IPv6 addresses:

**`/etc/nftables/rules/main.nft`**

```
ip6 saddr { 2001:db8::10, 2001:db8::20 } accept
```
Match addresses in a named set:

**`/etc/nftables/rules/main.nft`**

```
set trusted_ipv6 {
        type ipv6_addr
        elements = { 2001:db8::10, 2001:db8:1::/48 }
    }
    ip6 saddr @trusted_ipv6 accept
```
Use the source address as a meter key for per-address rate limiting:

**`/etc/nftables/rules/main.nft`**

```
set ssh_meter6 {
        type ipv6_addr
        flags dynamic
        timeout 1m
    }
    ct state new tcp dport 22 update @ssh_meter6 {
        ip6 saddr limit rate over 3/minute
    } drop
```
## Notes

The ip6 saddr expression matches IPv6 headers. Use **[ip saddr](https://wiki.gentoo.org/wiki/Nftables/Rules/Ip_saddr)** for IPv4 source addresses.

Combine the expression with other expressions to match destination addresses, protocols, or transport-layer ports.

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
- [Nftables/Rules/Ip saddr](https://wiki.gentoo.org/wiki/Nftables/Rules/Ip_saddr) — expression matches the source IPv4 address in the packet header.
- [Nftables/Rules/Meter](https://wiki.gentoo.org/wiki/Nftables/Rules/Meter) — applies stateful expressions separately to each selector value or combination of values.
- [Nftables/Rules/Limit](https://wiki.gentoo.org/wiki/Nftables/Rules/Limit) — matches packets or bytes within a specified rate.
