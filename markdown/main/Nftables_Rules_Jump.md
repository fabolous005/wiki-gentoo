<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Jump | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Jump -->
---
title: nftables/rules/jump
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Jump
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-06"
fingerprint: fdb09d1afd49582f
license: CC BY-SA 4.0
---

# nftables/rules/jump

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The jump verdict transfers packet processing to another regular chain while saving a return position in the current chain.

## Placement

The jump verdict is valid in both base and regular chains.

The destination must be a regular chain in the same table. A base chain cannot be used as a destination.

## Evaluation

The jump verdict is terminal. It terminates evaluation of the current rule and transfers packet processing to the first rule of the destination chain.

Unlike goto, jump saves a return position in the current chain.

When the destination chain finishes, processing resumes at the rule following the original jump. An explicit return verdict has the same effect.

## Syntax

`jump CHAIN`
- CHAIN - Name of the destination regular chain in the same table.

## Examples

### Interface-specific ingress filtering

Delegates incoming traffic to interface-specific filtering chains while retaining subsequent rule evaluation in the base chain.

**`/etc/nftables/rules/main.nft`**

**Interface-specific ingress filtering**

```
table inet my_inet_filter {
    chain my_inet_filter_input {
        type filter hook input priority filter;
        policy drop;
        iifname "lan0" jump input_lan
        iifname "wan0" jump input_wan
        ct state established,related accept
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

Delegates WAN traffic to a dedicated filtering chain while retaining the return positions established by earlier jump verdicts.

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
        iifname "wan0" jump input_wan
        return
    }
    chain input_wan {
        tcp dport 443 accept
    }
}
```
## Tips

- Use jump to delegate packet processing while retaining evaluation of subsequent rules in the calling chain.
- Use [goto](https://wiki.gentoo.org/wiki/Nftables/Rules/Goto) when processing should transfer to another chain without establishing a new return position.
- Use [return](https://wiki.gentoo.org/wiki/Nftables/Rules/Return) to explicitly end evaluation of the current chain.
- The destination must be a regular chain in the same table.

## Kernel configuration

The following Linux Kconfig option provides nftables verdict support:

- `CONFIG_NF_TABLES` - nf\_tables framework

## See also

- [Nftables/Rules/Goto](https://wiki.gentoo.org/wiki/Nftables/Rules/Goto) — transfers packet processing to another regular chain without saving a return position in the current chain.
- [Nftables/Rules/Return](https://wiki.gentoo.org/wiki/Nftables/Rules/Return) — terminates evaluation of the current chain and returns packet processing to the calling chain, if one exists.
- [Nftables/Rules/Drop](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Drop&action=edit&redlink=1)
- [Nftables/Rules/Accept](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Accept&action=edit&redlink=1)
- [Nftables/Rules/Continue](https://wiki.gentoo.org/wiki/Nftables/Rules/Continue) — continues packet processing with the next rule in the current chain.
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
