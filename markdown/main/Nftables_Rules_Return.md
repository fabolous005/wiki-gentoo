<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Return | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Return -->
---
title: nftables/rules/return
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Return
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-07"
fingerprint: fdb09d1afe48482f
license: CC BY-SA 4.0
---

# nftables/rules/return

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The return verdict terminates evaluation of the current chain and returns packet processing to the calling chain, if one exists.

## Placement

The return verdict is valid in both base and regular chains.

## Evaluation

The return verdict is terminal. It terminates evaluation of the current rule and returns packet processing to the most recent saved return position in the call stack.

When invoked in a regular chain, processing resumes at the rule following the most recent jump verdict.

When invoked in a base chain, or when no saved return position exists, evaluation ends and the base chain's policy applies.

## Syntax

`return`
## Examples

### Return from a regular chain

Delegates SSH traffic to a dedicated filtering chain and explicitly returns to the calling base chain when no rule accepts the packet.

**`/etc/nftables/rules/main.nft`**

**Return from a regular chain**

```
table inet my_inet_filter {
    chain my_inet_filter_input {
        type filter hook input priority filter;
        policy drop;
        tcp dport 22 jump input_ssh
        ct state established,related accept
    }
    chain input_ssh {
        ip saddr 192.0.2.0/24 accept
        return
    }
}
```
Packets originating from 192.168.1.0/24 receive an accept verdict. Other SSH packets reach return and resume processing at the ct state established,related rule in the calling chain.

### Bisecting ruleset

Explicitly ends evaluation of the base chain and applies its configured policy.

**`/etc/nftables/rules/main.nft`**

**Return from a base chain**

```
table inet my_inet_filter {
    chain my_inet_filter_input {
        type filter hook input priority filter;
        policy drop;
        iifname "lo" accept
        return
        ct state established,related accept
    }
}
```
The return verdict ends evaluation of my\_inet\_filter\_input. The chain's drop policy applies. The subsequent ct state established,related rule is never evaluated.

### Pre-portknocking

Excludes locally generated loopback traffic from port-knocking inspection.

**`/etc/nftables/rules/portknock.nft`**

**Loopback traffic exemption**

```
table inet portknock {
    chain inet_portknocking_input {
        type filter hook input priority -10;
        policy accept;
        iifname "lo" return
        # Port-knocking rules follow.
    }
}
```
Loopback traffic bypasses the remaining rules in this chain. All loopback traffic uses implied accept policy. Allows remaining chain rules to perform on non-loopback traffic (such as port-knocking).

## Tips

- Use return to explicitly end evaluation of a regular chain and resume processing in its calling chain.
- Use [jump](https://wiki.gentoo.org/wiki/Nftables/Rules/Jump) to invoke a regular chain while saving a return position, like a procedure/function call.
- Use [goto](https://wiki.gentoo.org/wiki/Nftables/Rules/Goto) to transfer processing without establishing a new return position.
- An implicit return occurs when processing reaches the end of a regular chain.
- In a base chain, return applies the chain's configured policy.

## Kernel configuration

The following Linux Kconfig option provides nftables verdict support:

- `CONFIG_NF_TABLES` - nf\_tables framework

## See also

- [Nftables/Rules/Jump](https://wiki.gentoo.org/wiki/Nftables/Rules/Jump) — transfers packet processing to another regular chain
- [Nftables/Rules/Goto](https://wiki.gentoo.org/wiki/Nftables/Rules/Goto) — transfers packet processing to another regular chain without saving a return position in the current chain.
- [Nftables/Rules/Drop](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Drop&action=edit&redlink=1)
- [Nftables/Rules/Accept](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Accept&action=edit&redlink=1)
- [Nftables/Rules/Continue](https://wiki.gentoo.org/wiki/Nftables/Rules/Continue) — continues packet processing with the next rule in the current chain.
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
