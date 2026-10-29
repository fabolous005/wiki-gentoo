<!-- source: https://wiki.gentoo.org/wiki/Nftables/Rules/Log | group: Gentoo Wiki (Main) | wiki-title: Nftables/Rules/Log -->
---
title: Nftables/Rules/Log
url: https://wiki.gentoo.org/wiki/Nftables/Rules/Log
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-28"
fingerprint: a412ab3cc7a87974
license: CC BY-SA 4.0
---

# Nftables/Rules/Log

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The log statement logs matching packets.

## Placement

The log statement can precede a terminal statement in the same rule. A terminal statement cannot precede another statement in that rule.

## Evaluation

The log statement is non-terminal. Rule evaluation continues after logging.

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
| level \<level\_name> | Sets the syslog severity.  Valid \<level\_name> are emerg, alert, crit, err, warn, notice, info, and debug.  The default syslog level is warn. The level audit form does not accept the other logging options. | 
| flags \<log\_flags> | Enables additional packet information. See [log flags](https://wiki.gentoo.org#log_flags_options) for \<log\_flags> detail. | 
| group \<nlog\_id> | Sends packet logs to an NFLOG group. \<nlog\_id> requires an non-negative integer value. | 
| queue-threshold \<threshold> | Sets the NFLOG queue threshold. \<threshold> requires a positive integer value. | 
| snaplen \<packet\_length> | Limits the packet payload length included in an NFLOG message. \<packet\_length> requires a positive integer value. | 

### Log Flags Option

The \`log\_flags\` option controls which additional packet-header fields are included in nftables log messages.

| Flag | Description | 
|---|---|
| tcp | Logs certain field in the TCP header. | 
| ip options | Logs IPv4 options present in the packet. | 
| skuid | Logs the UID associated with the originating socket, when available. Socket UID information may also be unavailable. | 
| ether | Logs Ethernet header information, including source and destination MAC addresses and the EtherType, when available. | 
| all | Enables all supported logging flags. | 

Valid \<tcp\_flag>s:

| Flag | Description | 
|---|---|
| sequence | Logs the TCP sequence number, identifying the position of the packet data in the TCP byte stream | 
| options | Logs TCP options carried in the TCP header, such as Maximum Segment Size (MSS), Window Scale, Selective Acknowledgment (SACK), and timestamps | 

If using multiple TCP flags, each flag must be separated by a comma.


**Log TCP sequence numbers and options**

```
nft add rule inet filter input tcp log flags tcp sequence,options
```
## Output

The log statement supports three output modes:

- **Kernel log:** Writes packet information to the kernel logging facility. Read messages with dmesg or a system logging service.
- **NFLOG:** Sends packet log messages to a netlink group. A userspace listener, such as [net-firewall/ulogd](https://packages.gentoo.org/packages/net-firewall/ulogd), receives the messages.
- **log level audit:** Writes audit records for services such as [sys-process/audit](https://packages.gentoo.org/packages/sys-process/audit).

The selected mode determines the output destination and available options.

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


{{Important: This assumes s\_src is defined in the existing syslog-ng configuration and receives kernel messages. The original program(kernel) filter and interface-name match should be checked against the actual format emitted by the nftables log statement and the local syslog-ng version.}}

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
## Kernel

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
