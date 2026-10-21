<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Dnat -->
---
title: nftables/rules/dnat
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-20"
fingerprint: c0a0b95af227bd7d
license: CC BY-SA 4.0
---

# nftables/rules/dnat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The `dnat` statement translates packet destination addresses. It belongs to the **statement** syntax group.

## Kernel

For `dnat` configuration, the relevant additional Linux Kconfig options include:

- `CONFIG_NF_NAT` - Netfilter NAT support
- `CONFIG_NF_TABLES` - nf\_tables framework
- `CONFIG_NFT_NAT` - nftables NAT statements
- `CONFIG_NF_CONNTRACK` - connection tracking

## Placement

Place `dnat` in a **nat** base chain having the **prerouting** or **output** hook, or in a regular chain called from such a base chain.

## Evaluation

The `dnat` statement is terminal because they implicitly issue an `accept` verdict.

The `dnat` statement establishes a destination NAT mapping for the connection's first packet. Subsequent packets use the established mapping.

## Syntax

dnat \[\[\[ip | ip6\] \[prefix\]\] to\] ADDR\_SPEC \[:PORT\_SPEC\] \[FLAGS\]
ADDR\_SPEC := address | address - address
PORT\_SPEC := port | port - port
FLAGS := FLAG \[, FLAGS\]
FLAG := persistent | random | fully-random

- `ip`, `ip6` - Address family
- `prefix` - Enable prefix-based address mapping
- `ADDR_SPEC` - Single address or inclusive address range
- `PORT_SPEC` - Single port or inclusive port range
- `persistent` - Prefer consistent address selection for each client
- `random` - Randomize port mapping
- `fully-random` - Use fully randomized port mapping



**`/etc/nftables/rules/main.nft`**

**DNAT statement**

```
dnat to 192.168.1.10
```
The `to` parameter accepts an address, address range, port, or port range. Optional flags include `persistent`, `random`, and `fully-random`.

## Examples

Translate destination addresses to a fixed address:

**`/etc/nftables/rules/main.nft`**

**Fixed destination address**

```
table ip nat {
    chain prerouting {
        type nat hook prerouting priority dstnat;
        policy accept;
        dnat to 192.168.1.10
    }
}
```
Translate destination addresses to an address range:

**`/etc/nftables/rules/main.nft`**

**Destination address range**

```
table ip nat {
    chain prerouting {
        type nat hook prerouting priority dstnat;
        policy accept;
        dnat to 192.168.1.10-192.168.1.20
    }
}
```
DNAT with a destination port range and persistent flag:

**`/etc/nftables/rules/main.nft`**

**DNAT with port range and persistent flag**

```
table ip nat {
    chain prerouting {
        type nat hook prerouting priority dstnat;
        policy accept;
        tcp dport 80 dnat to 192.168.1.10:8080-8090 persistent
    }
}
```
## Tips

- Use [`masquerade`](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade) for dynamically assigned outgoing addresses
- Use [`snat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat) for known source addresses or defined address ranges.
- Use [`redirect`](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect) to redirect traffic to a local service
- In an **inet** family chain, specify `ip` or `ip6` when providing an address.

## See also

- [Nftables/Rules/Snat](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat)
- [Nftables/Rules/masquerade](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade)
- [Nftables/Rules/Redirect](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
