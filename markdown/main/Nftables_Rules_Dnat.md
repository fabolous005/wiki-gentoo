<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Dnat -->
---
title: nftables/rules/dnat
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-05"
fingerprint: e4a1f95af223bd7f
license: CC BY-SA 4.0
---

# nftables/rules/dnat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The dnat statement translates packet destination addresses.

## Placement

The dnat statement is valid only in a nat chain.

Place dnat in a base chain with the prerouting or output hook. A regular chain called from such a base chain can also use dnat.

The dnat statement requires connection tracking.

## Evaluation

The dnat statement is terminal. It terminates evaluation of the current rule and implicitly issues an accept verdict.

The first packet of a connection establishes the destination NAT mapping. Connection tracking applies the mapping to subsequent packets.

## Syntax

dnat \[\[\[ip | ip6\] \[prefix\]\] to\] ADDR\_SPEC \[:PORT\_SPEC\] \[FLAGS\]
ADDR\_SPEC := address | address - address
PORT\_SPEC := port | port - port
FLAGS := FLAG \[, FLAGS\]
FLAG := persistent | random | fully-random

- ip, ip6 - Address family
- prefix - Enables prefix-based address mapping
- ADDR\_SPEC - IP address, single or inclusive address range

- PORT\_SPEC - Port number, single or inclusive port range
- persistent - Prefers consistent address selection for each client
- random - Randomizes port mapping
- fully-random - Uses fully randomized port mapping

## Examples

### Fixed destination address

Redirects incoming traffic to a specific IP address.

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
### Destination address range

Distributes incoming traffic across a range of IP addresses.

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
### Persistent DNAT

Redirects HTTP traffic to a backend server using a port range and persistent client mapping.

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

- Use persistent for rudimentary load distribution where maintaining client affinity matters.
- Use [masquerade](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade) for dynamically assigned outgoing addresses.
- Use [snat](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat) for known source addresses or address ranges.
- Use [redirect](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect) to redirect traffic to a local service.
- In an inet family chain, specify ip or ip6 when providing an address.

## Kernel configuration

The following Linux Kconfig options provide DNAT support:

- `CONFIG_NF_NAT` - Netfilter NAT support
- `CONFIG_NF_TABLES` - nf\_tables framework
- `CONFIG_NFT_NAT` - nftables NAT statements
- `CONFIG_NF_CONNTRACK` - Connection tracking

## See also

- [Nftables/Rules/Snat](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat)
- [Nftables/Rules/masquerade](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade)
- [Nftables/Rules/Redirect](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
