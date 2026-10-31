<!-- source: https://wiki.gentoo.org/wiki/Nftables/Ruleset/Flush | group: Gentoo Wiki (Main) | wiki-title: Nftables/Ruleset/Flush -->
---
title: Nftables/Ruleset/Flush
url: https://wiki.gentoo.org/wiki/Nftables/Ruleset/Flush
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-30"
fingerprint: "5c11f71ba9c7a76f"
license: CC BY-SA 4.0
---

# Nftables/Ruleset/Flush

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **flush** command removes all rules from chains, all elements from sets and maps, or all tables and their contents from the Netfilter **nf\_tables** kernel ruleset.

## Placement

The `flush` keyword begins a command.

Place the `flush` command at the [nft](https://wiki.gentoo.org/wiki/Nft) command line or in a ruleset file.

## Evaluation

The `flush` command removes the contents of the specified object or object class.

The command can flush a **ruleset**, table, chain, set, map, or object class, depending on the specified syntax.

Flushing a table or ruleset removes its contained rules and elements without necessarily deleting the table or other objects themselves.

The `flush chain` command removes all rules from the specified chain.

The `flush set` and `flush map` commands remove the elements from the specified set or map.

## Syntax

flush ruleset    \[FAMILY\]
flush table      \[FAMILY\] TABLE\_ID
flush chain      \[FAMILY\] TABLE\_ID CHAIN\_ID
flush set        \[FAMILY\] TABLE\_ID SET\_ID
flush map        \[FAMILY\] TABLE\_ID MAP\_ID
flush meter      \[FAMILY\] TABLE\_ID METER\_ID
flush flow table \[FAMILY\] TABLE\_ID FLOW\_ID

where:

- FAMILY - address family.
- TABLE\_ID - name of the [**table**](https://wiki.gentoo.org/wiki/Nftables/Ruleset#tables) block.
- CHAIN\_ID - name of the [**chain**](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) block.
- SET\_ID - name of the **set** block.
- MAP\_ID - name of the **map** block.
- METER\_ID - name of the **meter** block.
- FLOW\_ID - name of the **flow table** block.

## Examples

### Flush a chain

To remove all rules from the input chain of the IPv4 filter table:

`root #``nft flush chain ip filter input`
The chain remains in the ruleset.

### Flush a set

To remove all elements from the trusted\_ips set in the IPv4 filter table:

`root #``nft flush set ip filter trusted_ips`
The set definition remains in the ruleset.

### Flush a table

To flush the IPv4 filter table:

`root #``nft flush table ip filter`
The table remains in the ruleset.

### Flush the ruleset

To flush the entire ruleset:

`root #``nft flush ruleset`
## Tips

- Flushing an object removes its contents; it does not necessarily delete the object.
- Use the [delete](https://wiki.gentoo.org/index.php?title=Nftables/Ruleset/Delete&action=edit&redlink=1) command to remove an object from the ruleset.
- Flushing the ruleset removes the configured tables and rules' contents from the kernel according to the command's defined semantics; verify the exact effects before using it on a live firewall.
- Use nft -c -f FILE to check a ruleset file before applying its commands.

## See also

- [Nftables/Ruleset/Add](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Add)
- [Nftables/Ruleset/List](https://wiki.gentoo.org/wiki/Nftables/Ruleset/List)
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem.
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
