<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules -->
---
title: Nftables/Rules
url: https://wiki.gentoo.org/wiki/Nftables/Rules
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-30"
fingerprint: fdbc9d1efb0bcfad
license: CC BY-SA 4.0
---

# Nftables/Rules

[Nftables](https://wiki.gentoo.org/wiki/Nftables)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Rules** match packet properties and execute statements. Chains contain ordered rules; tables contain chains.

## Placement

Rule placement determines evaluation order within a chain.

The add rule command appends a rule by default. The add rule command can appends a rule after a specified rule using position.

The insert rule prepends a rule by default. The insert rule command can prepends a rule before a specified rule using position.

Both commands accept position to place a rule relative to an existing rule handle.

## Evaluation

Rule evaluation processes expressions and statements in sequence.

Matching expressions execute their statements; a terminating verdict ends the current rule evaluation and determines control flow.

The iif expression matches the input interface index, resolved from the specified interface name when the rule is added.

The interface must exist when either the iif or meta iif expression is added.

Renaming the interface does not affect the match because the interface index is resolved when the rule is added to the ruleset.

Deleting and recreating the interface may assign a different index, causing the rule to stop matching the replacement interface. For example:

1. eth0 exists with interface index 2.
2. You add iif eth0 drop. nftables resolves eth0 to index 2.
3. You delete eth0. The rule still compares incoming packets against index 2.
4. You recreate eth0. The kernel might assign it index 5.
5. The rule still matches index 2, so packets arriving on the replacement eth0 do not match it.

The important details are:

- iif eth0 and meta iif eth0 both resolve eth0 to an interface index when the rule is added.
- The rule can only be added if eth0 exists at that time.
- Renaming eth0 to lan0 does not break the match, because the rule uses the index.
- If the interface is deleted, a newly created interface may receive a different index, so the old rule will not necessarily match it.
- iifname eth0 matches the name instead: the interface need not exist when the rule is added, and a recreated interface named eth0 can match again.

## Rule syntax

A rule consists of expressions and statements. Expressions determine whether a packet matches; statements perform actions on matching packets.

`nft add rule [<family>] TABLE_ID CHAIN_ID <expressions> <statements>`
The family can be omitted when it defaults to ip. A rule can also include a comment.

**`/etc/nftables/rules.d/ssh-server.nft`**

**SSH rule fragment**

```
tcp dport 22 accept
```
## Rule management

Rules are added, inserted, replaced, deleted, or reset with the corresponding nft command. Rule handles identify existing rules and can be displayed with the -a option.

| Command | Description | 
|---|---|
| add rule | Appends a rule to a chain, or places it after a specified rule. | 
| insert rule | Inserts a rule at the beginning of a chain, or before a specified rule. | 
| replace rule | Replaces an existing rule. | 
| delete rule | Deletes an existing rule. | 
| destroy rule | Deletes an existing rule without failing if it does not exist. | 
| reset rule | Resets stateful expression values in a rule, such as counter and quota values. | 
| reset rules | Resets rule state within the specified scope. | 

### Add a rule

The add rule command appends a rule to the end of a chain by default.

nft add rule \<family> \<table> \<chain> \<expressions> \<statements>

**`/etc/nftables/rules.d/allow-ssh.nft`**

**Allow SSH**

```
tcp dport 22 counter accept
```
A rule can be placed after an existing rule by specifying its handle or index.

nft add rule \<family> \<table> \<chain> handle \<handle> \<expressions> \<statements>

### Insert a rule

The insert rule command inserts a rule at the beginning of a chain by default. A handle or index can specify the existing rule before which the new rule is inserted.

nft insert rule \<family> \<table> \<chain> handle \<handle> \<expressions> \<statements>

### Replace a rule

The replace rule command replaces the rule identified by its handle.

nft replace rule \<family> \<table> \<chain> handle \<handle> \<expressions> \<statements>

### Delete a rule

The delete rule command removes the rule identified by its handle.

nft delete rule \<family> \<table> \<chain> handle \<handle>

Use nft -a list chain to display rule handles before deleting or replacing a rule.

### Destroy a rule

The destroy rule command removes the rule identified by its handle. Unlike delete rule, it does not fail when the specified rule does not exist.

nft destroy rule \<family> \<table> \<chain> handle \<handle>

### Reset rule state

The reset rule command resets stateful expression values in a specified rule, such as packet counters and quota usage.

nft reset rule \<family> \<table> \<chain> handle \<handle>

The reset rules command resets rule state within a specified family, table, or chain.

nft reset rules \[\<family> \[table \[\<chain>\]\]\]

## Expressions

Expressions match packet properties, packet metadata, connection-tracking state, or other values. They support comparisons, ranges, sets, and combinations.

Common expressions include:

- ip saddr — source IPv4 address.
- ip daddr — destination IPv4 address.
- ip6 saddr — source IPv6 address.
- ip6 daddr — destination IPv6 address.
- tcp dport — TCP destination port.
- udp dport — UDP destination port.
- meta iifname — incoming interface name.
- ct state — connection-tracking state.

**`/etc/nftables/rules.d/basic-expressions.nft`**

**Expression examples**

```
ip saddr 192.0.2.0/24 drop
tcp dport { 80, 443 } accept
ct state established,related accept
meta iifname "eth0" accept
meta iif not in { 1, 2 } drop
```
## Statements

Statements perform actions on matching packets. Non-terminal statements can precede a verdict, allowing multiple actions within one rule.

Common statements include:

- counter — counts matching packets and bytes.
- log — logs matching packets.
- limit — limits the packet rate.
- Packet modification statements — modify packet headers or metadata.
- Verdict statements — control rule and chain evaluation.

**`/etc/nftables/rules.d/logged-drop.nft`**

**Log and drop matching packets**

```
log prefix "dropped: " drop
```
The log statement is non-terminal; drop terminates evaluation by discarding the packet.

## Verdicts

Verdicts determine how rule evaluation proceeds for a matching packet.

| Verdict | Description | 
|---|---|
| accept | Accepts the packet at the current hook. A later base chain can still drop it. | 
| drop | Discards the packet immediately. | 
| jump | Transfers control to a regular chain and saves the return position. | 
| goto | Transfers control to a regular chain without saving the current return position. | 
| reject | Discards the packet and, when possible, sends an error response. | 
| return | Ends evaluation of the current chain. A regular chain returns to its caller; a base chain applies its policy. | 

## Hub page

This is a hub page for nftables rules and rule-related keywords.

The following pages provide keyword guides for configuring [Nftables](https://wiki.gentoo.org/wiki/Nftables) rules and command files for [Netfilter](https://wiki.gentoo.org/wiki/Netfilter).

## See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem.
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules] — match packet properties and execute statements.
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
