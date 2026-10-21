<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Snat | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Snat -->
---
title: nftables/rules/snat
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Snat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-20"
fingerprint: "82a0b95af137fd6d"
license: CC BY-SA 4.0
---

# nftables/rules/snat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The `snat` statement translates packet source addresses. It belongs to the **statement** syntax group.

## Kernel

For `snat` configuration, the relevant additional Linux Kconfig options include:

- `CONFIG_NF_NAT` - Netfilter NAT support
- `CONFIG_NF_TABLES` - nf\_tables framework
- `CONFIG_NFT_NAT` - nftables NAT statements
- `CONFIG_NF_CONNTRACK` - connection tracking

## Placement

Place `snat` in a **nat** base chain having the **postrouting** or **input** hook, or in a regular chain called from such base chain.

## Evaluation

The `snat` statement is terminal because they implicitly issue an `accept` verdict.

The `snat` statement establishes a source NAT mapping for the connection's first packet. Subsequent packets use the established mapping.

## Syntax

snat \[\[\[ip | ip6\] \[prefix\]\] to\] ADDR\_SPEC \[:PORT\_SPEC\] \[FLAGS\]
ADDR\_SPEC := address | address - address
PORT\_SPEC := port | port - port
FLAGS := FLAG \[, FLAGS\]
FLAG := persistent | random | fully-random

- `ip`, `ip6` - Address family
- `prefix` - Enable prefix-based address mapping
- `ADDR_SPEC` - Single address or inclusive address range
- `PORT_SPEC` - Single port or inclusive port range
- `persistent` - Prefer consistent address selection for each client
- `random` - Randomize source port mapping
- `fully-random` - Use fully randomized source port mapping



**`/etc/nftables/rules/main.nft`**

**SNAT statement**

```
snat to 192.0.2.1
```
The `to` parameter accepts an address, address range, or address and port range. Optional flags include `persistent`, `random`, and `fully-random`.

## Examples

Translate source addresses to a fixed address:

**`/etc/nftables/rules/main.nft`**

**Fixed source address**

```
table ip nice {
    chain spreadout {
        type nat hook postrouting priority filter;
        policy accept;
        ip saddr 192.168.1.0/24 snat to 192.0.2.1
    }
}
```
Translate source addresses to an address range:

**`/etc/nftables/rules/main.nft`**

**Source address range**

```
table ip nice {
    chain spreadout {
        type nat hook postrouting priority filter;
        policy accept;
        snat to 192.0.2.1-192.0.2.10
    }
}
```
SNAT with prefix, port range, and persistent flag:

**`/etc/nftables/rules/main.nft`**

**SNAT with prefix, port range, and persistent flag**

```
table ip nice {
    chain spreadout {
        type nat hook input priority filter;
        policy accept;
        snat ip prefix to 192.168.1.0/24:1024-65535 persistent
    }
}
```
## Tips

- Use [`masquerade`](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade) for dynamically assigned outgoing addresses
- Use [`dnat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat) to translate destinations to a specified address.
- Use [`redirect`](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect) to redirect traffic to a local service

## See also

- [Nftables/Rules/masquerade](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade)
- [Nftables/Rules/Dnat](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat)
- [Nftables/Rules/Redirect](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
