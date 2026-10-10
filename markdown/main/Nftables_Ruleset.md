<!-- source: https://wiki.gentoo.org/wiki/Nftables/Ruleset | group: Gentoo Wiki (Main) | wiki-title: Nftables/Ruleset -->
---
title: Nftables/Ruleset
url: https://wiki.gentoo.org/wiki/Nftables/Ruleset
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-09"
fingerprint: a585897a84e529c9
license: CC BY-SA 4.0
---

# Nftables/Ruleset

[Nftables](https://wiki.gentoo.org/wiki/Nftables)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Introduction

## Ruleset

nft command files may include other command files with include.

The include command allows a ruleset to be split into an administrator-defined directory hierarchy. nft does not impose a particular directory layout.

For example, a large homelab or enterprise ruleset might be organized as::

`root #``ls -R /etc/nftables/rules`
/etc/nftables/rules/main.nft
/etc/nftables/rules/filter/filter.nft
/etc/nftables/rules/filter/filter-bad-packets.nft
/etc/nftables/rules/filter/pre-routing/filter-prerouting.nft
/etc/nftables/rules/filter/postrouting/filter-postrouting.nft
/etc/nftables/rules/filter/ingress/filter-ingress.nft
/etc/nftables/rules/filter/output/filter-output.nft
/etc/nftables/rules/filter/output/lo/filter-output.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan-icmp.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan-tcp.nft
/etc/nftables/rules/filter/output/wan/filter-output-wan-udp.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan-icmp.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan-tcp.nft
/etc/nftables/rules/filter/output/lan/filter-output-lan-udp.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz-icmp.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz-tcp.nft
/etc/nftables/rules/filter/output/dmz/filter-output-dmz-udp.nft
/etc/nftables/rules/filter/input/filter-input.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan-icmp.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan-tcp.nft
/etc/nftables/rules/filter/input/wan/filter-input-wan-udp.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan-icmp.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan-tcp.nft
/etc/nftables/rules/filter/input/lan/filter-input-lan-udp.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz-icmp.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz-tcp.nft
/etc/nftables/rules/filter/input/dmz/filter-input-dmz-udp.nft
/etc/nftables/rules/filter/input/lo/filter-input.nft
/etc/nftables/rules/filter/forward/forward-input.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan-icmp.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan-tcp.nft
/etc/nftables/rules/filter/forward/wan/forward-input-wan-udp.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan-icmp.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan-tcp.nft
/etc/nftables/rules/filter/forward/lan/forward-input-lan-udp.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz-icmp.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz-tcp.nft
/etc/nftables/rules/filter/forward/dmz/forward-input-dmz-udp.nft
/etc/nftables/rules/filter/forward/lo/forward-input.nft
/etc/nftables/rules/filter/forward/forward-output.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan-icmp.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan-tcp.nft
/etc/nftables/rules/filter/forward/wan/forward-output-wan-udp.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan-icmp.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan-tcp.nft
/etc/nftables/rules/filter/forward/lan/forward-output-lan-udp.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz-icmp.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz-tcp.nft
/etc/nftables/rules/filter/forward/dmz/forward-output-dmz-udp.nft
/etc/nftables/rules/filter/forward/lo/forward-output.nft

## Tables

Tables provide the top-level namespace for nftables objects.

A table belongs to a family such as ip, ip6, inet, bridge, netdev, and arp.

Tables contain:

- chains;
- sets;
- maps; and
- flowtables.


Rules belong to chains, while sets, maps, and flowtables can be referenced by rules.

Sample table syntax

### Address families

Address families classify tables by the network protocol or packet-processing path they handle.

The ip family handles IPv4 traffic. The ip6 family handles IPv6 traffic. The inet family handles both IPv4 and IPv6 traffic. The arp, bridge, and netdev families handle other packet-processing paths.

Each table belongs to one address family. The family limits the protocols, hooks, and other nftables features available to the table.

### Table declaration

A table declaration creates a table and assigns it an address family.

The basic syntax is:

The family specifies the address family. The table\_name identifies the table within that family.

For example:

The inet family allows the table to contain rules for both IPv4 and IPv6 traffic.

### Table management

Tables are managed with the nft command.

The nft command can create, list, flush, rename, and delete tables. Table management operates on the active kernel ruleset.

A table can be created explicitly with nft add table or declared as part of a command file. An existing table can contain chains, sets, maps, flowtables, and other nftables objects.

Deleting a table also deletes the objects contained by that table. Flushing a table removes its contained objects while leaving the table itself in place.

## Rules

A rule is an ordered element of a chain.

Rules contain expressions.

Expressions determine whether a packet or other networking state matches.

Expressions are followed by statements and verdicts that determine what processing occurs.

For rule syntax and the components of a rule, see [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules).

#### Adding rules

The following command adds a **rule** to the **chain** called *input\_filter*, on the *base\_table* **table**, dropping all incoming traffic to port 80:

`root #``nft add rule inet base_table input_filter tcp dport 80 drop`
See [Nftables/Ruleset/Rules](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Rules) for details.

## Chains

Chains belong to a table and may be either base chains or regular chains.

For chain types, declaration, properties, lifecycle, relationships, and processing semantics, see [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain).

## Sets

Sets group values for use by rules.

A set contains elements of a single data type. A rule can match a packet value against multiple set elements with one match.

Sets commonly contain IP addresses, network addresses, ports, or interface names. Sets can also contain other data types supported by nftables.

### Named sets

Named sets have an administrator-defined name.

A named set can be referenced by multiple rules. Its elements can be modified without modifying the rules that reference the set.

### Anonymous sets

Anonymous sets have no name.

An anonymous set belongs to the rule that defines it. Its elements cannot be managed independently by name.

### Set elements

Set elements are the values contained in a set.

A set element can contain a single value or an interval of values. Named-set elements can be added, deleted, or replaced independently of rules that reference the set.

### Intervals

Intervals represent ranges of values within a set.

An interval set can match a range of addresses, ports, or other ordered values without listing each value individually.

## Maps

Maps associate keys with values and allow rules to use a matched value to select another value or action.

### Named maps

### Anonymous maps

### Map elements

## Counters

Counters record packet and byte totals for matching traffic. A counter statement increments these totals when packet processing reaches the statement.

Counters provide traffic statistics without changing the packet verdict.

### Anonymous counters

Anonymous counters belong to individual rules. No separate object declaration is required.

For example, the following rules count incoming HTTP and HTTPS traffic independently:

Each rule maintains its own packet and byte totals. The counter statement applies only to packets that reach it.

### Named counters

Named counters provide reusable stateful objects within a table. Multiple rules can reference the same counter.

For example, the following rules count HTTP and HTTPS traffic together:

Both rules increment the same packet and byte totals. The counter remains a separate object, independent of the rules that reference it.

A named counter can specify initial packet and byte totals:

The packets and bytes fields initialize the corresponding totals when the counter is created.

### Counter management

The nft command lists, resets, creates, and deletes named counters in the active kernel ruleset.

List all named counters:

`root #``nft list counters`
List counters in a table:

`root #``nft list counters table inet filter`
List an individual counter:

`root #``nft list counter inet filter web_traffic`
Reset an individual counter:

`root #``nft reset counter inet filter web_traffic`
Reset all named counters in a table:

`root #``nft reset counters table inet filter`
Reset all named counters:

`root #``nft reset counters`
Reset operations return the previous statistics and reset the counters to their initial values. Anonymous counters are not reset by named-counter reset commands.

Create a named counter:

`root #``nft add counter inet filter web_traffic`
Delete a named counter:

`root #``nft delete counter inet filter web_traffic`
Named counters must exist in the table containing the rules that reference them.

## Quotas

Quotas account for bytes processed by matching traffic. A quota statement matches according to whether the accumulated byte count is within or beyond a configured threshold.

A quota can control which rule statements execute, but does not define a packet verdict by itself. The rule must specify the desired verdict.

### Anonymous quotas

Anonymous quotas belong to individual rules. No separate object declaration is required.

The until keyword matches traffic while the quota has not exceeded its threshold. The over keyword matches traffic after the threshold has been exceeded.

For example, the following rule accepts UDP traffic to port 5060 until the quota exceeds 100 megabytes:

After the quota stops matching, packet processing continues with the next applicable rule. If no later rule matches, the chain policy determines the verdict.

The following rule drops UDP traffic to port 5060 after the quota threshold is exceeded:

Anonymous quotas cannot be referenced by name or managed independently of their rules.

### Named quotas

Named quotas provide reusable stateful objects within a table. Multiple rules can reference the same quota and contribute to its accumulated byte count.

For example, the following ruleset defines a 500-megabyte quota for HTTP traffic:

The quota statement matches HTTP traffic after the threshold is exceeded, causing the first rule to drop that traffic. HTTPS traffic does not reference the quota.

The used field specifies an initial accumulated byte count:

This declaration initializes the quota with 100 megabytes already accounted for.

A named quota can use either until or over, according to the required match condition. A quota shared by multiple rules maintains one combined byte count.

### Quota management

The nft command lists, resets, creates, and deletes named quotas in the active kernel ruleset.

List all named quotas:

`root #``nft list quotas`
List quotas in a table:

`root #``nft list quotas table inet filter`
List an individual quota:

`root #``nft list quota inet filter http_traffic`
Reset an individual quota:

`root #``nft reset quota inet filter http_traffic`
Reset all named quotas in a table:

`root #``nft reset quotas table inet filter`
Reset all named quotas:

`root #``nft reset quotas`
Reset operations return the previous usage and reset the quota to its initial state. Anonymous quotas are not reset by named-quota reset commands.

Create a named quota:

`root #``nft add quota inet filter http_traffic { over 500 mbytes` }

Delete a named quota:

`root #``nft delete quota inet filter http_traffic`
Named quotas must exist in the table containing the rules that reference them.



## Flowtables

Flowtables provide:

- flow-based forwarding for established network flows
- bypassing portions of the conventional packet-processing path
- hardware offload where supported

## Objects

Objects provide reusable state or configuration within a table.

Stateful objects maintain state across packet evaluations. Examples include:

- counters, which track packets and bytes
- quotas, which track byte usage against a threshold
- limits, which constrain packet or byte rates

Named objects have administrator-defined names and can be referenced by multiple rules within the same table.

The nft command manages objects in the active kernel ruleset.

For example, named counters and quotas allow multiple rules to share state:

Both rules update the same counter and quota. The counter tracks packet and byte totals; the quota matches traffic after its shared byte threshold is exceeded.

## NAT

## Commands

### add

### create

### delete

### list

### flush

### rename

### replace

### Examples

### See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset]
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules)
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
