<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/masquerade -->
---
title: nftables/rules/masquerade
url: https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-20"
fingerprint: "8ea0b93af28b7579"
license: CC BY-SA 4.0
---

# nftables/rules/masquerade

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The `masquerade` statement translates packet source addresses to the outgoing interface address. It belongs to the **statement** syntax group.

## Kernel

For `masquerade` configuration, the relevant additional Linux Kconfig options include:

- `CONFIG_NF_TABLES` - nf\_tables framework
- `CONFIG_NF_NAT` - Netfilter NAT support
- `CONFIG_NFT_NAT` - nftables NAT statements
- `CONFIG_NFT_REDIR` - nftables redirect statement support (the corresponding Kconfig symbol may vary by kernel version)

## Placement

Place `masquerade` in a **nat** base chain having the **postrouting** hook, or in a regular chain called from such a base chain.

## Evaluation

The `masquerade` statement is terminal because they implicitly issue an `accept` verdict.

The `masquerade` statement establishes a source NAT mapping for the connection's first packet. Subsequent packets use the established mapping.

## Syntax

masquerade \[to :PORT\_SPEC\] \[FLAGS\]
PORT\_SPEC := port | port - port
FLAGS := FLAG \[, FLAGS\]
FLAG := persistent | random | fully-random

- `PORT_SPEC` - Source port or inclusive port range
- `persistent` - Prefer consistent address selection for each client
- `random` - Randomize source port mapping
- `fully-random` - Use fully randomized source port mapping



**`/etc/nftables/rules/main.nft`**

**Masquerade statement**

```
masquerade
```
The optional `to` parameter specifies a source port or port range. Optional flags control port mapping.

## Examples

Masquerade packets leaving through a dynamically addressed interface:

**`/etc/nftables/rules/main.nft`**

**Masquerade outgoing traffic**

```
table ip nat {
    chain outbound {
        type nat hook postrouting priority srcnat;
        policy accept;
        oifname "eth0" masquerade
    }
}
```
Masquerade traffic from a specific network with a source port range and persistent mapping:

**`/etc/nftables/rules/main.nft`**

**Masquerade with port range and flags**

```
table ip nat {
    chain outbound {
        type nat hook postrouting priority srcnat;
        policy accept;
        ip saddr 192.168.1.0/24 oifname "eth0" masquerade to :1024-65535 persistent
    }
}
```
## Tips

- Use [`dnat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat) to translate destinations to a specified address.
- Use [`snat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat) for known source addresses or defined address ranges.
- Use [`redirect`](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect) to redirect traffic to a local service

## See also

- [Nftables/Rules/Snat](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat)
- [Nftables/Rules/Dnat](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
