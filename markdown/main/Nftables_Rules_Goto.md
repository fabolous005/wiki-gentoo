<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Goto | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Goto -->
---
title: nftables/rules/goto
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Goto
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-05"
fingerprint: fdb0991afd49582f
license: CC BY-SA 4.0
---

# nftables/rules/goto

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The goto verdict transfers packet processing to another regular chain without saving a return position in the current chain.

## Placement

The goto verdict is valid in both base and regular chains.

The destination must be a regular chain in the same table. A base chain cannot be used as a destination.

## Evaluation

The goto verdict is terminal. It terminates evaluation of the current rule and transfers packet processing to the first rule of the destination chain.

Unlike jump, goto does not save a return position in the current chain.

When the destination chain finishes, processing resumes after the most recent jump in the call stack, if one exists. Otherwise, evaluation of the current base chain ends and its policy applies.

## Syntax

`goto CHAIN`
- CHAIN - Name of the destination regular chain in the same table.

## Examples

### Interface-specific ingress filtering

Delegates incoming traffic to interface-specific filtering chains without evaluating subsequent rules in the base chain.

**`/etc/nftables/rules/main.nft`**

**Interface-specific ingress filtering**

```
table inet my_inet_filter {
    chain my_inet_filter_input {
        type filter hook input priority filter;
        policy drop;
        iifname "lan0" goto input_lan
        iifname "wan0" goto input_wan
    }
    chain input_lan {
        tcp dport 22 accept
        udp dport 53 accept
    }
    chain input_wan {
        tcp dport 443 accept
    }
}
```
### Nested chain delegation

Delegates WAN traffic to a dedicated filtering chain while retaining the return position established by an earlier jump.

**`/etc/nftables/rules/main.nft`**

**Nested chain delegation**

```
table inet my_inet_filter {
    chain my_inet_filter_input {
        type filter hook input priority filter;
        policy drop;
        jump input_dispatch
        iifname "lan0" tcp dport 22 accept
    }
    chain input_dispatch {
        iifname "wan0" goto input_wan
        return
    }
    chain input_wan {
        tcp dport 443 accept
    }
}
```
## Tips

- Use goto to delegate packet processing without returning to the remaining rules in the current chain.
- Use [jump](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Jump&action=edit&redlink=1) when processing should return to the rule following the chain invocation.
- Use [return](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Return&action=edit&redlink=1) to explicitly end evaluation of the current chain.
- The destination must be a regular chain in the same table.

## Kernel configuration

The following Linux Kconfig option provides nftables verdict support:

- `CONFIG_NF_TABLES` - nf\_tables framework

## See also

- [Nftables/Rules/Jump](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Jump&action=edit&redlink=1)
- [Nftables/Rules/Return](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Return&action=edit&redlink=1)
- [Nftables/Rules/Drop](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Drop&action=edit&redlink=1)
- [Nftables/Rules/Accept](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Accept&action=edit&redlink=1)
- [Nftables/Rules/Continue](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Continue&action=edit&redlink=1)
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
