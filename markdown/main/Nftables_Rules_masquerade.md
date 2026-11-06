<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/masquerade -->
---
title: nftables/rules/masquerade
url: https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-05"
fingerprint: aa80d91af28bfd6d
license: CC BY-SA 4.0
---

# nftables/rules/masquerade

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The `masquerade` statement translates packet source addresses to the outgoing interface address. It belongs to the **statement** syntax group.

## Placement

Place `masquerade` in a **nat** type of base chain having the **postrouting** hook, or in a regular chain called from such a base chain.

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

## Usage

Masquerading automatically selects the source address from the outgoing interface. This eliminates the need to update NAT rules when the interface address changes.

For interfaces with static-assigned addresses, snat is generally preferable because the translated address is already known.

## Examples

### Home and Office Internet Gateway

Shares a dynamically assigned ISP address among private LAN clients.

**`/etc/nftables/rules/main.nft`**

**Home and office Internet gateway**

```
table ip ip_nat {
    chain ip_nat_postrouting {
        type nat hook postrouting priority srcnat;
        policy accept;
        ip saddr 192.168.10.0/24 oifname "ppp0" masquerade
    }
}
```
### Container Network Egress

Allows container workloads to access external networks through the host's network interface.

**`/etc/nftables/rules/main.nft`**

**Container network egress**

```
table ip ip_nat {
    chain ip_nat_postrouting {
        type nat hook postrouting priority srcnat;
        policy accept;
        ip saddr 172.20.0.0/16 oifname "ens3" masquerade
    }
}
```
### VPN Client Internet Access

Allows remote VPN clients to access the Internet through the VPN gateway's WAN interface.

**`/etc/nftables/rules/main.nft`**

**VPN client Internet access**

```
table ip ip_nat {
    chain ip_nat_postrouting {
        type nat hook postrouting priority srcnat;
        policy accept;
        ip saddr 10.8.0.0/24 oifname "eth0" masquerade
    }
}
```
## Tips

- Use [`dnat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat) to translate destinations to a specified address.
- Use [`snat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat) for known source addresses or defined address ranges.
- Use [`redirect`](https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect) to redirect traffic to a local service

## Kernel configuration

For `masquerade` configuration, the relevant additional Linux Kconfig options include:

- `CONFIG_NF_TABLES` - nf\_tables framework
- `CONFIG_NF_NAT` - Netfilter NAT support
- `CONFIG_NFT_NAT` - nftables NAT statements
- `CONFIG_NFT_REDIR` - nftables redirect statement support (the corresponding Kconfig symbol may vary by kernel version)

## See also

- [Nftables/Rules/Snat](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat)
- [Nftables/Rules/Dnat](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
