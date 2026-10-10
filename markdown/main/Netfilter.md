<!-- source: https://wiki.gentoo.org/wiki/Netfilter | group: Gentoo Wiki (Main) | wiki-title: Netfilter -->
---
title: Netfilter
url: https://wiki.gentoo.org/wiki/Netfilter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-09"
fingerprint: a79bdb11c3309d45
license: CC BY-SA 4.0
---

# Netfilter

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Netfilter** is the Linux kernel’s packet-filtering framework, providing in-kernel hooks for firewalling, NAT, connection tracking, and packet mangling.

The page introduces Netfilter from kernel architecture through userspace interfaces and individual subsystems.

The final sections cover kernel configuration and diagnostics.

## Architecture

[Netfilter] provides the following kernel facilities:

- filter packets inside the Linux networking stack
- intercept packets at multiple points using hooks
- traverse packets through registered hooks
- provide an API between kernel space and user space
- allow user space to configure Netfilter inside the kernel

### Linux networking stack

The Linux networking stack moves packets between network devices, protocol handlers, sockets, and Netfilter hooks.

### Netfilter hooks

Netfilter hooks intercept packets at defined points in the Linux networking stack.

- Hook points identify where Netfilter invokes registered kernel callbacks during packet traversal.
- Hook priority orders registered Netfilter callbacks within each hook point.
- Hook verdicts tell Netfilter whether to continue, drop, reject, queue, or accept a packet.

### Packet paths

Packet paths describe how packets traverse Linux networking and Netfilter hooks.

- Incoming traffic enters through a network device and traverses the receive-side Netfilter hooks.
- Locally generated traffic originates from the kernel or a local socket and traverses the output-side Netfilter hooks.
- Forwarded traffic enters through one network interface and traverses the forwarding path before leaving through another interface.
- Outgoing traffic leaves through a network device after traversing the applicable Netfilter output hooks.

### Packet processing

Packet processing invokes registered Netfilter callbacks as packets traverse the kernel networking paths.

- Hook registration installs kernel callbacks into selected Netfilter hook points with an assigned priority
- Hook traversal invokes registered callbacks in priority order and processes their returned verdicts
- Packet modification mutates packet metadata or payload before subsequent networking or Netfilter processing
- **Verdicts** determine whether packet traversal continues or terminates

## Userspace integration

Userspace integration configures Netfilter and exchanges state with the kernel.

- **Netlink** is a kernel/userspace interface for configuring Netfilter state and exchanging networking information
- APIs allow userspace applications to configure Netfilter
- [nft](https://wiki.gentoo.org/wiki/Nft) configure and inspect Netfilter
- Other applications use Netfilter interfaces to implement custom packet-processing and networking functions
- Netfilter provides packet logging through the nfnetlink\_log subsystem.
  - NFLOG delivers selected packet information from the kernel to a userspace application through Netlink.
  - The userspace application is responsible for processing or storing the resulting log records.

### NFLOG

#### nfnetlink\_log

##### userspace consumer

NFLOG delivers selected packet information from the kernel to a userspace application through Netlink.

## Subsystems

nftables maps ruleset objects onto the Netfilter kernel infrastructure.

Tables provide namespaces for chains and other ruleset objects.

[Chains](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) attach rule processing to Netfilter hooks or provide reusable processing paths.

[Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) combine expressions, statements, and verdicts to process packets.

Stateful objects maintain or reference packet-processing state.

Sets and maps provide stateful lookup data.

Counters and quotas maintain packet and byte accounting.

Connection-tracking references access connection state maintained by the Netfilter connection-tracking subsystem.

Flowtables provide accelerated packet processing for eligible flows.

Network address translation uses Netfilter NAT infrastructure to modify packet addresses and ports.

Packet logging uses Netfilter logging infrastructure to deliver packet-log events to userspace.

Security enforcement integrates nftables with Netfilter security infrastructure and other Linux security facilities.

The [nft](https://wiki.gentoo.org/wiki/Nft) userspace command configures ruleset objects through the Netlink API.

The Netfilter kernel infrastructure evaluates rules and maintains associated state.

## Packet processing pipeline

The packet processing pipeline moves packets through the Netfilter hooks.

- **Ingress** receives packets from network devices.
- **Prerouting** processes packets before the routing decision.
- **routing** decision selects the local-delivery or forwarding path.
- **Input** processes packets destined for the local system.
- **Forward** processes packets routed between network interfaces.
- **Output** processes packets generated by the local system.
- **Postrouting** processes packets after the routing decision and before transmission.

## Configuration interfaces

Netfilter APIs configure the following kernel networking interfaces:

- nftables - current Netfilter packet-filtering framework
- iptables - legacy xtables interface
- ip6tables - legacy xtables interface for IPv6 filtering
- arptables - ARP packet filtering
- ebtables - Ethernet bridge packet filtering

## nftables integration

nftables maps its ruleset objects to Netfilter kernel infrastructure.

### Tables and families

nftables tables define namespaces for ruleset objects and associate those objects with a Netfilter address family.

### Base chains

[Base chains](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain#Base_chains) attach **nftables** rule processing to Netfilter packet-processing hooks.

### Hook and priority

Netfilter hooks determine where packet processing occurs.

Chain priority determines the ordering of base chains attached to the same hook.

### Rules

Rules define packet-processing operations within chains.

Expressions inspect packet, connection, interface, and other kernel state.

Statements perform actions on packets or associated state.

Verdicts determine the disposition or subsequent processing of packets.

### Stateful objects

Stateful objects maintain or reference information used during packet processing.

Sets and maps provide lookup data.

Counters and quotas maintain packet and byte accounting.

Connection-tracking objects provide access to connection state maintained by Netfilter.

### Flowtables

Flowtables provide an accelerated packet-processing path for eligible flows.

Flowtable entries bypass subsequent traversal of the normal Netfilter hook path for packets belonging to an offloaded flow.

## Connection tracking

Connection tracking updates state machines for network connections and packet flows.

- **Conntrack state** classifies packets by their connection state.
- **Connection tracking and NAT** associates NAT translations with tracked connections.
- **Connection tracking and filtering** carries the connection state to packet-filtering rules.
- **Expectations and helpers** track related connections created by protocols with dynamic connection behavior.

## Network address translation

Network address translation rewrites packet addresses and ports.



- **Destination NAT** rewrites destination addresses and ports before packet delivery.
- **Source NAT** rewrites source addresses and ports before packet transmission.
- **Masquerading** performs dynamic source NAT using the outgoing interface address.
- **NAT and connection tracking** maintain NAT state of each packets having same 5-tuples.

## Special processing paths

Some Netfilter processing occurs outside the conventional IP packet path, including bridge forwarding, network-device ingress, flowtable processing, and userspace queueing.

- Bridge networking - Layer-2 bridge forwarding has its own Netfilter integration.
- Network-device ingress - Processing occurs at the device ingress point, before ordinary IP processing.
- Flowtable fast path - Established flows can take a shortcut around ordinary hook traversal.
- Userspace packet processing - A packet can leave the normal kernel processing sequence temporarily for userspace.

## Interaction with other Linux networking facilities

[Netfilter] interacts with other Linux networking and security facilities:

- **Routing** determines packet forwarding and output paths used by Netfilter hooks.
- **Traffic control** processes packets through Linux queuing disciplines and traffic-control hooks.
- **Network namespaces** isolate networking state, including Netfilter rulesets and connection-tracking state.
- **[Linux Security Modules](https://wiki.gentoo.org/wiki/SELinux)** provide security enforcement that can interact with Netfilter packet processing.
- **UNIX socket layer** provides local userspace communication used by networking applications and logging facilities.

## Legacy and current interfaces

Netfilter supports legacy xtables and current nftables userspace interfaces.

### iptables framework

The iptables framework configures Netfilter through legacy xtables interfaces.

- iptables and ip6tables manage IPv4 and IPv6 rules.
- arptables and ebtables manage ARP and Ethernet bridge filtering.

### nftables framework

The nftables framework configures Netfilter through the Netlink API.

- The [nft](https://wiki.gentoo.org/wiki/Nft) command manages tables, chains, rules, sets, and maps.
- The inet family handles both IPv4 and IPv6.

### xtables compatibility

The iptables-nft compatibility frontend translates legacy commands into nftables ruleset operations.

- The iptables-legacy frontend uses the original xtables interfaces.
- Both implementations can coexist, but maintain separate rulesets.

Use one implementation consistently to simplify firewall administration and diagnostics.



## Kernel configuration

The kernel CONFIG\_\* options controlling Netfilter families, hooks, protocols, subsystems, and related packet-processing facilities.

The following kernel options enable Netfilter components used by [base chain declaration](https://wiki.gentoo.org/wiki/Nftables/Configuration/Chain#Base_chain_declaration):

### Kernel configurations, sorted by CONFIG\_

Following tables has mapped protocol-family/chain-type/hook-name to kernel configuration items:

| CONFIG\_SYMBOL | Component | Supports protocol family/ chain type/ chain hook name trigraphs | 
|---|---|---|
| CONFIG\_BRIDGE\_NETFILTER | bridge packet filtering | bridge/filter/prerouting bridge/filter/input bridge/filter/forward bridge/filter/output bridge/filter/postrouting | 
| CONFIG\_NF\_DEFRAG\_IPV4 | IPv4 packet defragmentation | auto-included by CONFIG\_NF\_CONNTRACK | 
| CONFIG\_NF\_CONNTRACK | connection tracking | inet/filter/prerouting inet/filter/input inet/filter/forward inet/filter/output inet/filter/postrouting ip/filter/prerouting ip/filter/input ip/filter/forward ip/filter/output ip/filter/postrouting ip6/filter/prerouting ip6/filter/input ip6/filter/forward ip6/filter/output ip6/filter/postrouting | 
| CONFIG\_NF\_NAT | network address translation | inet/nat/prerouting inet/nat/input inet/nat/output inet/nat/postrouting ip/nat/prerouting ip/nat/input ip/nat/output ip/nat/postrouting ip6/nat/prerouting ip6/nat/input ip6/nat/output ip6/nat/postrouting | 

### Kernel configurations, sorted by family/type/hook

The following gives you the required kernel CONFIG\_OPTIONS if you want a specific protocol-family/chain-type/hook-name:

| protocol family/ chain type/ chain hook name | Required CONFIG\_SYMBOL | Description | 
|---|---|---|
| netdev/filter/ingress | CONFIG\_NF\_TABLES\_NETDEV | nftables netdev family filtering at ingress | 
| bridge/filter/prerouting | CONFIG\_NF\_TABLES\_BRIDGE | native nftables bridge family filtering at prerouting | 
| arp/filter/input | CONFIG\_NF\_TABLES\_ARP | nftables ARP family filtering at input | 
| bridge/filter/forward | CONFIG\_NF\_TABLES\_BRIDGE | native nftables bridge family filtering at forward | 
| bridge/filter/postrouting | CONFIG\_NF\_TABLES\_BRIDGE | native nftables bridge family filtering at postrouting | 
| arp/filter/output | CONFIG\_NF\_TABLES\_ARP | nftables ARP family filtering at output | 
| netdev/filter/egress | CONFIG\_NF\_TABLES\_NETDEV | nftables netdev family filtering at egress | 

| family/ chain type/ hook name | Required CONFIG\_SYMBOL | Description | 
|---|---|---|
| netdev/filter/ingress | CONFIG\_NF\_TABLES\_NETDEV | netdev-family filtering at ingress | 



| family/ chain type/ hook name | Required CONFIG\_SYMBOL | Description | 
|---|---|---|
| inet/filter/ingress | CONFIG\_NF\_TABLES\_INET | inet family filtering at ingress | 
| inet/filter/prerouting | CONFIG\_NF\_TABLES\_INET | inet family filtering at prerouting | 
| inet/nat/prerouting | CONFIG\_NF\_TABLES\_INET | inet family NAT at prerouting | 
| inet/filter/input | CONFIG\_NF\_TABLES\_INET | inet family filtering at input | 
| inet/filter/forward | CONFIG\_NF\_TABLES\_INET | inet family filtering at forward | 
| inet/filter/output | CONFIG\_NF\_TABLES\_INET | inet family filtering at output | 
| inet/filter/postrouting | CONFIG\_NF\_TABLES\_INET | inet family filtering at postrouting | 
| inet/nat/input | CONFIG\_NF\_TABLES\_INET | inet family NAT at input | 
| inet/nat/output | CONFIG\_NF\_TABLES\_INET | inet family NAT at output | 
| inet/nat/postrouting | CONFIG\_NF\_TABLES\_INET | inet family NAT at postrouting | 
| inet/route/output | CONFIG\_NF\_TABLES\_INET | inet family routing at output | 

| family/ chain type/ hook name | Required CONFIG\_SYMBOL | Description | 
|---|---|---|
| ip/filter/forward | CONFIG\_NF\_TABLES\_IPV4 | IPv4 filtering at forward | 
| ip/filter/input | CONFIG\_NF\_TABLES\_IPV4 | IPv4 filtering at input | 
| ip/filter/output | CONFIG\_NF\_TABLES\_IPV4 | IPv4 filtering at output | 
| ip/filter/postrouting | CONFIG\_NF\_TABLES\_IPV4 | IPv4 filtering at postrouting | 
| ip/filter/prerouting | CONFIG\_NF\_TABLES\_IPV4 | IPv4 filtering at prerouting | 
| ip/nat/input | CONFIG\_NF\_TABLES\_IPV4 | IPv4 NAT at input | 
| ip/nat/output | CONFIG\_NF\_TABLES\_IPV4 | IPv4 NAT at output | 
| ip/nat/postrouting | CONFIG\_NF\_TABLES\_IPV4 | IPv4 NAT at postrouting | 
| ip/nat/prerouting | CONFIG\_NF\_TABLES\_IPV4 | IPv4 NAT at prerouting | 
| ip/route/output | CONFIG\_NF\_TABLES\_IPV4 | IPv4 routing at output | 

| family/ chain type/ hook name | Required CONFIG\_SYMBOL | Description | 
|---|---|---|
| ip6/filter/forward | CONFIG\_NF\_TABLES\_IPV6 | IPv6 filtering at forward | 
| ip6/filter/input | CONFIG\_NF\_TABLES\_IPV6 | IPv6 filtering at input | 
| ip6/filter/output | CONFIG\_NF\_TABLES\_IPV6 | IPv6 filtering at output | 
| ip6/filter/postrouting | CONFIG\_NF\_TABLES\_IPV6 | IPv6 filtering at postrouting | 
| ip6/filter/prerouting | CONFIG\_NF\_TABLES\_IPV6 | IPv6 filtering at prerouting | 
| ip6/nat/input | CONFIG\_NF\_TABLES\_IPV6 | IPv6 NAT at input | 
| ip6/nat/output | CONFIG\_NF\_TABLES\_IPV6 | IPv6 NAT at output | 
| ip6/nat/postrouting | CONFIG\_NF\_TABLES\_IPV6 | IPv6 NAT at postrouting | 
| ip6/nat/prerouting | CONFIG\_NF\_TABLES\_IPV6 | IPv6 NAT at prerouting | 
| ip6/route/output | CONFIG\_NF\_TABLES\_IPV6 | IPv6 routing at output | 

| family/ chain type/ hook name | Required CONFIG\_SYMBOL | Description | 
|---|---|---|
| netdev/filter/egress | CONFIG\_NF\_TABLES\_NETDEV | netdev-family filtering at egress | 


See [Nftables kernel section](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables#Kernel) for final implementation of Nftables kernel configuration.

## Diagnostics

### Inspecting hooks

`root #``nft list ruleset``root #``nft -a list ruleset`
Look for **hook** in [base chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain#Base_chains) declarations.

### Inspecting connection tracking

Provided by [conntrack-tools](https://packages.gentoo.org/packages/conntrack-tools)

`root #``conntrack -L``root #``conntrack -S``root #``cat /proc/sys/net/netfilter/nf_conntrack_count`
### Inspecting packet paths

`root #``nft monitor trace``root #``tcpdump -ni any`
### Tracing packet processing

`root #``nft monitor trace``root #``conntrack -E``root #``cat /proc/net/nf_conntrack`

would call the subsection:



### Diagnosing Netlink errors

Netfilter userspace interfaces communicate with kernel subsystems through Netlink. Netlink errors can therefore occur before packet processing begins.

Useful commands:

`root #``nft -nn list ruleset`

First-line test of the nftables Netlink interface.

`root #``nft monitor`

Observes nftables Netlink events, useful for determining whether ruleset changes are actually reaching the kernel.

`root #``strace -e trace=sendmsg,recvmsg nft list ruleset`

Shows the Netlink system calls made by nft.

For the actual Netlink traffic:

`root #``ss -a -f netlink`

Shows Netlink sockets.  Look for nft:kernel.

For real Netlink messages between nft and kernel-level Netlink diagnostics, nlmon is the Netlink monitoring interface; tcpdump can be used via NETLINK\_NETFILTER.

`root #``modprobe nlmon``root #``ip link add nlmon0 type nlmon``root #``ip link set nlmon0 up``root #``tcpdump -ni nlmon0`


## See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules)
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter] — Linux kernel’s packet-filtering framework
- [Security Handbook](https://wiki.gentoo.org/wiki/Security_Handbook) — valuable guidance on Gentoo Linux security and cybersecurity in general.
- [Iptables](https://wiki.gentoo.org/wiki/Iptables) — a program used to configure and manage the kernel's netfilter modules.
