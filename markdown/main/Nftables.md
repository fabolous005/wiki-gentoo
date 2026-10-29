<!-- source: https://wiki.gentoo.org/wiki/Nftables | group: Gentoo Wiki (Main) | wiki-title: Nftables -->
---
title: nftables
url: https://wiki.gentoo.org/wiki/Nftables
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-28"
fingerprint: b70b7b57c3822b00
license: CC BY-SA 4.0
---

# nftables

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**nftables** is the Linux packet-handling framework.

It replaces the older [iptables](https://wiki.gentoo.org/wiki/Iptables), ip6tables, arptables, and ebtables interfaces.



## Introduction

**nftables** is the Linux packet-handling framework for configuring and processing network traffic through the Netfilter kernel infrastructure.

nftables provides the [nft](https://wiki.gentoo.org/wiki/Nft) userspace command for configuring its ruleset.

The nftables ruleset defines tables, chains, rules, and other objects used by Netfilter for packet processing.

nftables replaces the older iptables, ip6tables, arptables, and ebtables interfaces.

## Installation

### Kernel

The nftables requires the Linux kernel's Netfilter and nf\_tables support.

Kernel settings here come in two methods:

- User Interface menu (make menuconfig) or
- File-based config (\*.config) files

#### Menu-based settings

Alternatively, one of the following user interface may be used instead.

`user $``make nconfig  # ncursed-based`
or

`user $``make menuconfig  # raw ANSI terminal`
or

`user $``make xconfig  # Xwindows`
##### Basic Netfilter (firewalling with NAT)

*Example Config*: A **bare minimum** for basic IPv4 firewalling with NAT:

**Nftables IP kernel requirements**

\[\*\] Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.
     Networking options  --->
     ...
           \<\*> Packet socket [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PACKET\</code> to find this item.
           \<\*>   Packet: sockets monitoring interface
           \[\*\] Network packet filtering framework (Netfilter) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\</code> to find this item.  --->
           Core Netfilter Configuration  --->
               \<M> Netfilter connection tracking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_CONNTRACK\</code> to find this item.
               \<M> Syslog packet logging [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_LOG\_SYSLOG\</code> to find this item.
               \<M> Connection tracking netlink interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_CT\_NETLINK\</code> to find this item.
               \<M> Network Address Translation support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_NAT\</code> to find this item.
               \<M> Netfilter nf\_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\</code> to find this item.
               \<M>   Netfilter nf\_tables mixed IPv4/IPv6 tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\_INET\</code> to find this item.
               \<M>   Netfilter nf\_tables conntrack module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_CT\</code> to find this item.
               \<M>   Netfilter nf\_tables log module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_LOG\</code> to find this item.
               \<M>   Netfilter nf\_tables limit module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_LIMIT\</code> to find this item.
           IP: Netfilter Configuration  --->
               \<M> IPv4 socket lookup support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_SOCKET\_IPV4\</code> to find this item.
               \<M> IPv4 nf\_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\_IPV4\</code> to find this item.
               \<M> IPv4 packet rejection [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_REJECT\_IPV4\</code> to find this item.
               \<M> IP tables support (required for filtering/masq/NAT) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_IPTABLES\</code> to find this item.
               \<M>   Packet filtering [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_FILTER\</code> to find this item.
               \<M>     REJECT target support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_TARGET\_REJECT\</code> to find this item.
               \<M>   iptables NAT support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_NAT\</code> to find this item.
               \<M>     MASQUERADE target support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_TARGET\_MASQUERADE\</code> to find this item.


*Additional Common Config:* For **mixed IPv4 and IPv6 rules** combined into one table: *CONFIG\_NF\_TABLES\_INET*

##### inet family

If family *inet* is not enabled, only families *ip* and *ip6* can be used individually.

**Nftables inet family**

\[\*\] Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.
     Networking options  --->
     ...
           \[\*\] Network packet filtering framework (Netfilter) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\</code> to find this item.  --->
           Core Netfilter Configuration  --->
               \<M> Netfilter nf\_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\</code> to find this item.
               \<M>   Netfilter nf\_tables mixed IPv4/IPv6 tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\_INET\</code> to find this item.

##### Netdev support

Optionally, early filtering based on network device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\_NETDEV\</code> to find this item. requires netdev tables support.

To filter by interface, add in Netdev-related settings:

**Nftables netdev family**

\[\*\] Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.
     Networking options  --->
     ...
           \[\*\] Network packet filtering framework (Netfilter) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\</code> to find this item.  --->
           Core Netfilter Configuration  --->
               \<M> Netfilter nf\_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\</code> to find this item.
               \[\*\]   Netfilter nf\_tables netdev tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\_NETDEV\</code> to find this item.

#### File-based settings

Merge/append file-based settings into kernel config (.config) file.

##### Basic IPv4/IPv6 filtering with NAT

For basic workstation with NAT IPv4/IPv6 and Netfilter, use:

**`nftables-ipv4-ipv6.config`**

**Basic workstation with IPv4/IPv6 filtering with NAT**

```
CONFIG_NET=y
CONFIG_INET=y
# Networking options
CONFIG_PACKET=y
CONFIG_NETLINK_DIAG=y
# Network packet filtering framework
CONFIG_NETFILTER=y
# Core Netfilter Configuration
CONFIG_NF_CONNTRACK=m
CONFIG_NF_CT_NETLINK=m
CONFIG_NF_NAT=m
CONFIG_NF_TABLES=m
CONFIG_NF_TABLES_INET=m
# nftables expressions needed for ordinary rules
CONFIG_NFT_CT=m
CONFIG_NFT_LOG=m
CONFIG_NFT_LIMIT=m
# disable legacy iptables
CONFIG_NETFILTER_XTABLES=n
CONFIG_IP_NF_IPTABLES=n
```
##### Basic INET filtering with NAT

For basic workstation with NAT INET & IPv4/IPv6 and Netfilter, apply:

**`nftables-inet.config`**

**Basic workstation with INET/IPv4/IPv6 and NAT filtering**

```
CONFIG_NET=y
CONFIG_INET=y
CONFIG_NETFILTER=y
CONFIG_NF_TABLES=m
CONFIG_NF_TABLES_INET=m
CONFIG_NF_CONNTRACK=m
CONFIG_NF_CT_NETLINK=m
CONFIG_NF_NAT=m
CONFIG_NFT_CT=m
```
##### Basic NETDEV filtering with NAT

For basic workstation with NETDEV and NAT filtering, apply:

**`nftables-netdev.config`**

**Basic workstation with NETDEV and NAT filtering**

```
CONFIG_NET=y
CONFIG_INET=y
CONFIG_NETFILTER=y
CONFIG_NF_TABLES=m
CONFIG_NF_TABLES_INET=m
CONFIG_NFT_TABLES_NETDEV=m
```
##### Merging file settings

In a Linux source main directory (cd /usr/src/linux), merge the

- .config files into kernel .config file.

`user $``./scripts/kconfig/merge_config.sh  .config  nftables-ipv4-ipv6.config`
The base file './config' does not exist. Creating one...
Using ./config as base
Merging ../doc/nftables-ipv4-ipv6.kconfig
./.tmp.config.Hxee7Svi4A:18:warning: symbol value 'm' invalid for NF\_TABLES\_INET
#
# configuration written to .config
#
Value requested for CONFIG\_NF\_CONNTRACK not in final .config
Requested value:  CONFIG\_NF\_CONNTRACK=m
Actual value:     CONFIG\_NF\_CONNTRACK=y
Value requested for CONFIG\_NF\_CT\_NETLINK not in final .config
Requested value:  CONFIG\_NF\_CT\_NETLINK=m
Actual value:     CONFIG\_NF\_CT\_NETLINK=y
Value requested for CONFIG\_NF\_NAT not in final .config
Requested value:  CONFIG\_NF\_NAT=m
Actual value:     CONFIG\_NF\_NAT=y
Value requested for CONFIG\_NF\_TABLES not in final .config
Requested value:  CONFIG\_NF\_TABLES=m
Actual value:     CONFIG\_NF\_TABLES=y
Value requested for CONFIG\_NF\_TABLES\_INET not in final .config
Requested value:  CONFIG\_NF\_TABLES\_INET=y
Actual value:     CONFIG\_NF\_TABLES\_INET=y
Value requested for CONFIG\_NFT\_CT not in final .config
Requested value:  CONFIG\_NFT\_CT=m
Actual value:     CONFIG\_NFT\_CT=y
Value requested for CONFIG\_NFT\_LOG not in final .config
Requested value:  CONFIG\_NFT\_LOG=m
Actual value:     CONFIG\_NFT\_LOG=y
Value requested for CONFIG\_NFT\_LIMIT not in final .config
Requested value:  CONFIG\_NFT\_LIMIT=m
Actual value:     CONFIG\_NFT\_LIMIT=y
Value requested for CONFIG\_NETFILTER\_XTABLES not in final .config
Requested value:  CONFIG\_NETFILTER\_XTABLES=n
Actual value:     # CONFIG\_NETFILTER\_XTABLES is not set
Value requested for CONFIG\_IP\_NF\_IPTABLES not in final .config
Requested value:  CONFIG\_IP\_NF\_IPTABLES=n
Actual value:     # CONFIG\_IP\_NF\_IPTABLES is not set

`user $``./scripts/kconfig/merge_config.sh  .config  nftables-inet.config``user $``./scripts/kconfig/merge_config.sh  .config  nftables-netdev.config`
Then run the latest .config against the new settings for a more complete .config.

This step brings any new dependencies into the CONFIG\_\* settings current and ready to build.

`user $``make oldconfig`
#
# No change to .config
#

The /usr/src/linux/.config is now ready for Linux kernel build.

### USE flags


### USE flags for
            [net-firewall/nftables](https://packages.gentoo.org/packages/net-firewall/nftables)
            
            Linux kernel firewall, NAT and packet mangling tools

| [+gmp](https://packages.gentoo.org/useflags/+gmp) | Add support for dev-libs/gmp (GNU MP library) | 
| [+readline](https://packages.gentoo.org/useflags/+readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Create man pages for the package (requires app-text/asciidoc) | 
| [json](https://packages.gentoo.org/useflags/json) | Enable JSON support via dev-libs/jansson | 
| [libedit](https://packages.gentoo.org/useflags/libedit) | Use the libedit library (replacement for readline) | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xtables](https://packages.gentoo.org/useflags/xtables) | Add libxtables support to try to automatically translate rules added by iptables-compat | 

The package currently provides the following local USE flags:

- **doc** — install package documentation.
- **json** — enable JSON support.
- **xtables** — enable libxtables support for compatibility with iptables tools.


[net-firewall/nftables](https://packages.gentoo.org/packages/net-firewall/nftables) does not require the legacy iptables filtering framework for normal nftables use.

### Emerge

`root #``emerge --ask net-firewall/nftables`
Verify the installation:

`root #``nft --version`
Optionally perform the master checklist [here](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables#Master_Checklist).

### Additional software

Some useful packages that use [Netfilter](https://wiki.gentoo.org/wiki/Netfilter):

- [net-firewall/conntrack-tools](https://packages.gentoo.org/packages/net-firewall/conntrack-tools) - very useful
- [net-analyzer/tcpdump](https://packages.gentoo.org/packages/net-analyzer/tcpdump) - CLI packet displaying
- [net-analyzer/wireshark](https://packages.gentoo.org/packages/net-analyzer/wireshark) - Qt-based

## Configuration

### Environment variables

nftables does not use shell environment variables.

nft provides its own variables.

The define command creates nft variables.

Shell environment variable can be passed as nft variables using nft -D CLI option.

`root #``export WAN_INTERFACE_SHELL_ENVVAR=wlan0``root #``nft -D NFT_WAN_INTERFACE=$WAN_INTERFACE_SHELL_ENVAR /etc/nftables/rules/main.nft`
### Files

| File | Description | 
|---|---|
| /etc/nftables | Directory for nft ruleset files maintained by the administrator. | 
| /etc/nftables/rules/main.nft | Ruleset maintained by administrator. Not directly loaded at bootup. Test and load the ruleset, then do /etc/init.d/nftables save to persist it. Any filename and location may be used. rules/main.nft is used on this page as the administrator's ruleset. | 
| /proc/sys/net/netfilter/\* | Kernel parameters and statistics for Netfilter. See [kernel.org](https://docs.kernel.org/networking/nf_conntrack-sysctl.html) for details. | 

For more specific files by OpenRC or systemd, see [Nftables/Configuration files](https://wiki.gentoo.org/wiki/Nftables/Configuration#Files).

### Service

The service is responsible for loading the configured ruleset and making it persistent across reboots.

Add ruleset into /etc/nftables/rules/main.nft for either OpenRC or systemd.

#### OpenRC

The init script, /etc/init.d/nftables, supports the following actions:

- **save** - Stores the currently loaded ruleset in the location defined by `NFTABLES_SAVE`, default: /var/lib/nftables/rules-save.
- **reload** - Loads the currently loaded ruleset from `NFTABLES_SAVE`.
- **stop** - Intended to be called on system shutdown. If `SAVE_ON_STOP` is enabled, saves the ruleset.
- **start** - Intended to be called on system boot, loads the ruleset from `NFTABLES_SAVE`.
- **clear** - Flushes the currently loaded ruleset, equivalent to nft flush ruleset.
- **list** - lists the currently loaded ruleset.

Nftables can be started at boot with:

`root #``rc-update add nftables default`
#### systemd

When using systemd, nftables can be administered in using one of two models:

| Model | Unit | Description | 
|---|---|---|
| **Direct File** | nftables.service | Loads the administrator-maintained ruleset during startup. The ruleset file is the source of truth for the configuration. Only an administrator can change this file. | 
| **Saved state** | nftables-load.service nftables-store.service | Loads the saved ruleset during startup and optionally saves the active kernel nf\_tables ruleset at shutdown. | 

The canonical configuration model uses the administrator-maintained ruleset as the source of truth.

The saved-state model allows the active kernel nf\_tables ruleset to be modified incrementally and saved for restoration at the next startup.

Further systemd details of both **Nftables** models are in [Nftables systemd configuration service](https://wiki.gentoo.org/wiki/Nftables/Configuration#systemd).

## Usage

See [nft](https://wiki.gentoo.org/wiki/Nft) for Netfilter usage.

## Examples

See the [Nftables examples article](https://wiki.gentoo.org/wiki/Nftables/Examples).

## Troubleshooting

### Master Checklist

The master checklist of proper descoping Netfilter problems are:

`root #``zgrep NF_TABLES /proc/config.gz`
or

`root #``grep NF_TABLES /boot/config-$(uname -r)`
Check for CONFIG\_NF\_TABLES first. Then check the kernel configuration for the address-family and Netfilter features required by the ruleset.

Check that Netlink (Netfilter API) is working:

`root #``nft list tables`
Check that nf\_tables is accessible:

`root #``nft list ruleset`
Check that associated tables to nf\_tables are accessible:

`root #``nft -a list ruleset`
Check that nftables command file is syntaxically correct:

`root #``nft -c -f /path/to/file`
Check that command file is loadable:

`root #``nft -f /path/to/file`
For systemd, check init system for nftables status:

`root #``systemctl status nftables``root #``journalctl -u nftables``root #``systemctl cat nftables`
For OpenRC, check init system for nftables status:

`root #``rc-service nftables status`
### Check the active ruleset

`root #``nft list ruleset`
If rule handles are needed:

`root #``nft -a list ruleset`
### Validate a configuration

`root #``nft -c -f ruleset-file`
This checks the configuration without applying it.

### Check kernel support

If an address family or feature is unavailable, verify that the corresponding kernel support is enabled.

For example, an inet table requires support for mixed IPv4/IPv6 tables.

### No such file or directory

If this error is printed for every chain of a table definition make sure, that the table's family is available through the kernel. This happens for example if the table uses family *inet* and the kernel configuration did not enable mixed IPv4 and IPv6 rules (CONFIG\_NF\_TABLES\_INET).

### Conflicting intervals

A set definition of IP ranges causes this error if ranges overlap. For example 224.0.0.0/3 and 240.0.0.0/5 overlap completely. Either add *auto-merge* to the set's options, drop the range that is fully included or change syntax to 224.0.0.0-255.255.255.255.

### Family netdev and ingress hook

Broken packets should be rejected early which requires an ingress hook for family netdev. This sets up a chain that acts for a dedicated network device before packets enter further processing – improved performance. The configuration looks like this:

Mind the device name enp4s0. If this changes for example when changing hardware or an upgrade changed device naming this family is broken. In turn none of the rules will be loaded. The error looks like this (filename and line numbers differ depending on the host configuration):

Check that the device name is actually correct and exists, e.g. ip addr list.

### Connections blocked after nftables restart or reboot

The default configuration of the save and restore functions uses numeric mode to store the rule set. The persisted rule set could have changed from the original upload from a manually written file. Such a transformation might break things. Therefore, ensure that:

1. /etc/conf.d/nftables contains the parameter -n for the SAVE\_OPTIONS
2. Loading the rule set as root yields a working configuration
3. The save and restore cycle of restarting nftables service causes the issue

If all three conditions are met, remove the -n parameter from SAVE\_OPTIONS in /etc/conf.d/nftables. Then load the rule set again from the manually written file and restart the service. This cycles through save and restore and should create a fully working rule set.

## Removal

`root #``emerge --ask --depclean --verbose sys-firewall/nftables`
## Examples

See [Nftables/Examples](https://wiki.gentoo.org/wiki/Nftables/Examples) for examples covering:

- workstation firewalls
- servers
- routers
- NAT
- IPv4/IPv6 filtering
- connection tracking
- sets and maps
- network-device filtering
- service-specific rules

## See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem.
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables] — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
- [Security Handbook](https://wiki.gentoo.org/wiki/Security_Handbook) — valuable guidance on Gentoo Linux security and cybersecurity in general.
- [Iptables](https://wiki.gentoo.org/wiki/Iptables) — a program used to configure and manage the kernel's netfilter modules.
