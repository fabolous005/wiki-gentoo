<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules -->
---
title: Nftables/Rules
url: https://wiki.gentoo.org/wiki/Nftables/Rules
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: a1be9b1af70bdd0f
license: CC BY-SA 4.0
---

# Nftables/Rules

[Nftables](https://wiki.gentoo.org/wiki/Nftables)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Rules** match packet properties and perform actions. Chains contain ordered rules; tables contain chains.

## Rule syntax

A rule combines expressions and statements.

nft add rule \<family> \<table> \<chain> \<expressions> \<statements>

For example:

**`/etc/nftables/rules.d/ssh-server.nft`**

```
tcp dport 22 accept
```
## Expressions

Expressions support comparisons, ranges, sets, and combinations of packet properties.

Expressions match packet properties, metadata, connection tracking state, or other values.

Common expressions include:

- ip saddr — source IPv4 address.
- ip daddr — destination IPv4 address.
- ip6 saddr — source IPv6 address.
- tcp dport — TCP destination port.
- udp dport — UDP destination port.
- meta iifname — incoming interface.
- ct state — connection tracking state.

**`/etc/nftables/rules.d/basic-expression.nft`**

**Sample Nftables expression**

```
ip saddr 192.0.2.0/24 drop
tcp dport { 80, 443 } accept
ct state established,related accept
meta iifname == "eth0"
meta iif not in { 1, 2 }  # ifIndex
meta iifname ~ "^eth.*";  # regex
meta hour in { 0,2,4,6,8,12 }
```
## Statements

Statements act on matching packets. Common statements include counter, log, limit, packet modification, and verdicts.

Non-terminal statements can precede a verdict.

### log

The log statement logs matching packets without terminating rule evaluation.

**`/etc/nftables/rules.d/drop-logged.nft`**

```
log prefix "dropped: " drop
```
## Verdicts

The decision to take its packet to another chain are called a verdict.

| Verdict Name | Verdict Description | 
|---|---|
| accept | verdict accepts a packet at the current hook. A later base chain can still drop it. | 
| drop | discards a packet immediately. | 
| jump | calls a regular chain and saves the return position. | 
| goto | transfers control to a regular chain without saving the current return position. | 
| reject | discards a packet and, when possible, sends an error response. | 
| return | ends the current chain. Regular chains return to the caller; base chains apply their policy. | 

## Hub page

This is a hub page for Nftables rules.

The following pages provide keyword guides for configuring [Nftables](https://wiki.gentoo.org/wiki/Nftables) command files for [Netfilter](https://wiki.gentoo.org/wiki/Netfilter).

## See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
