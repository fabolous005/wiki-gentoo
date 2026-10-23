<!-- source: https://wiki.gentoo.org/wiki/Nftables/Ruleset/Add | group: Gentoo Wiki (Main) | wiki-title: Nftables/Ruleset/Add -->
---
title: Nftables/Ruleset/Add
url: https://wiki.gentoo.org/wiki/Nftables/Ruleset/Add
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-22"
fingerprint: c5b1dd5e0b930d6d
license: CC BY-SA 4.0
---

# Nftables/Ruleset/Add

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **add** command adds objects or elements to the Netfilter **nf\_tables** kernel ruleset.

## Placement

The `add` keyword begins a command.

Place the `add` command at the [nft](https://wiki.gentoo.org/wiki/Nft) command line or in a ruleset file.

## Evaluation

The `add` command adds a new object or element to the kernel ruleset.

The command can add a table, `chain`, `rule`, **set**, \`map\`, flowtable, stateful object, or element to an existing set or map.

Adding an object that already exists fails.

Use the create command when an existing object must cause an error instead of being reused.

The add rule command appends a rule to a chain unless a position is specified.

The add element command adds one or more elements to an existing set or map. The element data must conform to the set or map definition.

## Syntax

```
add table  [FAMILY] TABLE_ID [TABLE_SPEC]
add chain  [FAMILY] TABLE_ID CHAIN_ID [CHAIN_SPEC]
add rule   [FAMILY] TABLE_ID CHAIN_ID [RULE_POSITION] STATEMENT ...
add set    [FAMILY] TABLE_ID SET_ID SET_SPEC
add map    [FAMILY] TABLE_ID MAP_ID MAP_SPEC
add element [FAMILY] TABLE_ID SET_ID { ELEMENTS }
add flowtable [FAMILY] TABLE_ID FLOW_ID FLOW_SPEC
add counter [FAMILY] TABLE_ID COUNTER_ID [COUNTER_SPEC]
add quota   [FAMILY] TABLE_ID QUOTA_ID QUOTA_SPEC
add limit   [FAMILY] TABLE_ID LIMIT_ID LIMIT_SPEC
add secmark [FAMILY] TABLE_ID SECMARK_ID SECMARK_SPEC
add synproxy [FAMILY] TABLE_ID SYNPROXY_ID SYNPROXY_SPEC
add ct helper [FAMILY] TABLE_ID CT_ID CT_HELPER_SPEC
add ct timeout [FAMILY] TABLE_ID CT_ID CT_TIMEOUT_SPEC
add ct expectation [FAMILY] TABLE_ID CT_ID CT_EXPECTATION_SPEC
```
where:

- FAMILY - address family, such as ip, ip6, inet, netdev, bridge, or arp.
- TABLE\_ID - name of the [**table**](https://wiki.gentoo.org/wiki/Nftables/Ruleset#tables) block.
- CHAIN\_ID - name of the [**chain**](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) block.
- RULE\_POSITION - optional rule insertion position, specified using a supported handle or index form.
- STATEMENT - rule statement.
- SET\_ID - name of the **set** or **map** block.
- ELEMENTS - one or more elements matching the set or map definition.
- TABLE\_SPEC, CHAIN\_SPEC, SET\_SPEC, MAP\_SPEC, FLOW\_SPEC, and the stateful-object specifications - object-specific configuration.

## Examples

### Add a table

To add an IPv4 table named filter:

`root #``nft add table ip filter`
### Add a chain

To add an input chain to the IPv4 filter table:

`root #``nft add chain ip filter input`
### Add a rule

To append a rule accepting TCP traffic to port 22:

`root #``nft add rule ip filter input tcp dport 22 accept`
The table and chain must already exist.

### Add an element

To add an IPv4 address to an existing set named trusted\_ips in the filter table:

`root #``nft add element ip filter trusted_ips { 192.168.3.10 }`
The set must already exist, and its definition must accept IPv4 addresses.

### Add a set

To add a set of IPv4 addresses:

`root #``nft add set ip filter trusted_ips { type ipv4_addr ; }`
The set definition specifies the data type accepted by its elements.

## Tips

- The parent table must exist before adding objects that belong to it.
- The referenced chain must exist before adding a rule to it.
- The referenced set or map must exist before adding elements.
- Use nft -c -f FILE to check a ruleset file before applying its commands.
- For persistent ruleset administration, see [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration).

## See also

- [Nftables/Ruleset/List](https://wiki.gentoo.org/wiki/Nftables/Ruleset/List)
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — defines the collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem used for [nftables](https://wiki.gentoo.org/wiki/Nftables) packet handling.
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
