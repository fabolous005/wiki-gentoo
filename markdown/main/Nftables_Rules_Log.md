<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Log | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Log -->
---
title: Nftables/Rules/Log
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Log
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: a698abacf7b8795c
license: CC BY-SA 4.0
---

# Nftables/Rules/Log

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The log statement logs matching packets.

## Evaluation

The log statement is non-terminal. Rule evaluation continues after logging.

## Placement

The log statement can precede a terminal statement in the same rule. A terminal statement cannot precede another statement in that rule.

## Syntax

log \[prefix "\<string>"\] \[level \<level>\] \[flags \<flags>\]
log group \<group> \[prefix "\<string>"\] \[queue-threshold \<value>\] \[snaplen \<size>\]
log level audit

## Usage

The log statement belongs inside a rule in a chain. Packet expressions can restrict which packets are logged.

**`/etc/nftables/rules.d/33-ssh-server-log.nft`**

```
chain input {
    tcp dport 22 log prefix "SSH: " accept
}
```
The log statement can precede a terminal statement, such as accept or drop. Terminal statements cannot precede another statement in the same rule.

## Options

| Option | Description | 
|---|---|
| prefix | Prepends a string to log messages. | 
| level | Sets the syslog severity. | 
| flags | Enables additional packet information. | 
| group | Sends packet logs to an NFLOG group. | 
| queue-threshold | Sets the NFLOG queue threshold. | 
| snaplen | Limits the packet payload length included in an NFLOG message. | 

The default syslog level is warn. Valid levels are emerg, alert, crit, err, warn, notice, info, and debug.

Valid flags include tcp sequence, tcp options, ip options, skuid, ether, and all.

The level audit form does not accept the other logging options.

## Output

The log statement supports three output modes:

- **Kernel log:** Writes packet information to the kernel logging facility. Read messages with dmesg or a system logging service.
- **NFLOG:** Sends packet log messages to a netlink group. A userspace listener, such as [net-firewall/ulogd](https://packages.gentoo.org/packages/net-firewall/ulogd), receives the messages.
- **Audit:** Writes audit records for services such as [sys-process/audit](https://packages.gentoo.org/packages/sys-process/audit).

The selected mode determines the output destination and available options.

## Examples

Log SSH traffic before accepting it:

**`/etc/nftables/rules.d/33-log-incoming-ssh.nft`**

**Log incoming SSH sessions**

```
tcp dport 22 log prefix "SSH: " accept
```
Rate-limit logging before dropping packets:

**`/etc/nftables/rules.d/66-log-dropped.nft`**

**"Log any dropped packets over 5 per seconds"**

```
limit rate 5/second log prefix "Dropped: " drop
```
Send packet logs to NFLOG group 1:

**`"/etc/nftables/rules.d/xx-log-as-firewall.nft`**

**"Mark this log as 'Firewall: '**

```
log group 1 prefix "Firewall: "
```
## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and perform actions.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — a Linux packet filtering framework for the Netfilter subsystem, providing a unified interface for configuring packet filtering, connection tracking, NAT, and related network functionality.
- [Category:Logging](https://wiki.gentoo.org/wiki/Category:Logging)
