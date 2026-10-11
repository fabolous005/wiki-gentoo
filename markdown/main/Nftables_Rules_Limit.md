<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Limit | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Limit -->
---
title: Nftables/Rules/Limit
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Limit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: "10927b3ce73a6d78"
license: CC BY-SA 4.0
---

# Nftables/Rules/Limit

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **limit** statement matches packets or bytes within a specified rate.

## Evaluation

The limit statement is non-terminal. Rule evaluation continues when the rate condition matches.

The over keyword inverts the rate condition, matching traffic above the specified rate.

## Placement

The limit statement belongs inside a rule in a chain. Packet expressions can restrict which traffic is rate-limited.

## Syntax

limit rate \[over\] \<value>/\<time-unit> \[burst \<value> packets\]
limit rate \[over\] \<value> \<byte-unit>/\<time-unit> \[burst \<value> \<byte-unit>\]

Valid time units are second, minute, hour, and day.

Valid byte units are bytes, kbytes, and mbytes.

## Options

| Option | Description | 
|---|---|
| rate | Sets the matching rate. | 
| over | Matches rates above the specified rate. | 
| burst | Sets the token bucket burst allowance. | 

The default packet burst is 5 packets. The default byte burst is 0 bytes.

## Examples

Accept up to 10 packets per second:

**`/etc/nftables/rules.d/40-limit-packets.nft`**

```
limit rate 10/second accept
```
Drop packets exceeding 10 per second:

**`/etc/nftables/rules.d/40-limit-excess-packets.nft`**

```
limit rate over 10/second drop
```
Log and drop excess packets:

**`/etc/nftables/rules.d/66-log-dropped.nft`**

```
limit rate over 5/second log prefix "Dropped: " drop
```
Limit traffic to 1 MiB per second:

**`/etc/nftables/rules.d/40-limit-bandwidth.nft`**

```
limit rate 1 mbytes/second accept
```
Allow a burst of 20 packets:

**`/etc/nftables/rules.d/40-limit-burst.nft`**

```
limit rate 10/second burst 20 packets accept
```
## Notes

The limit statement uses a token bucket. The burst allowance controls how much traffic can exceed the average rate temporarily.

The limit statement maintains its own rate-limit state. Use a **[meter](https://wiki.gentoo.org/wiki/Nftables/Rules/Meter)** when separate rate limits are required for different packet keys, such as source addresses.

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
- [Nftables/Rules/Log](https://wiki.gentoo.org/wiki/Nftables/Rules/Log) — logs matching packets.
- [Nftables/Rules/Meter](https://wiki.gentoo.org/wiki/Nftables/Rules/Meter) — applies stateful expressions separately to each selector value or combination of values.
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [Nftables/Examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
