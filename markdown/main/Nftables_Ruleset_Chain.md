<!-- source: https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain | group: Gentoo Wiki (Main) | wiki-title: Nftables/Ruleset/Chain -->
---
title: Nftables/Ruleset/Chain
url: https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: fcb09d1ec74b8e0f
license: CC BY-SA 4.0
---

# Nftables/Ruleset/Chain

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Chain** contains a group of rules used to process network traffic.

Conceptual order of this page is chain > types > declaration > name/table scope > properties > lifecycle > relationships > semantics > examples



## Introduction

A chain is a named container for an ordered sequence of rules within a [Nftables](https://wiki.gentoo.org/wiki/Nftables) ruleset. A chain provides a distinct point in packet processing where rules are organized and evaluated.

This page describes chain types, declaration, properties, naming, reuse, lifecycle, relationships, and semantics.

## Chain types

A chain can be a:

- base chain, or
- regular chain

### Regular chains

The regular chain contains a group of rules.

### Base chains

Packet processing enters a base chain, through Netfilter [hook](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain#hook).

A base chain has additional properties; regular chains do not.

Properties that determines:

- which part of Linux packet flow to intercept.
- which base chains starts first.
- which network interface to restrict to.

### Differences

The primary difference is how a chain gets started.

Only a base chain can register with a Netfilter hook; similar to registering for a per-packet callback.

A regular chain is similar to a procedure/function; see {{Link|Nftables/Ruleset/Chain#Chaining|chaining\]\].

Omission of the property configuration in a base chain creates a regular chain.

## Chain declaration

A chain declaration creates a chain within a table.

The chain statement declares a chain by name.

Its name identifies the declared chain within its table.

Then defines its rules within a brace-delimited block.

**`/etc/nftables/rules/main.nft`**

**Chain declaration**

The declaration creates the chain; nothing enters that regular chain until another chain reaches it.

## Table scope

A chain belongs exactly to one table.

The table establishes the chain's namespace.

The same name can be used in different tables even within same family.

For example, these are three distinct chains, having same input\_accept\_http name:

**`/etc/nftables/rules/main.nft`**

**Accept all HTTP requests**

```
table inet lan {
    chain input_accept_http {
       tcp dport 80 accept
    }
}
table inet wan {
    chain input_accept_http {
       tcp dport 80 accept
    }
}
table inet dmz {
    chain input_accept_http {
       tcp dport 80 accept
    }
}
```
#### Chain naming

An unquoted chain name starts with an alphabetic character, can contain alphanumeric, /, |, \_, and .. Other characters and nftables keywords require quoting.

Chain names are otherwise user-defined.

Example chain names:

- blackhole\_logged
- input\_filter\_wan\_dirty
- dmz\_forward\_all\_http\_allow

#### Reserved words

Using a nftables reserved word as a chain name will result in validation error.

[nft](https://wiki.gentoo.org/wiki/Nft) will complain until the name gets quoted or renamed.

A condensed list of unhyphenated token names in nftables/src/parser\_bison.y source file is given below:

`user $``grep %token nftables/src/parser_bison.y | more`
vmap,include,define,redefine,undefine,fib,socket,transparent,wildcard,cgroupv2,tproxy,osf,synproxy,mss,wscale,typeof,hook,hooks,device,devices,table,tables,chain,chains,rule,rules,sets,set,element,map,maps,flowtable,handle,ruleset,trace,inet,netdev,add,update,replace,create,insert,delete,get,list,reset,flush,rename,describe,import,export,monitor,all,accept,drop,continue,jump,goto,return,to,constant,interval,dynamic,timeout,elements,expires,policy,memory,performance,size,flow,offload,meter,meters,flowtables,number,string,ll,nh,th,bridge,ether,saddr,daddr,type,vlan,id,cfi,dei,pcp,arp,htype,ptype,hlen,plen,operation,ip,version,hdrlength,dscp,ecn,length,ttl,protocol,checksum,ptr,value,lsrr,rr,ssrr,ra,icmp,code,seq,gateway,mtu,igmp,mrt,options,ip6,priority,flowlabel,nexthdr,hoplimit,icmpv6,ah,reserved,spi,esp,comp,flags,cpi,port,udp,sport,dport,udplite,csumcov,tcp,ackseq,doff,window,urgptr,option,echo,eol,mptcp,nop,sack,sack0,sack1,sack2,sack3,fastopen,md5sig,timestamp,count,left,right,tsval,tsecr,subtype,dccp,sctp,chunk,data,init,heartbeat,abort,shutdown,error,ecne,cwr,asconf,tsn,stream,ssn,ppid,vtag,rt,rt0,rt2,srh,addr,tag,sid,hbh,frag,reserved2,dst,mh,meta,mark,iif,iifname,iiftype,oif,oifname,oiftype,skuid,skgid,nftrace,rtclassid,ibriport,obriport,ibrname,obrname,pkttype,cpu,iifgroup,oifgroup,cgroup,time,classid,nexthop,ct,l3proto,zone,direction,event,expectation,expiration,helper,label,state,status,original,reply,counter,name,packets,bytes,avgpkt,counters,quotas,limits,synproxys,helpers,log,prefix,group,snaplen,level,limit,rate,burst,over,until,quota,used,secmark,secmarks,second,minute,hour,day,week,reject,with,icmpx,snat,dnat,masquerade,redirect,random,persistent,queue,num,bypass,fanout,dup,fwd,numgen,inc,mod,offset,jha

Quoted chain name can use a reserved word.

### Chain reuse

A chain belongs to one table and cannot be attached to nor reference from another table.

However, reuse of the same chain name and its rules can be duplicated into other tables, treated as separate objects.

### Chain and rules

A chain contains an ordered sequence of rules.

The chain provides the container and processing order.

The rules provide the packet-processing operations.

## Base chain properties

### type

The type property defines the type of processing performed by the base chain.

The valid types are:

- filter
- nat
- route

### hook

The hook property decides which part of the Linux Netfilter flow is attached to.

hook attaches the base chain to a specific Netfilter hook.

hook determines where in the Linux networking flow that this base chain attaches to and receive packets (see famous Linux Netfilter diagram).

### priority

Priority property specifies the placement within the hook ordering.

Multiple base chains attached to the same hook are ordered by
[priority](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain#priority).

Lower priority numeric values are traversed first.

Ordering of identical priorities remains undefined.

#### Priority example

Real world example of re-prioritizing a set of chains within a table would be:

**`/etc/nftables/rules/inet_filter.nft`**

**"custom chain before or after a conventional filtering chain"**

Noticed that main\_filter got listed first in the nftables commands? Lower priority ensures that early\_filter will be processed first.

Some commonly used priority numbers have a label, which can be used instead.

The available values depend on the address family:

| Priority label | Priority value | Address family | Hooks | 
|---|---|---|---|
| raw | -300 | ip, ip6, inet | all | 
| mangle | -150 | ip, ip6, inet | all | 
| dstnat | -100 | ip, ip6, inet | prerouting | 
| filter | 0 | ip, ip6, inet, arp, netdev | all | 
| security | 50 | ip, ip6, inet | all | 
| srcnat | 100 | ip, ip6, inet | postrouting | 

The bridge address family uses a separate set of standard priority values:

| Priority | Value | Hooks | 
|---|---|---|
| dstnat | -300 | prerouting | 
| filter | -200 | all | 
| out | 100 | output | 
| srcnat | 300 | postrouting | 

### policy

The policy property defines the default verdict for packets.

If no verdict at end of base chain, the default policy applies.

### device

The device property is only available for ingress and egress hook type.

The device property restricts the base chain to a specific network interface.

The property applies only where the selected hook supports device-specific attachment.



### Valid combinations

| Type | Purpose | Valid hooks | 
|---|---|---|
| filter | Packet filtering | input, output, forward, prerouting, and postrouting hooks. | 
| nat | Network Address Translation (NAT) | prerouting, input, output, and postrouting hooks. | 
| route | Rerouting after that first route lookup. | output hook. Modifying certain packet properties within a chain can cause a new route lookup. | 

The filter type is family-dependent; in addition to the above:

- inet additionally supports ingress.


The following families have their own set of filter hooks:

- netdev supports only ingress and egress.
- arp supports only input and output.
- bridge supports only prerouting, input, forward, output, and postrouting.

To drop all web traffic over WAN eth2 outbound:

**`/etc/nftables/rules/main.nft`**

**Base chain declaration**

## Regular chain declaration

To drop all SSH traffic inbound at WAN eth2 interface:

**`/etc/nftables/rules/main.nft`**

**Regular chain declaration**

The above declaration creates a regular chain.

Nothing enters the chain because no other chain reaches it.


To really drop all SSH traffic inbound and allow outbound web traffic at WAN eth2 interface:

**`/etc/nftables/rules/main.nft`**

**Both Base chain & Regular chain declaration**

## Chain lifecycle

Lifecycle of a chain is create, inspect, flush, rename, delete.

### Using a command line

`root #``nft create chain my_chain { tcp dport 80 accept }` `root #``nft list chain my_chain``root #``nft list chains``root #``nft flush chain my_chain``root #``nft rename chain my_chain your_chain``root #``nft delete chain my_chain`
### Using a command file

Feed all command files using this command

`root #``nft -f my_chain.nft`
**`my_chain.nft`**

**"Create a regular chain"**

**`my_chain.nft`**

**"Listing a regular chain"**

**`my_chain.nft`**

**"Flushing a regular chain"**

**`my_chain.nft`**

**"Renaming a regular chain"**

**`my_chain.nft`**

**"Deleting a regular chain"**

## Chain relationships

### Chaining

Explicit actions transition processing from one chain to another regular chain.

Valid explicit actions are: goto and jump.

Explicit action can only target regular chains within the same table.

No regular chain can call a base chain; base chain is entered only through its Netfilter hook.

### Abrupt pathways

jump saves its current position, then enters another regular chain.

return exits the current regular chain, ignoring its remaining rules.

When a saved return position exists, return resumes processing at that position.

Otherwise, return is finished and the base chain policy gets used.

goto enters another regular chain without saving a return position.

### End of Chain

End processing depends on the chain type and how the chain was entered.

### Premature End of Chain

return rule exits the middle of a regular chain, conditionally or explicitly.

Multiple base chains can use the same regular chain.

This allows separate interfaces to share a common regular chain.

Bonded interfaces, bridges, and backup WAN interfaces can each be treated as a group.

Groups can also share the same regular chain.

devices can be grouped together within a table.

Use one base chain for each device, have them all call the same regular chain.

**`/etc/nftables/rules/main.nft`**

**Binding network interfaces**

## Chain semantics

### Processing Order

Rules are processed in the order they occur within the chain.

Multiple base chains can be attached to the same Netfilter hook.

The [priority](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain#priority) property determines their processing order.

Lower numeric values are traversed first.

### Chain traversal

jump, goto, and return alter chain traversal.

See [Abrupt pathways](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain#Abrupt_pathways) for their behavior.

### Base chain behavior

A base chain is entered through its Netfilter hook.

When processing reaches the end of a base chain, its policy gets used.

## Examples

**`/etc/nftables/rules/main.nft`**

**Minimal regular chain declaration used in Nftables command file**

```
chain invalid_packets {
}
```
### Minimal base chain

**`/etc/nftables/rules/main.nft`**

**Minimal base chain declaration used in Nftables command file**

```
chain input {
    type filter hook input priority filter;
    policy accept;
    # your rule goes here
    # more rules can go here
}
```
## See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [Nftables/Ruleset/Chain] — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules)
- [Nftables/Configuration](https://wiki.gentoo.org/wiki/Nftables/Configuration)
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
