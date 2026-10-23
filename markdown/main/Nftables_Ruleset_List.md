<!-- source: https://wiki.gentoo.org/wiki/Nftables/Ruleset/List | group: Gentoo Wiki (Main) | wiki-title: Nftables/Ruleset/List -->
---
title: Nftables/Ruleset/List
url: https://wiki.gentoo.org/wiki/Nftables/Ruleset/List
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-22"
fingerprint: "45f9dc9e0b932dcd"
license: CC BY-SA 4.0
---

# Nftables/Ruleset/List

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The `list` command displays the ruleset or specified objects and object classes in the Netfilter **nf\_tables** kernel subsystem.

## Placement

The `list` keyword begins a command. Place the command at the [nft](https://wiki.gentoo.org/wiki/Nft) command line or in a ruleset file.



## Evaluation

A `list` can request an object or an object class.

An object is a component in the Netfilter **nf\_tables** kernel tables.


`list` can only request one object class at a time.

## Syntax

list ruleset  \[FAMILY\]
list table  \[FAMILY\]  TABLE\_ID
list tables  \[FAMILY\]
list chain  \[FAMILY\]  TABLE\_ID CHAIN\_ID
list chains  \[FAMILY\]
list meter  \[FAMILY\]  TABLE\_ID SET\_ID
list meters  \[FAMILY\]
list map  \[FAMILY\]  TABLE\_ID SET\_ID
list maps  \[FAMILY\]
list set  \[FAMILY\]  TABLE\_ID SET\_ID
list sets  \[FAMILY\]
list sets table  \[FAMILY\]  TABLE\_ID
list counter  \[FAMILY\] TABLE\_ID COUNTER\_ID
list counters  \[FAMILY\]
list counters table  \[FAMILY\]  TABLE\_ID
list ct expectation table  \[FAMILY\]  TABLE\_ID
list ct expectation  \[FAMILY\]  TABLE\_ID  CT\_ID
list ct expectations
list ct helper  \[FAMILY\]  TABLE\_ID  CT\_ID
list ct helpers table  \[FAMILY\]  TABLE\_ID
list ct timeout table  \[FAMILY\]  TABLE\_ID
list ct timeout  \[FAMILY\]  TABLE\_ID  CT\_ID
list flow table  \[FAMILY\]  TABLE\_ID  FLOW\_ID
list flow tables  \[FAMILY\]
list flowtable  \[FAMILY\]  TABLE\_ID  FLOW\_ID
list flowtables  \[FAMILY\]
list hooks  \[FAMILY\]
list hooks netdev  \[device DEVICE\_NAME\]
list limit  \[FAMILY\]  LIMIT\_ID
list limits  \[FAMILY\]
list limits table  \[FAMILY\]  TABLE\_ID
list quota  \[FAMILY\]  QUOTA\_ID
list quotas  \[FAMILY\]
list quotas table  \[FAMILY\]  TABLE\_ID
list secmark  \[FAMILY\]  SECMARK\_ID
list secmarks  \[FAMILY\]
list secmarks table  \[FAMILY\]  TABLE\_ID
list synproxy  \[FAMILY\]  SYNPROXY\_ID
list synproxys  \[FAMILY\]
list synproxys  table \[FAMILY\]  TABLE\_ID

where:

- `FAMILY` - an ip, ip6, inet, netdev, bridge, arp, or xfrm option
- `TABLE_ID` - name of [**table**](https://wiki.gentoo.org/wiki/Nftables/Ruleset#tables) block.
- `CHAIN_ID` - name of [**chain**](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) block.
- `COUNTER` - name of **counter** block.
- `CT_ID` - name of **connection tracking** block.
- `FLOW_ID` - name of **flow** block.
- `LIMIT_ID` - name of [**limit**](https://wiki.gentoo.org/wiki/Nftables/Rules/Limit) block.
- `MAP_ID` - name of **map** block.
- `SET_ID` - name of **set** block.
- `QUOTA_ID` - name of **quota** block.
- `SECMARK_ID` - name of **secmark** block.
- `SYNPROXY_ID` - name of **synproxy** block.
- `DEVICE_NAME` - name of interface device.





## Examples

### List ruleset by command line

To list the entire ruleset by command line:

`root #``nft list ruleset`
### List ruleset by ruleset file

To list the entire ruleset by a ruleset file, create a file:

FILE **`list.nft`****List entire Nftables ruleset**

```
list ruleset
```
then execute ruleset file:

`root #``nft -f list.nft`
## Tips

- Use list ruleset to display all tables and their contents
- Use the appropriate object-class command to restrict output
- Use a separate command for each object class that must be listed individually

## See also

- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — the collection of tables, chains, rules, and other configured objects used to define [nftables](https://wiki.gentoo.org/wiki/Nftables) packet handling.
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
