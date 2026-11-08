<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Continue | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Continue -->
---
title: nftables/rules/continue
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Continue
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-07"
fingerprint: fdb09d5ad469491f
license: CC BY-SA 4.0
---

# nftables/rules/continue

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The continue verdict continues packet processing with the next rule in the current chain.

## Placement

The continue verdict is valid in both base and regular chains.

## Evaluation

The continue verdict is terminal. continue terminates evaluation of the current rule and proceeds to the next rule in the same chain.

The current chain remains active. Subsequent rules are evaluated until another verdict terminates packet processing or the end of the chain is reached.

A matching rule without a terminating verdict implicitly proceeds to the next rule in the same chain. The continue verdict explicitly specifies this behavior.

In a regular chain invoked by jump, the saved return position remains unchanged. Processing continues with the next rule in the invoked chain.

The continue verdict is largely redundant in ordinary nftables rulesets. Its primary purpose is to make rule progression explicit.

## Syntax

`continue`


## Examples

### SSH connection logging

Logs incoming SSH connection attempts before evaluating subsequent access-control rules.

**`/etc/nftables/rules/main.nft`**

**SSH connection logging**

```
table inet my_inet_filter {
    chain my_inet_filter_input {
        type filter hook input priority filter;
        policy drop;
        tcp dport 22 log prefix "Incoming SSH connection attempt: " continue
        ip saddr 192.168.1.0/24 tcp dport 22 log prefix "SSH connection made: " accept
    }
}
```
Incoming SSH connection attempts are logged before packet processing continues.

- Packets originating from 192.168.1.0/24 receive an accept verdict.
- Other SSH packets continue through the remaining rules and ultimately receive the base chain's drop policy.

### Packet header modification

Sets the packet's DSCP value before evaluating subsequent filtering rules.

**`/etc/nftables/rules/main.nft`**

**Packet header modification**

```
table inet my_inet_filter {
    chain prerouting {
        type filter hook prerouting priority mangle;
        policy accept;
        ip daddr 192.168.1.10 ip dscp set cs1 continue
    }
    chain input {
        type filter hook input priority filter;
        policy drop;
        ip daddr 192.168.1.10 accept
    }
}
```
Packets destined for 192.168.1.10 receive the cs1 DSCP value. The continue verdict proceeds to the next rule in prerouting without terminating evaluation.

### Explicit rule progression

Explicitly continues packet evaluation after matching traffic from a trusted local network.

**`/etc/nftables/rules/main.nft`**

**Explicit rule progression**

```
table inet my_inet_filter {
    chain input {
        type filter hook input priority filter;
        policy drop;
        ip saddr 192.168.1.0/24 continue
        ct state established,related accept
        tcp dport 22 accept
    }
}
```
Packets originating from 192.168.1.0/24 continue to the subsequent rules. The continue verdict does not bypass connection-state or SSH filtering. Other matching packets are evaluated by the same subsequent rules.

## Tips

Quick summary of drop, return, and continue statements:

| Rule | Matching source | Non-matching source | 
|---|---|---|
| ip saddr 192.168.1.0/24 continue | Next rule | Next rule | 
| ip saddr ! 192.168.1.0/24 drop | Next rule | Drop packet | 
| ip saddr ! 192.168.1.0/24 return | Next rule | Return from chain | 

- Use continue to explicitly proceed to the next rule in the current chain.
- Use return to terminate evaluation of the current chain and resume processing at the calling chain.
- Use jump to invoke another regular chain while retaining a return position.

## Kernel configuration

The following Linux Kconfig option provides nftables verdict support:

- `CONFIG_NF_TABLES` - nf\_tables framework

## See also

- [Nftables/Rules/Jump](https://wiki.gentoo.org/wiki/Nftables/Rules/Jump) — transfers packet processing to another regular chain
- [Nftables/Rules/Goto](https://wiki.gentoo.org/wiki/Nftables/Rules/Goto) — transfers packet processing to another regular chain without saving a return position in the current chain.
- [Nftables/Rules/Return](https://wiki.gentoo.org/wiki/Nftables/Rules/Return) — terminates evaluation of the current chain and returns packet processing to the calling chain, if one exists.
- [Nftables/Rules/Drop](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Drop&action=edit&redlink=1)
- [Nftables/Rules/Accept](https://wiki.gentoo.org/index.php?title=Nftables/Rules/Accept&action=edit&redlink=1)
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
