<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Include | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Include -->
---
title: Nftables/Rules/Include
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Include
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-28"
fingerprint: "271e226e666d6d6d"
license: CC BY-SA 4.0
---

# Nftables/Rules/Include

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The `include` statement reads ruleset content from another file. It belongs to the **common\_block** syntax group.

### Placement

Place `include` wherever **common\_block** syntax is valid.

**common\_block** can be inserted in any non-block lines and inside all blocks (surrounded by a pair of block delimiters (`{` ... `}`).

### Evaluation

The `include` statement processes the specified file in place.

Relative paths are resolved against the process working directory ($CWD), not the including file's directory.

Wildcard (glob) patterns (`*`, `?`, and `[]`) match multiple files in lexical order.

### Syntax

**`/etc/nftables/rules/main.nft`**

**Example include statement**

```
include "/etc/nftables/rules/filter.nft"
```
Filepath are always surrounded by a pair of single-quote (`'`...`'`) or double-quote (`"`...`"`).

Use backslash to escape any inside quote; example `"abcdefg\"hijk"`.

### Examples

Include all .nft files in a directory:

**`/etc/nftables/rules/main.nft`**

```
include "/etc/nftables/rules.d/*.nft"
```
Notice the glob `*` in its filepath.

### Tips

- The \`include\` statement is processed by nft, not by the kernel.
- Use absolute paths to avoid working-directory dependencies, unless it is for a {\[c|nft}} test-harness.
- Order included files so that required declarations precede their references.
- Its lexical order follows C locale (defined by `LANG` (or `LC_CTYPE`) environment variable); typically `0-9AaBbCc ... Zz` order.



## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Modular rulesets](https://wiki.gentoo.org/wiki/Nftables/Configuration#Modular_Ruleset)
