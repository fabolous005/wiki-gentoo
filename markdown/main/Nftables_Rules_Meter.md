<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Meter | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Meter -->
---
title: Nftables/Rules/Meter
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Meter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: fab6fb99f713e531
license: CC BY-SA 4.0
---

# Nftables/Rules/Meter

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The meter statement applies stateful expressions separately to each selector value or combination of values.

## Evaluation

The meter statement evaluates its expressions for each distinct selector key. Stateful expressions, such as limit and counter, maintain separate state for each key.

## Placement

The meter statement belongs inside a rule in a chain. Packet expressions can restrict which traffic enters the meter.

## Syntax

`meter <name> { <selector> [timeout <value>] <stateful-expression> }`
Selectors identify the packets sharing a meter entry. Multiple selectors can be combined using concatenation.

## Requirements

The meter uses a named dynamic set to maintain per-key state. The set requires a suitable type and the dynamic flag. Add timeout to expire inactive entries when required.

## Examples

Rate-limit new SSH connections per source address:

**`/etc/nftables/rules.d/40-meter-ssh.nft`**

```
set ssh_meter {
        type ipv4_addr
        flags dynamic
        timeout 1m
    }
    ct state new tcp dport 22 meter ssh_meter {
        ip saddr limit rate over 3/minute
    } drop
```
Rate-limit HTTP requests per source address:

**`/etc/nftables/rules.d/40-meter-http.nft`**

```
set http_meter {
        type ipv4_addr
        flags dynamic
        timeout 1m
    }
    tcp dport 80 meter http_meter {
        ip saddr limit rate over 100/second
    } drop
```
The set timeout controls how long an entry remains without being refreshed. Choose a timeout appropriate for the traffic and rate-limit policy.

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
- [Nftables/Rules/Limit](https://wiki.gentoo.org/wiki/Nftables/Rules/Limit) — matches packets or bytes within a specified rate.
- [Nftables/Rules/Log](https://wiki.gentoo.org/wiki/Nftables/Rules/Log) — logs matching packets.
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
