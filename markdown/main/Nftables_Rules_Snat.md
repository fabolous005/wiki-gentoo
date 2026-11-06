<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Snat | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Snat -->
---
title: nftables/rules/snat
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Snat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-05"
fingerprint: "82a1b95ab613fd6d"
license: CC BY-SA 4.0
---

# nftables/rules/snat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The `snat` statement translates packet source addresses.

## Placement

The snat statement is valid only in a nat chain.

Place `snat` in a base chain having the **postrouting** or **input** hook. A regular chain called from such base chain can also use snat.

It belongs to the **statement** syntax group.

## Evaluation

The `snat` statement is terminal. It terminates evaluation of the current rule and implicitly issue an accept verdict.

The first packet of a connection establishes the source NAT mapping. Connection tracking applies the mapping to subsequent packets.

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

### Datacenter Egress NAT

Makes thousands of internal servers appear behind a designated public IP address.

**`/etc/nftables/rules/main.nft`**

**Datacenter egress NAT**

```
table ip nat {
    chain postrouting {
        type nat hook postrouting priority srcnat;
        policy accept;
        ip saddr 10.20.0.0/16 oifname "eth0" snat to 203.0.113.10
    }
}
```
### Outbound Traffic Distribution

Spreads outgoing connections from a large server farm across a pool of public IP addresses.

**`/etc/nftables/rules/main.nft`**

**Outbound traffic distribution**

```
table ip nat {
    chain postrouting {
        type nat hook postrouting priority srcnat;
        policy accept;
        ip saddr 10.20.0.0/16 oifname "eth0" snat to 203.0.113.10-203.0.113.20
    }
}
```
### Multi-WAN Source NAT

Assigns outgoing connections to the appropriate public IP address when traffic exits through different Internet service providers.

**`/etc/nftables/rules/main.nft`**

**Multi-WAN source NAT**

```
table ip nat {
    chain postrouting {
        type nat hook postrouting priority srcnat;
        policy accept;
        oifname "wan1" snat to 198.51.100.10
        oifname "wan2" snat to 203.0.113.10
    }
}
```
## Tips

- Use [`masquerade`](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade) for dynamically assigned outgoing addresses
- Use [`dnat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat) to translate destinations to a specified address.
- Use [`redirect`](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect) to redirect traffic to a local service

## Kernel configuration

For snat configuration, the relevant additional Linux Kconfig options include:

- `CONFIG_NF_NAT` - Netfilter NAT support
- `CONFIG_NF_TABLES` - nf\_tables framework
- `CONFIG_NFT_NAT` - nftables NAT statements
- `CONFIG_NF_CONNTRACK` - connection tracking

## See also

- [Nftables/Rules/masquerade](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade)
- [Nftables/Rules/Dnat](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat)
- [Nftables/Rules/Redirect](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
