<!-- source: https://wiki.gentoo.org/wiki/Nft | group: Gentoo Wiki (Main) | wiki-title: Nft -->
---
title: Nft
url: https://wiki.gentoo.org/wiki/Nft
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-08"
fingerprint: b51ff559c705837f
license: CC BY-SA 4.0
---

# Nft

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**nft** configures and inspects the Linux kernel's nftables packet handling framework.  **nft** is the userspace command-line utility for administering nftables through Netlink.

## Introduction

**nft** is used to check, load, modify, and inspect nftables rulesets.

**nft** reads nftables from the command line, a file, or standard input.

## Usage

To install **nft**, see [Nftables installation](https://wiki.gentoo.org/wiki/Nftables#Installation).
See [Nftables/Configuration service](https://wiki.gentoo.org/wiki/Nftables/Configuration#Service) for Gentoo service configuration (OpenRC/systemd/sysvinit).

### Invocation

`user $``nft [ options ] [ commands ]`
Most commonly used options are:

- `-f file`
- Read nftables commands from the specified file and apply them to the kernel through Netlink.
- `-c -f file`
- From a specified file, check the validity of nftables commands without applying the changes.
- `-c -f -`
- From standard input, check the validity of nftables commands without applying the changes.
- `-c nftables_command`
- From the command line, check the validity of specified nftables command without applying the changes.


Execute man nft for more detailed options.

### Files

| File | Description | 
|---|---|
| (any file) | -f accepts any readable file containing nftables commands. See [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration) for the Gentoo service configuration and its actual filename. | 
| $HOME/.nft.history | interactive command history, persistent across nft sessions | 



#### Safely updating ruleset

When working on a ruleset stored in a file, check it before loading it into the kernel.

Check the ruleset before applying it:

`root #``nft -c -f file`
Load the checked ruleset:

`root #``nft -f file`
For persistent ruleset administration, see for your specific service used in
[Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration).

### Rulesets

List the complete ruleset:

`root #``nft list ruleset`
Load a ruleset from a file:

`root #``nft -f file`
Check a ruleset file without applying it:

`root #``nft -c -f file`
#### Tables

List tables:

`root #``nft list tables`
Create a table:

`root #``nft add table inet filter`
List a table:

`root #``nft list table inet filter`
Delete a table:

`root #``nft delete table inet filter`
#### Chains

List chains:

`root #``nft list chains`
Create a base chain:

`root #``nft add chain inet filter input '{ type filter hook input priority filter; policy drop; }'`
List a chain:

`root #``nft list chain inet filter input`
#### Rules

Add a rule:

`root #``nft add rule inet filter input tcp dport 22 accept`
List rules and their handles:

`root #``nft -a list chain inet filter input`
Delete a rule by handle:

`root #``nft delete rule inet filter input handle 5`
#### Sets

List sets:

`root #``nft list sets`
Create a set:

`root #``nft add set inet filter trusted '{ type ipv4_addr; }'`
Sets can be referenced by rules to match multiple values without duplicating rules.

#### Monitoring

Monitor nftables events:

`root #``nft monitor`
Monitor ruleset changes:

`root #``nft monitor rules`
#### JSON

`root #``nft -j list ruleset`
JSON output is useful when nftables output needs to be processed programmatically.

## Caveats

### Ruleset changes are immediate

Commands which modify the ruleset affect the running kernel immediately. They are not merely changes to a configuration file.

### Flushing the ruleset

`root #``nft flush ruleset`
removes all tables, chains, rules, sets, and other objects in the current ruleset.

### Rule handles

Rule handles are assigned by the kernel and can be used to identify individual rules when deleting or replacing them.

Handles are not intended to be persistent identifiers.

## Tips

### Save the current ruleset

The active ruleset can be written to a file:

`root #``nft list ruleset > file`
The resulting file can be loaded with:

`root #``nft -f file`
### Numeric output

Use -n when output should not contain name lookups:

`root #``nft -n list ruleset`
### Show rule handles

Use -a when a rule may subsequently need to be deleted or replaced:

`root #``nft -a list ruleset`
### Exit code

**nft** returns a non-zero exit code when an operation fails. A zero exit code indicates that the requested operation completed successfully.

The exit code can be tested from the shell:

`root #``nft -c -f file ; echo $?`
For example:

`root #``nft -c -f file && echo "ruleset check passed"`
Exit codes used by **nft** as observed in its source code.

| Value | Description | 
|---|---|
| 1 | NFT\_EXIT\_FAILURE / EXIT\_FAILURE — general command failure. Both symbolic constants resolve to the same value. | 
| 2 | YY\_EXIT\_FAILURE — parser failure status, originating from the Bison-generated parser. | 
| 3 | NFT\_EXIT\_NONL — indicates that output was requested without a trailing newline. | 
| 111 | Returned by **nft** for an internal/runtime failure where the source explicitly calls exit(111). | 



## Troubleshooting

### netlink: Error: cache initialization failed: Operation not permitted.

nft is a root-privileged operation and requires root-privilege. Use su - or sudo nft or login in as root.

### nft cannot access the kernel ruleset

Check whether the kernel's nftables support is available:

`root #``nft list tables`
Verify that nf\_tables and any required Netfilter modules are enabled in the running kernel.

### A ruleset does not load

Check the ruleset before loading into the nf\_tables via Netlink:

`root #``nft -c -f file`
The diagnostic output identifies errors detected during checking.

If checking succeeds but loading fails, inspect the diagnostic output from:

`root #``nft -f file`
### A rule cannot be deleted

Display the rule handles:

`root #``nft -a list chain inet filter input`
Then specify the required handle:

`root #``nft delete rule inet filter input handle HANDLE`
## See also

- [nft] — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules)
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
- [Security Handbook](https://wiki.gentoo.org/wiki/Security_Handbook) — valuable guidance on Gentoo Linux security and cybersecurity in general.

## External resources

- [nftables](https://www.netfilter.org/projects/nftables/) – The upstream nftables project.
- [nft(8)](https://www.netfilter.org/projects/nftables/manpage.html) – The upstream manual page.
- [nftables wiki](https://wiki.nftables.org/) – Upstream documentation and examples.
