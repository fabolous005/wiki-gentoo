<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Log | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Log -->
---
title: Nftables/Rules/Log
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Log
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-04"
fingerprint: e612ba18cdb078bc
license: CC BY-SA 4.0
---

# Nftables/Rules/Log

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The log statement logs the currently matched packet.

## Placement and evaluation

The log statement is non-terminal. It can precede a terminal statement, such as accept or drop. Terminal statements must be last in a rule.

Logging does not terminate rule evaluation.

## Syntax

log \[prefix "\<string>"\] \[level \<level>\] \[flags \<flags>\]
log group \<group> \[prefix "\<string>"\] \[queue-threshold \<value>\] \[snaplen \<size>\]
log level audit

## Usage

Place log inside a rule. Packet expressions select which packets are logged.

**`/etc/nftables/rules.d/33-ssh-server-log.nft`**

```
chain input {
    tcp dport 22 log prefix "SSH: " accept
}
```
## Options

| Option | Datatype | Description | 
|---|---|---|
| prefix | quoted string | Prepends a string to log messages. | 
| level \<level\_name> | emerg, alert, crit, err, warn, notice, info, or debug | Sets the syslog severity. The audit level uses a separate syntax and cannot be combined with other logging options. Default is warn. | 
| flags \<log\_flags> | See [Log Flags](https://wiki.gentoo.org#Log_Flags) table | Enables additional packet-header fields | 
| group \<nlog\_id> | unsigned 16-bit integer | Sends messages to an NFLOG group | 
| queue-threshold \<threshold> | unsigned 32-bit integer | Sets the NFLOG queue threshold | 
| snaplen \<packet\_length> | unsigned 32-bit integer | Limits the packet payload length included in NFLOG messages | 

### Log Flags

The flags option enables additional packet-header fields.

| Flag | Description | 
|---|---|
| tcp \<tcp\_flag> | Logs TCP header information. See [tcp\_flag](https://wiki.gentoo.org#tcp_flags) table. | 
| ip options | Logs IPv4 and IPv6 options | 
| skuid | Logs the originating socket UID, when available. | 
| ether | Logs Ethernet header information, including source and destination MAC addresses and EtherType. | 
| all | Enables all supported logging flags. | 

The tcp\_flag accepts the following sub-flags:


| TCP Flag | Description | 
|---|---|
| sequence | Logs the TCP sequence number. | 
| options | Logs TCP options, including MSS, window scaling, SACK, and timestamps. | 

Separate multiple TCP sub-flags with commas.


**Log TCP sequence numbers and options**

```
nft add rule inet filter input tcp log flags tcp sequence,options
```
## Logging backends

The log statement supports kernel logging, NFLOG, and audit logging.

### Kernel logging

Kernel logging writes packet information to the kernel logging subsystem. Read messages with dmesg or a system logging service.

The following rule logs and counts packets before dropping them:

log prefix "Dropped input traffic: " counter drop

### NFLOG

NFLOG sends packet log messages to a netlink group. A userspace listener, such as [net-firewall/ulogd](https://packages.gentoo.org/packages/net-firewall/ulogd), receives them.

**`/etc/nftables/rules.d/xx-log-as-firewall.nft`**

**Send firewall logs to NFLOG group 1**

```
log group 1 prefix "Firewall: "
```
### Audit

The log level audit form sends records to the Linux audit subsystem. It does not accept the other logging options.

## System logging

Kernel log messages share the kernel logging subsystem with other kernel messages.

Use a userspace logging service to filter nftables messages into separate files.

For details on userspace logging service, see [Logging](https://wiki.gentoo.org/wiki/Logging).

## Examples

Rate-limit logging, but drop all packets:

**`/etc/nftables/rules.d/66-log-dropped.nft`**

**Rate-limit logging, drop all packets**

```
limit rate 5/second log prefix "Dropped: "
drop
```
Rate-limit logging and rate-limit dropping:

**`/etc/nftables/rules.d/67-log-drop.nft`**

**Rate-limit logged and dropped packets**

```
limit rate 5/second log prefix "Dropped: " drop
```
## Kernel

The following Linux Kconfig options provide nftables logging support:

- `CONFIG_NF_TABLES` - nftables framework.
- `CONFIG_NFT_LOG` - nftables logging statement support.
- `CONFIG_NF_LOG_SYSLOG` - kernel logging backend.
- `CONFIG_NETFILTER_NETLINK_LOG` - NFLOG support through netlink.

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — the Linux packet-handling framework
- [Category:Logging](https://wiki.gentoo.org/wiki/Category:Logging)
