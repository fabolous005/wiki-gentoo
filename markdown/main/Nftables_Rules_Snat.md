<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Snat | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Snat -->
---
title: nftables/rules/snat
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Snat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-17"
fingerprint: "82a3395af23fb55d"
license: CC BY-SA 4.0
---

# nftables/rules/snat

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The `snat` statement translates packet source addresses. It belongs to the **statement** syntax group.

### Placement

Place `snat` in a **nat** chain with the **postrouting** or **input** hook, or in a regular chain called from such base chain.

### Evaluation

The `snat` statement establishes a source NAT mapping for the connection's first packet. Subsequent packets use the established mapping.

### Syntax

snat \[\[\[ip | ip6\] \[prefix\]\] to\] ADDR\_SPEC \[:PORT\_SPEC\] \[FLAGS\]
ADDR\_SPEC := address | address - address
PORT\_SPEC := port | port - port
FLAGS := FLAG \[, FLAGS\]
FLAG := persistent | random | fully-random

- `ip`, `ip6`: Address family.
- `prefix`: Enable prefix-based address mapping.
- `ADDR_SPEC`: Single address or inclusive address range.
- `PORT_SPEC`: Single port or inclusive port range.
- `persistent`: Prefer consistent address selection for each client.
- `random`: Randomize source port mapping.
- `fully-random`: Use fully randomized source port mapping.



FILE **`/etc/nftables/rules/main.nft`****SNAT statement**

```
snat to 192.0.2.1
```
The `to` parameter accepts an address, address range, or address and port range. Optional flags include `persistent`, `random`, and `fully-random`.

### Examples

Translate source addresses to a fixed address:

FILE **`/etc/nftables/rules/main.nft`****Fixed source address**

```
ip saddr 192.168.1.0/24 snat to 192.0.2.1
```
Translate source addresses to an address range:

FILE **`/etc/nftables/rules/main.nft`****Source address range**

```
snat to 192.0.2.1-192.0.2.10
```
SNAT with prefix, port range, and persistent flag:

FILE **`/etc/nftables/rules/main.nft`****SNAT with prefix, port range, and persistent flag**

```
snat ip prefix to 192.168.1.0/24:1024-65535 persistent
```
### Tips

- Use `masquerade` for dynamically assigned outgoing addresses.
- Use `snat` for known addresses or defined address ranges.

## See also

- [Nftables/Rules/masquerade](https://wiki.gentoo.org/index.php?title=Nftables/Rules/masquerade&action=edit&redlink=1)
- [Nftables/Rules/dnat](https://wiki.gentoo.org/index.php?title=Nftables/Rules/dnat&action=edit&redlink=1)
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
