<!-- source: https://wiki.gentoo.org/wiki/Nftables | group: Gentoo Wiki (Main) | wiki-title: Nftables -->
---
title: nftables
url: https://wiki.gentoo.org/wiki/Nftables
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: fc91991ae589ce6f
license: CC BY-SA 4.0
---

# nftables

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**nftables** is a Linux packet filtering framework for the Netfilter subsystem, providing a unified interface for configuring packet filtering, connection tracking, NAT, and related network functionality.



## Introduction

nftables is the packet-filtering framework and ruleset interface for the Linux kernel's Netfilter subsystem. It provides a unified rule language and the nft userspace utility for configuring packet filtering, connection tracking, NAT, and other Netfilter functionality.

**tables**, **chains**, and **rules** builds the ruleset: tables contain chains, chains contain rules, and base chains attach rulesets to Netfilter **hooks**; rules combine packet and connection-state expressions with statements that determine what happens when a packet matches.

nftables allows variable numbers of tables or chains, whereas legacy iptables, ip6tables, arptables, and ebtables are fixed number.

Ruleset defines its own structure. Ruleset can use sets, maps, counters, verdicts, and other objects to build considerably more expressive packet-processing logic.

nftables **address families** determine which protocol layer a table operates on. inet family permits IPv4 and IPv6 rules to share a table, while ip, ip6, arp, bridge, and netdev provide more specialized filtering contexts.

nft utility is the userspace interface to the nftables ruleset. Rules can be manipulated individually or loaded from files as an atomic ruleset, making the firewall configuration reproducible and straightforward to manage.

Legacy iptables and ip6tables command interfaces remain available through the nftables-backed iptables-nft implementation. Legacy software continues to use iptables command syntax using iptables-nft transformer tool

As with the iptables framework, nftables is built upon rules which specify actions. These rules are attached to chains. A chain can contain a collection of rules and is registered in the netfilter hooks. Chains are stored inside tables. A table is specific for one of the layer 3 protocols. One of the main differences with iptables is that there are no predefined tables and chains anymore.

### Tables

A table is a container for chains. Unlike [iptables](https://wiki.gentoo.org/wiki/Iptables), nftables has no predefined tables (filter, raw, mangle...). An iptables-like structure can be used, but this is not required. Tables must be defined with an `address family` and `name`, which can be anything.

The `address families` used by nftables are documented under man nft 8:

| Address family | Description | 
|---|---|
| **ip** | Used for IPv4 related chains. | 
| **ip6** | Used for IPv6 related chains. | 
| **inet** | Mixed IPv4/IPv6 chains (kernel 3.14 and up). | 
| **arp** | Used for ARP related chains. | 
| **bridge** | Used for bridging related chains. | 
| **netdev** | Used for chains that filter early in the stack (kernel 4.2 and up). | 

### Chains

Chains are used to group rules. As with the tables, nftables does not have any predefined chains. Chains are grouped in **base** and **non-base** types. Base chains are registered in one of the netfilter hooks, non-base chains are not. **base chain**s must be defined with a `hook` type, and `priority`. In contrast, **non-base chains** are not attached to a hook and they don't see any traffic by default. They can be used as jump targets to arrange a rule-set in a tree of chains.

The `chains` used by nftables are documented under man nft 8:

| Chain | Families | Hooks | Description | 
|---|---|---|---|
| **filter** | **all** | **all** | Standard chain, generally used for filtering. | 
| **nat** | **ip**, **ip6**, **nat** | **prerouting**, **input**, **output**, **postrouting** | Used to perform Native Address Translation using conntrack. Only the first packet of a connection uses this chain. | 
| **route** | **ip**, **ip6** | **output** | Packets that traverse this chain type, if about to be accepted, trigger a route lookup if the IP header has changed. | 

Each address family has different hook capabilities, defined under the respective `{name} ADDRESS FAMILY` section of man nft 8:

#### IPv4/IPv6/INET/Bridge hooks

| Hook | Description | 
|---|---|
| **prerouting** | Processes all packets entering the system, invoked before the routing process, used for early filtering or changing attributes which would affect routing. | 
| **input** | Processes packets destined for the local system. | 
| **forward** | Processes packets received by the local system, but destined for another one. | 
| **output** | Processes packets sent by the local system (includes NATed packets). | 
| **postrouting** | Processes all packets leaving the system, regardless of the source. | 
| **ingress** (Since 5.10) | Processes all packets entering the system, before **prerouting** (and all other layer 3 handlers).  Only available to the **inet** `address family`. | 

#### ARP hooks

| Hook | Description | 
|---|---|
| **input** | Processes ARP packets the local system receives. | 
| **output** | Processes ARP packets leaving the local system. | 

#### Netdev hooks

| Hook | Description | 
|---|---|
| **ingress** | Processes all packets entering the system. Invoked after network taps such as [tcpdump](https://wiki.gentoo.org/wiki/Tcpdump), and before layer 3 handlers. | 
| **egress** | Processes all packets leaving the system.  Invoked before [tcpdump](https://wiki.gentoo.org/wiki/Tcpdump) egress. | 

### Rules

**Rules** specify what action is taken for a given packet. **Rules** are attached to **chains**. Each **rule** can have an expression to match packets and one or more actions to perform when matching. Unlike iptables, it is possible to specify multiple actions per **rule**, and counters are off by default. A **counter** must be specified explicitly in each rule for which packet- and byte-counters are desired.

Each rule has a unique handle number by which it can be distinguished.

The following matches are available:

- **ip**: IP protocol.
- **ip6**: IPv6 protocol.
- **tcp**: TCP protocol.
- **udp**: UDP protocol.
- **udplite**: UDP-lite protocol.
- **sctp**: SCTP protocol.
- **dccp**: DCCP protocol.
- **ah**: Authentication headers.
- **esp**: Encrypted security payload headers.
- **ipcomp**: IPcomp headers.
- **icmp**: icmp protocol.
- **icmpv6**: icmpv6 protocol.
- **ct**: Connection tracking.
- **meta**: meta properties such as interfaces.

#### Matches

| Match | Arguments | Description/Example | 
| **ip** | version | Ip Header version | 
|  | hdrlength | IP header length | 
|  | tos | Type of Service | 
|  | length | Total packet length | 
|  | id | IP ID | 
|  | frag-off | Fragmentation offset | 
|  | ttl | Time to live | 
|  | protocol | Upper layer protocol | 
|  | checksum | IP header checksum | 
|  | saddr | Source address | 
|  | daddr | Destination address | 
| **ip6** | version | IP header version | 
|  | priority |  | 
|  | flowlabel | Flow label | 
|  | length | Payload length | 
|  | nexthdr | Next header type (Upper layer protocol number) | 
|  | hoplimit | Hop limit | 
|  | saddr | Source Address | 
|  | daddr | Destination Address | 
| **tcp** | sport | Source port | 
|  | dport | Destination port | 
|  | sequence | Sequence number | 
|  | ackseq | Acknowledgement number | 
|  | doff | Data offset | 
|  | flags | TCP flags | 
|  | window | Window | 
|  | checksum | Checksum | 
|  | urgptr | Urgent pointer | 
| **udp** | sport | Source port | 
|  | dport | destination port | 
|  | length | Total packet length | 
|  | checksum | Checksum | 
| **udplite** | sport | Source port | 
|  | dport | destination port | 
|  | cscov | Checksum coverage | 
|  | checksum | Checksum | 
| **sctp** | sport | Source port | 
|  | dport | destination port | 
|  | vtag | Verification tag | 
|  | checksum | Checksum | 
| **dccp** | sport | Source port | 
|  | dport | destination port | 
| **ah** | nexthdr | Next header protocol (Upper layer protocol) | 
|  | hdrlength | AH header length | 
|  | spi | Security Parameter Index | 
|  | sequence | Sequence Number | 
| **esp** | spi | Security Parameter Index | 
|  | sequence | Sequence Number | 
| **ipcomp** | nexthdr | Next header protocol (Upper layer protocol) | 
|  | flags | Flags | 
|  | cfi | Compression Parameter Index | 
| **icmp** | type | icmp packet type | 
| **icmpv6** | type | icmpv6 packet type | 
| **ct** | state | State of the connection | 
|  | direction | Direction of the packet relative to the connection | 
|  | status | Status of the connection | 
|  | mark | Connection mark | 
|  | expiration | Connection expiration time | 
|  | helper | Helper associated with the connection | 
|  | l3proto | Layer 3 protocol of the connection | 
|  | saddr | Source address of the connection for the given direction | 
|  | daddr | Destination address of the connection for the given direction | 
|  | protocol | Layer 4 protocol of the connection for the given direction | 
|  | proto-src | Layer 4 protocol source for the given direction | 
|  | proto-dst | Layer 4 protocol destination for the given direction | 
| **meta** | length | Length of the packet in bytes: *meta length > 1000* | 
|  | protocol | ethertype protocol: *meta protocol vlan* | 
|  | priority | TC packet priority | 
|  | mark | Packet mark | 
|  | iif | Input interface index | 
|  | iifname | Input interface name | 
|  | iiftype | Input interface type | 
|  | oif | Output interface index | 
|  | oifname | Output interface name | 
|  | oiftype | Output interface hardware type | 
|  | pkttype | Packet type: unicast, multicast or broadcast | 
|  | skuid | UID associated with originating socket | 
|  | skgid | GID associated with originating socket | 
|  | rtclassid | Routing realm | 

#### Statements

Statements represent the action to be performed when a rule matches. They exist in two kinds: Terminal statements, unconditionally terminate the evaluation of the current rules and non-terminal statements that either conditionally or never terminate the current rules. There can be an arbitrary amount of non-terminal statements, but there must be only a single terminal statement. The terminal statements can be:

- **accept**: Accept the packet and stop the ruleset evaluation.
- **drop**: Drop the packet and stop the ruleset evaluation.
- **reject**: Reject the packet with an icmp message.
- **queue**: Queue the packet to userspace and stop the ruleset evaluation.
- **continue**:
- **return**: Return from the current chain and continue at the next rule of the last chain. In a base chain it is equivalent to accept.
- **jump \<chain>**: Continue at the first rule of \<chain>. It will continue at the next rule after a return statement is issued.
- **goto \<chain>**: Similar to jump, but after the new chain the evaluation will continue at the last chain instead of the one containing the goto statement.

### Sets

*nftables* allows defining anonymous and named **[sets](https://wiki.nftables.org/wiki-nftables/index.php/Sets)** ([dictionaries](https://wiki.nftables.org/wiki-nftables/index.php/Dictionaries) and [maps](https://wiki.nftables.org/wiki-nftables/index.php/Maps)). For example, the following nft script defines the fullbogons set, adds elements to it and drops packages from the IPs conforming the set.

**`rules.nft`**

```
#!/sbin/nft
add set filter fullbogons { type ipv4_addr; flags interval; }
add element filter fullbogons {0.0.0.0/8}
add element filter fullbogons {10.0.0.0/8}
add element filter fullbogons {41.62.0.0/16}
add element filter fullbogons {41.67.64.0/20}
add rule filter input iifname eth0 ct state new ip saddr @fullbogons counter drop comment "drop from blacklist"
```
## Installation

### Kernel

*Example Config*: A **bare minimum** for basic IPv4 firewalling with NAT:

**Nftables kernel requirements**

\[\*\] Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.  --->
   Networking options  --->
       \[\*\] Network packet filtering framework (Netfilter) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\</code> to find this item.  --->
           Core Netfilter Configuration  --->
               \<M> Netfilter connection tracking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_CONNTRACK\</code> to find this item.
               \<M> Netfilter nf\_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\</code> to find this item.
               \<M>   Netfilter nf\_tables conntrack module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_CT\</code> to find this item.
               \<M>   Netfilter nf\_tables log module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_LOG\</code> to find this item.
               \<M>   Netfilter nf\_tables limit module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_LIMIT\</code> to find this item.
               \<M>   Netfilter nf\_tables masquerade support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_MASQ\</code> to find this item.
               \<M>   Netfilter nf\_tables nat module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NFT\_NAT\</code> to find this item.
           IP: Netfilter Configuration  --->
               \<M> IPv4 nf\_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TABLES\_IPV4\</code> to find this item.
               \<M> IPv4 packet rejection [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_REJECT\_IPV4\</code> to find this item.
               \<M> IP tables support (required for filtering/masq/NAT) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_IPTABLES\</code> to find this item.
               \<M>   Packet filtering [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_FILTER\</code> to find this item.
               \<M>     REJECT target support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_TARGET\_REJECT\</code> to find this item.
               \<M>   iptables NAT support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_NAT\</code> to find this item.
               \<M>     MASQUERADE target support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_TARGET\_MASQUERADE\</code> to find this item.

*Additional Common Config:* For **mixed IPv4 and IPv6 rules** combined into one table: *CONFIG\_NF\_TABLES\_INET*

(If family *inet* is not enabled, only families *ip* and *ip6* can be used individually)

**Nftables inet family**

*Additional Optional Config:* **Early filtering based on network device** requires netdev tables support*: CONFIG\_NF\_TABLES\_NETDEV*

**Nftables netdev family**

Nftables is very modular, and has many more options than mentioned here. Certain software likely requires additional features.

Depending on the goal, the bare minimum shown here may suffice, or other additional options can be enabled as modules so the kernel will load them as needed.

Disclaimer: This network section of the kernel changes frequently. More modules = more compatibility (they only load when requested).


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

### Emerge

Install [net-firewall/nftables](https://packages.gentoo.org/packages/net-firewall/nftables):

`root #``emerge --ask net-firewall/nftables`
## Configuration

### OpenRC

The init script, /etc/init.d/nftables, supports the following actions:

- **save** - Stores the currently loaded ruleset in the location defined by `NFTABLES_SAVE`, default: /var/lib/nftables/rules-save.
- **reload** - Loads the currently loaded ruleset from `NFTABLES_SAVE`.
- **stop** - Intended to be called on system shutdown. If `SAVE_ON_STOP` is enabled, saves the ruleset.
- **start** - Intended to be called on system boot, loads the ruleset from `NFTABLES_SAVE`.
- **clear** - Flushes the currently loaded ruleset, equivalent to nft flush ruleset.
- **list** - lists the currently loaded ruleset.

Nftables can be started at boot with:

`root #``rc-update add nftables default`
### systemd

After first setup:

`root #``touch /var/lib/nftables/rules-save``root #``systemctl enable --now nftables-store``root #``systemctl enable --now nftables-load`
## Usage

All nftable commands are executed with the nft utility from [net-firewall/nftables](https://packages.gentoo.org/packages/net-firewall/nftables).

### Tables

#### Creating tables

The following command adds a **table** called *base\_table* for the IPv4 and IPv6 layers:

`root #``nft add table inet base_table`
Likewise, a table for arp can be created with

`root #``nft add table arp base_table`
#### Listing tables

The following command lists all tables:

`root #``nft list tables`
table inet base\_table
table arp base\_table

The tables can be filtered by type by adding the **address family** as an argument:

`root #``nft list tables inet`
table inet base\_table

The contents of the table **base\_table** can be listed with:

`root #``nft list table inet base_table````
table inet base_table {
        chain input_filter {
                 type filter hook input priority 0;
                 ct state established,related accept
                 iifname "lo" accept
                 ip protocol icmp accept
                 drop
        }
}
```
`root #``nft -a list table inet base_table````
table inet base_table {
        chain input_filter {
                 type filter hook input priority 0;
                 ct state established,related accept # handle 2
                 iifname "lo" accept # handle 3
                 ip protocol icmp accept # handle 4
                 drop # handle 5
        }
}
```
#### Deleting tables

The following command deletes the **table** called *base\_table*, which is part of the **address family** *inet*:

`root #``nft delete table inet base_table`
### Chains

#### Adding chains

The following command adds a **base chain** called *input\_filter* to the **inet** *base\_table* table. It is registered to the *input* **hook** with **priority** *0*, and **type** *filter*.

`root #``nft add chain inet base_table input_filter "{type filter hook input priority 0;}"` #### Listing chain rules

The rules for a chain can be listed using:

`root #``nft list chain inet base_table input_filter````
table inet base_filter {
	chain input_filter {
	        type filter hook input priority 0;
        }
}
```
#### Deleting chains

Chains can be removed with:

`root #``nft delete chain inet base_table input_filter`
### Rules

#### Adding rules

The following command adds a **rule** to the **chain** called *input\_filter*, on the *base\_table* **table**, dropping all incoming traffic to port 80:

`root #``nft add rule inet base_table input_filter tcp dport 80 drop`
#### Listing all rules

The following command can be used to list all rules in the current ruleset (with handles):

`root #``nft -a list ruleset````
table inet base_table {
        chain input_filter { # handle 1
                 type filter hook input priority 0; # handle 2
                 tcp dport http drop # handle 3
        }
        chain output_filter { # handle 4
                 type filter hook input priority 0; # handle 5
                 tcp sport http drop # handle 6
        }
}
```
#### Deleting rules

To delete a rule, the rule's handle number is required:

`root #``nft delete rule inet base_table input_filter handle 3`
## Modular Ruleset Management

nft supports atomic rule replacement by using nft -f. Thus it is possible to conveniently manage the rules using files.

Nftables allows including additional files and directories. A directory, such as /etc/nftables.conf.d/ can be created which contains additional rulesets to be loaded.

The following file creates a basic skeleton of a ruleset, which can be used with modules, loaded from /etc/nftables.conf.d/:

**`/etc/nftables.rules`**

**Basic router ruleset**

```
#! /sbin/nft -f
 
flush ruleset
 
table netdev filter {
  # Basic filter chain, devices can be configued to jump here
  chain ingress_filter {
    # Drop all fragments.
    ip frag-off & 0x1fff != 0 counter drop
 
    # Drop XMAS packets.
    tcp flags & (fin|syn|rst|psh|ack|urg) == fin|syn|rst|psh|ack|urg counter drop
 
    # Drop NULL packets.
    tcp flags & (fin|syn|rst|psh|ack|urg) == 0x0 counter drop
 
    # Drop uncommon MSS values.
    tcp flags syn tcp option maxseg size 1-535 counter drop
 
  }
}
 
table inet filter {
  chain input {
    type filter hook input priority 200; policy drop;
    counter jump input_hook
    counter jump base_filter
    log prefix "Dropped input traffic: " counter drop
  }
  chain input_hook {
  }
 
  chain base_filter {
    counter jump drop_filter
    ct state vmap {
      established: accept,
      related: accept,
      new: continue,
      invalid: drop
    }
    # Allow loopback traffic
    iifname lo counter accept
    oifname lo counter accept
  }
 
  chain drop_filter {
  }
 
  chain forward {
    type filter hook forward priority 200; policy drop;
    counter jump forward_hook
    counter jump base_filter
    log prefix "Dropped forwarded traffic: " counter drop
  }
 
  chain forward_hook {
  }
 
  chain output {
    type filter hook output priority 200; policy drop;
    counter jump output_hook
    counter jump base_filter
    log prefix "Dropped output traffic: " counter drop
  }
  chain output_hook {
  }
}
 
table inet nat {
  chain prerouting {
    type nat hook prerouting priority 0;
  }
  chain postrouting {
    type nat hook postrouting priority 500;
  }
}
include "/etc/nftables.rules.d/*.rules"
```
### Example Modules

#### Variable definitions

**`/etc/nftables.rules.d/00-definitions.rules`**

**Define variables which can be used in other modules.**

```
#! /sbin/nft -f
 
define lan_interface = fib.lan
define wifi_interface = ax1800
define management_interface = fib.management
define dmz_interface = ethernet2
 
define wan_interface = fib.wan
 
define wifi_network = 192.168.1.0/24
define lan_network = 192.168.10.0/24
define management_network = 192.168.255.0/24
define dmz_network = 192.168.2.0/24
 
table inet filter {
  set trusted_nets {
    type ipv4_addr
    flags interval
    elements = { $lan_network, $wifi_network }
  }
  set dmz_nets {
    type ipv4_addr
    flags interval
    elements = { $dmz_network }
  }
  set untrusted_nets {
    type ipv4_addr
    flags interval
    elements = { $dmz_network }
  }
}
```
#### Jumping to the ingress filter

**`/etc/nftables.rules.d/00-ingress.rules`**

**Hook the device ingress for*fib1* and *fib2*, filtering with the defined *ingress\_filter* chain.**

```
#! /sbin/nft -f
 
table netdev filter {
  chain ingress {
    type filter hook ingress device fib1 priority -500;
    jump ingress_filter
  }
  chain ingress {
    type filter hook ingress device fib2 priority -500;
    jump ingress_filter
  }
}
```
#### Basic drop filter

**`/etc/nftables.rules.d/01-drop-policy.rules`**

**Populate the*drop\_filter* chain to drop some spammy traffic found on the DMZ.**

```
#! /sbin/nft -f
 
define dmz_spam_udp = { 1234 }
define dmz_spam_tcp = { 2350 }
 
table inet filter {
  set spam_udp {
    type inet_service
    elements = { $dmz_spam_udp }
  }
  set spam_tcp {
    type inet_service
    elements = { $dmz_spam_tcp }
  }
 
  chain drop_filter {
    tcp dport @spam_tcp counter drop
    udp dport @spam_udp counter drop
  }
}
```
#### Basic ICMP filter

**`/etc/nftables.rules.d/01-icmp.rules`**

**Allow basic ICMP traffic**

```
#! /sbin/nft -f
 
define allowed_icmp_types = { echo-reply, echo-request }
define trusted_icmp_types = { destination-unreachable, time-exceeded }
define allowed_icmpv6_types = { nd-router-solicit, nd-router-advert, nd-neighbor-solicit, nd-neighbor-advert, echo-request, echo-reply }
 
table inet filter {
  chain base_filter {
    ip protocol icmp jump icmp_filter
    ip6 nexthdr icmpv6 jump icmpv6_filter
  }
  chain icmp_filter {
    icmp type $allowed_icmp_types counter accept
    ip saddr @trusted_nets icmp type $trusted_icmp_types counter accept
    ip daddr @trusted_nets icmp type $trusted_icmp_types counter accept
  }
  chain icmpv6_filter {
    icmpv6 type $allowed_icmpv6_types counter accept
  }
}
```
#### Allow DHCP traffic

**`/etc/nftables.rules.d/05-dhcp.rules`**

**Allow DHCP client and server traffic on appropriate interfaces.**

```
#! /sbin/nft -f
 
define dhcp_server_interfaces = { $lan_interface, $wifi_interface, $dmz_interface }
define dhcp_client_interfaces = { $management_interface, $wan_interface }
 
table inet filter {
  set dhcp_server {
   type inet_service;
   elements = { 67 }
  }
  set dhcp_client {
   type inet_service;
   elements = { 68 }
  }
  set dhcp6_server {
   type inet_service;
   elements = { 547 }
  }
  set dhcp6_client {
   type inet_service;
   elements = { 546 }
  }
  chain input_hook {
    iifname $dhcp_client_interfaces udp sport @dhcp_server udp dport @dhcp_client counter accept comment "Allow DHCP client input traffic"
    iifname $dhcp_client_interfaces udp sport @dhcp6_server udp dport @dhcp6_client counter accept comment "Allow DHCPv6 client input traffic"
    iifname $dhcp_server_interfaces udp dport @dhcp_server udp sport @dhcp_client counter accept comment "Allow DHCP server input traffic"
    iifname $dhcp_server_interfaces udp dport @dhcp6_server udp sport @dhcp6_client counter accept comment "Allow DHCPv6 server input traffic"
  }
  chain output_hook {
    oifname $dhcp_client_interfaces udp dport @dhcp_server udp sport @dhcp_client counter accept comment "Allow DHCP client output traffic"
    oifname $dhcp_client_interfaces udp dport @dhcp6_server udp sport @dhcp6_client counter accept comment "Allow DHCPv6 client output traffic"
    oifname $dhcp_server_interfaces udp sport @dhcp_server udp dport @dhcp_client counter accept comment "Allow DHCP server output traffic"
    oifname $dhcp_server_interfaces udp sport @dhcp6_server udp dport @dhcp6_client counter accept comment "Allow DHCPv6 server output traffic"
  }
}
```
#### Allow inbound and forwarded SSH traffic

**`/etc/nftables.rules.d/22-ssh.rules`**

**Allow inbound and forwarded SSH from the*$management\_network*; allow SSH to be forwarded to GitHub servers, from certain networks, and users on the router.**

```
#! /sbin/nft -f
define github_ssh_servers = { 140.82.112.3, 140.82.112.4, 140.82.114.3, 140.82.113.4, 140.82.114.4, 140.82.113.3 }
 
table inet filter {
  set external_ssh_servers {
    type ipv4_addr
    elements = { $github_ssh_servers }
  }
  set external_ssh_clients {
    type ipv4_addr
    flags interval
    elements = { $lan_network }
  }
  set ssh_clients {
    type ipv4_addr
    flags interval
    elements = { $management_network }
  }
  set ssh_ports {
    type inet_service;
    elements = { 22 }
  }
  chain ssh_filter {
    ip saddr @external_ssh_clients ip daddr @external_ssh_servers counter accept comment "Allow these users to SSH to specified external servers"
    ip saddr @ssh_clients counter accept comment "Allow this set to SSH anywhere, including the router itself"
    skuid 1000 ip daddr @external_ssh_servers counter accept comment "Allow SSH traffic from UID 1000 to allowed external servers"
  }
  chain base_filter {
    tcp dport @ssh_ports counter jump ssh_filter
  }
}
```
#### Allow outbound and forwarded NTP traffic

**`/etc/nftables.rules.d/21-ntp.rules`**

**Allow forwarded NTP traffic, outbound from the NTP user.**

```
#! /sbin/nft -f
table inet filter {
  set ntp_ports {
    type inet_service
    elements = { 123 }
  }
  set ntp4_ports {
    type inet_service
    elements = { 4460 }
  }
  chain base_filter {
    tcp dport @ntp4_ports counter jump ntp_filter
    udp dport @ntp_ports counter jump ntp_filter
  }
  chain ntp_filter {
    ip saddr @trusted_nets counter accept
    skuid 123 counter accept
  }
}
```
#### NAT LANs

**`/etc/nftables.rules.d/05-lan-nat.rules`**

**NAT some local networks.**

```
#! /sbin/nft -f
 
table inet nat {
  set nat_nets {
    type ipv4_addr
    flags interval
    elements = { $lan_network, $wifi_network, $dmz_network }
  }
  chain	postrouting {
    oifname $wan_interface ip saddr @nat_nets counter masquerade
  }
}
```
#### Masquerade Docker Traffic

**`/etc/nftables.rules.d/10-docker.rules`**

**Masquerade docker traffic**

```
#! /sbin/nft -f
 
define docker_default_net = 172.17.0.0/16
define docker_server_net = 10.100.100.0/24
define docker_nets = { $docker_default_net, $docker_server_net }
table inet filter {
  set docker_nets {
    type ipv4_addr
    flags interval
    elements = { $docker_nets }
  }
  set dns_ports {
    type inet_service
    elements = { 53 }
  }
  set web_ports {
    type inet_service
    elements = { 443 }
  }
 
  chain docker_filter {
    udp dport @dns_ports counter accept
    oifname $wan_interface tcp dport @web_ports counter accept
  }
  chain forward_hook {
    ip saddr @docker_nets counter jump docker_filter
  }
}
 
table inet nat {
  set nat_nets {
    type ipv4_addr
    flags interval
    elements = { $docker_nets }
  }
}
```
## Logging

### Log action

The **log** rule can be used to log traffic to the kernel log.

From the example configuration above, `log prefix "Dropped input traffic: " counter drop` results in:

### syslog-ng nftables configuration

Most logging daemons will log nftables traffic to the kernel log file, since it is logged to the kernel log. This can make it difficult to view dropped traffic, since it is mixed with non-firewall related traffic.

To configure [syslog-ng](https://wiki.gentoo.org/wiki/Syslog-ng) to log certain nftables traffic to other files:

**`/etc/syslog-ng/syslog-ng.conf`**

**Filter*fib.wan* interface traffic to /var/log/nft/wan.log**

```
source kernsrc {
    file("/proc/kmsg");
};
 
destination nft_WAN { file("/var/log/nft/wan.log"); };
 
filter f_nft_WAN { program(kernel)
                   and message("^.*IN=fib\.wan.*$"); };
 
log { source(kernsrc); filter(f_nft_WAN); destination(nft_WAN); };
```
## Examples

See the [Nftables examples article](https://wiki.gentoo.org/wiki/Nftables/Examples).

## Troubleshooting

Before loading new or edited rules check them with nft

`user $``nft -c -f ruleset`
### No such file or directory

If this error is printed for every chain of a table definition make sure, that the table's family is available through the kernel. This happens for example if the table uses family *inet* and the kernel configuration did not enable mixed IPv4 and IPv6 rules (CONFIG\_NF\_TABLES\_INET).

### Conflicting intervals

A set definition of IP ranges causes this error if ranges overlap. For example 224.0.0.0/3 and 240.0.0.0/5 overlap completely. Either add *auto-merge* to the set's options, drop the range that is fully included or change syntax to 224.0.0.0-255.255.255.255.

### Connections blocked after nftables restart or reboot

The default configuration of the save and restore functions uses numeric mode to store the rule set. The persisted rule set could have changed from the original upload from a manually written file. Such a transformation might break things. Therefore, ensure that:

1. /etc/conf.d/nftables contains the parameter -n for the SAVE\_OPTIONS
2. Loading the rule set as root yields a working configuration
3. The save and restore cycle of restarting nftables service causes the issue

If all three conditions are met, remove the -n parameter from SAVE\_OPTIONS in /etc/conf.d/nftables. Then load the rule set again from the manually written file and restart the service. This cycles through save and restore and should create a fully working rule set.

### Family netdev and ingress hook

Broken packets should be rejected early which requires an ingress hook for family netdev. This sets up a chain that acts for a dedicated network device before packets enter further processing – improved performance. The configuration looks like this:

Mind the device name enp4s0. If this changes for example when changing hardware or an upgrade changed device naming this family is broken. In turn none of the rules will be loaded. The error looks like this (filename and line numbers differ depending on the host configuration):

Check that the device name is actually correct and exists, e.g. ip addr list.

## See also

- [Iptables](https://wiki.gentoo.org/wiki/Iptables) — a program used to configure and manage the kernel's netfilter modules. Contains a section about migration to nftables

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [Nftables/Configuration/Chain](https://wiki.gentoo.org/wiki/Nftables/Configuration/Chain) — contains a group of rules used to process network traffic.
