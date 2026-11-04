<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Log | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Log -->
---
title: Nftables/Rules/Log
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Log
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-03"
fingerprint: a412bb3ce7ac78fc
license: CC BY-SA 4.0
---

# Nftables/Rules/Log

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The log statement logs matching packets.

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

| Option | Description | 
|---|---|
| prefix | Adds a prefix to log messages. | 
| level \<level\_name> | Sets the syslog severity. Valid values: emerg, alert, crit, err, warn, notice, info, or debug. Default is warn. The audit level uses a separate syntax; cannot be combined with other logging options. | 
| flags \<log\_flags> | Enables additional packet header fields. See [log flags](https://wiki.gentoo.org#Log_Flags). | 
| group \<nlog\_id> | Sends messages to an NFLOG group. Requires a non-negative integer. | 
| queue-threshold \<threshold> | Sets the NFLOG queue threshold. Requires a positive integer. | 
| snaplen \<packet\_length> | Limits the packet payload length included in NFLOG messages. Requires a positive integer. | 

### Log Flags

The flags option selects additional packet-header fields for logging.

| Flag | Description | 
|---|---|
| `tcp` | Logs certain field in the TCP header. | 
| ip options | Logs IPv4 options present in the packet. | 
| skuid | Logs the originating socket UID, when available. | 
| ether | Logs Ethernet header fields, including source and destination MAC addresses and EtherType. | 
| all | Enables all supported logging flags. | 

Valid `tcp` flags:

| Flag | Description | 
|---|---|
| sequence | Logs the TCP sequence number, identifying the position of the packet data in the TCP byte stream | 
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

log prefix "Dropped input traffic: " counter drop



### Log action

From the example configuration above, `log prefix "Dropped input traffic: " counter drop` results in:

### syslog-ng nftables logging

nftables log messages are sent to the kernel logging subsystem. By default, these messages may be mixed with other kernel messages, making firewall events harder to isolate.

Configure [syslog-ng](https://wiki.gentoo.org/wiki/Syslog-ng) to filter nftables log messages and write them to a separate file.

**`/etc/syslog-ng/syslog-ng.conf`**

**Log traffic matching the*fib.wan* interface to /var/log/nft/wan.log**

```
filter f_nft_WAN {
    program("kernel")
    and message("IN=fib\\.wan");
};
destination d_nft_WAN {
    file("/var/log/nft/wan.log");
};
log {
    source(s_src);
    filter(f_nft_WAN);
    destination(d_nft_WAN);
};
```


## Examples

Limit logging before dropping packets:

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
## Kernel configuration

For the `log` configuration, the relevant Linux Kconfig options are:

- `CONFIG_NF_TABLES` - nftables framework
- `CONFIG_NFT_LOG` -  nftables log statement support; builds the nft\_log module


The logging backend depends on the form of the statement:

- `CONFIG_NF_LOG_SYSLOG` — kernel logging through the kernel log, readable via dmesg or a syslog service
- `CONFIG_NETFILTER_NETLINK_LOG` — NFLOG support for sending packet logs to userspace through netlink

## See also

- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — the Linux packet-handling framework
- [Category:Logging](https://wiki.gentoo.org/wiki/Category:Logging)
