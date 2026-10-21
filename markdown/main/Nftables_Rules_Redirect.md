<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Redirect -->
---
title: nftables/rules/redirect
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Redirect
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-20"
fingerprint: "84a23938f3a346fd"
license: CC BY-SA 4.0
---

# nftables/rules/redirect

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The `redirect` statement translates packet destination addresses to the local host. It belongs to the **statement** syntax group.

## Kernel

For `redirect` configuration, the relevant additional Linux Kconfig options include:

- `CONFIG_NF_CONNTRACK` — connection tracking
- `CONFIG_NF_NAT` — Netfilter NAT support
- `CONFIG_NF_TABLES` — nf\_tables framework
- `CONFIG_NFT_REDIR` — nftables redirect statement support (the corresponding Kconfig symbol may vary by kernel version)
- `CONFIG_NF_NAT_REDIRECT` — underlying NAT redirect support, selected by CONFIG\_NFT\_REDIR

## Placement

Place `redirect` statement in a **nat** base chain having the **prerouting** or **output** hook, or in a regular chain called from such a base chain.

## Evaluation

The `redirect` statement is terminal because they implicitly issue an `accept` verdict.

The `redirect` statement establishes a destination NAT mapping for the connection's first packet. Subsequent packets use the established mapping.

## Syntax

redirect \[to :PORT\_SPEC\] \[FLAGS\]
PORT\_SPEC := port | port - port
FLAGS := FLAG \[, FLAGS\]
FLAG := persistent | random | fully-random

- `PORT_SPEC` - Destination port or inclusive port range
- `persistent` - Prefer consistent address selection for each client
- `random` - Randomize port mapping
- `fully-random` - Use fully randomized port mapping



**`/etc/nftables/rules/main.nft`**

**Redirect statement**

```
redirect
```
The optional `to` parameter specifies a destination port or port range. Without a port specification, the destination port remains unchanged.

## Examples

Redirect incoming TCP traffic on port 80 to local port 8080:

**`/etc/nftables/rules/main.nft`**

**Redirect incoming traffic**

```
table ip nat {
    chain prerouting {
        type nat hook prerouting priority dstnat;
        policy accept;
        tcp dport 80 redirect to :8080
    }
}
```
Redirect locally generated TCP traffic on port 853 to local port 10053:

**`/etc/nftables/rules/main.nft`**

**Redirect locally generated traffic**

```
table ip nat {
    chain output {
        type nat hook output priority dstnat;
        policy accept;
        tcp dport 853 redirect to :10053
    }
}
```
Redirect incoming TCP traffic to a port range with randomized port mapping:

**`/etc/nftables/rules/main.nft`**

**Redirect with port range and flags**

```
table ip nat {
    chain prerouting {
        type nat hook prerouting priority dstnat;
        policy accept;
        tcp dport 80 redirect to :8080-8090 fully-random
    }
}
```
## Tips

- Use [`masquerade`](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade) for dynamically assigned outgoing addresses
- Use [`dnat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat) to translate destinations to a specified address.
- Use [`snat`](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat) for known source addresses or defined address ranges.
- Use [`redirect`] to redirect traffic to a local service
- Configure the local service to listen on the translated port.

## See also

- [Nftables/Rules/Dnat](https://wiki.gentoo.org/wiki/Nftables/Rules/Dnat)
- [Nftables/Rules/Snat](https://wiki.gentoo.org/wiki/Nftables/Rules/Snat)
- [Nftables/Rules/masquerade](https://wiki.gentoo.org/wiki/Nftables/Rules/masquerade)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
