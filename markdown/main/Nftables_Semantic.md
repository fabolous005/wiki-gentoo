<!-- source: https://wiki.gentoo.org/wiki/Nftables/Semantic | group: Gentoo Wiki (Main) | wiki-title: Nftables/Semantic -->
---
title: Nftables/Semantic
url: https://wiki.gentoo.org/wiki/Nftables/Semantic
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-07"
fingerprint: "1706e738dc15ced5"
license: CC BY-SA 4.0
---

# Nftables/Semantic

From Gentoo Wiki

\< [Nftables](https://wiki.gentoo.org/wiki/Nftables)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Nftables semantic

| Keyword | Packet access | Packet modification | Evaluation flow | Chain traversal | Verdict | Side effects | 
|---|---|---|---|---|---|---|
| accept | - | - | Terminate | - | Accept | - | 
| drop | - | - | Terminate | - | Drop | - | 
| continue | - | - | Next rule | - | - | - | 
| return | - | - | Terminate | Return | - | - | 
| jump | - | - | Terminate | Jump | - | - | 
| goto | - | - | Terminate | Goto | - | - | 
| counter | - | - | Continue | - | - | Counting | 
| last | Metadata | - | Continue | - | - | - | 
| log | Header; Metadata | - | Continue | - | - | Logging | 
| limit | - | - | Conditional | - | - | Rate limiting | 
| quota | - | - | Conditional | - | - | Quota | 
| reject | Header; Metadata | - | Terminate | - | Reject | Packet generation | 
| snat | Header | Header | Continue | - | - | NAT | 
| dnat | Header | Header | Continue | - | - | NAT | 
| masquerade | Header | Header | Continue | - | - | NAT | 
| redirect | Header | Header | Continue | - | - | NAT | 
| tproxy | Header; Metadata | Metadata | Continue | - | - | - | 
| queue | - | - | Terminate | - | Queue | Userspace | 
| dup | Header; Metadata | - | Continue | - | - | Packet generation | 
| fwd | Header; Metadata | Metadata | Continue | - | - | - | 
| set | Header; Payload; Metadata; Conntrack | - | Continue | - | - | - | 
| map | Header; Payload; Metadata; Conntrack | - | Continue | - | - | - | 
| meter | Header; Payload; Metadata; Conntrack | - | Conditional | - | - | Rate limiting | 
| synproxy | Header; Conntrack | Header | Continue | - | - | Conntrack | 
| ct | Conntrack | Conntrack | Continue | - | - | Conntrack | 
| meta | Metadata | Metadata | Continue | - | - | - | 
| payload | Header; Payload | Header; Payload | Continue | - | - | - | 
| notrack | - | Metadata | Continue | - | - | - | 
| flow | Header; Metadata | Metadata | Continue | - | - | Flow offload | 
| reset | Header | Header | Continue | - | - | - | 
| ct count | Conntrack | - | Conditional | - | - | Conntrack | 
| counter (named object) | - | - | Continue | - | - | Counting | 
| limit (named object) | - | - | Conditional | - | - | Rate limiting | 
| quota (named object) | - | - | Conditional | - | - | Quota | 
| synproxy (named object) | Header; Conntrack | Header | Continue | - | - | Conntrack | 
| tunnel (named object) | Header; Metadata | Metadata | Continue | - | - | - | 
| vmap | Header; Payload; Metadata; Conntrack | - | Conditional | Jump; Goto; Return | Accept; Drop | - | 
| add (set statement) | Header; Payload; Metadata; Conntrack | - | Continue | - | - | - | 
| update (set statement) | Header; Payload; Metadata; Conntrack | - | Continue | - | - | - | 
| delete (set statement) | Header; Payload; Metadata; Conntrack | - | Continue | - | - | - | 
| xt | - | - | N/A | - | - | - | 
| nftrace | - | Metadata | Continue | - | - | Tracing | 
| ct helper (named object) | Conntrack | Conntrack | Continue | - | - | Conntrack | 
| ct timeout (named object) | Conntrack | Conntrack | Continue | - | - | Conntrack | 
| ct expectation (named object) | Header; Conntrack | Conntrack | Continue | - | - | Conntrack | 
| ct count (named object) | Conntrack | - | Conditional | - | - | Conntrack | 
| secmark (named object) | - | Metadata | Continue | - | - | - | 
| numgen | - | - | Continue | - | - | - | 
| jhash | Header; Payload; Metadata; Conntrack | - | Continue | - | - | - | 
| symhash | Header; Metadata | - | Continue | - | - | - | 
| fib | Header; Metadata | - | Continue | - | - | - | 
| osf | Header | - | Conditional | - | - | - | 
| socket | Header; Metadata | - | Conditional | - | - | - | 
| ipsec | Metadata | - | Conditional | - | - | - | 
| tunnel | Metadata | - | Continue | - | - | - | 
| ll | Payload | Payload | Continue | - | - | - | 
| nh | Payload | Payload | Continue | - | - | - | 
| th | Payload | Payload | Continue | - | - | - | 
| hbh | Header | Header | Conditional | - | - | - | 
| frag | Header | Header | Conditional | - | - | - | 
| rt | Header; Metadata | Header | Conditional | - | - | - | 
| dst | Header | Header | Conditional | - | - | - | 
| mh | Header | Header | Conditional | - | - | - | 
| ether | Header | Header | Continue | - | - | - | 
| vlan | Header | Header | Continue | - | - | - | 
| arp | Header | Header | Continue | - | - | - | 
| ip | Header | Header | Continue | - | - | - | 
| icmp | Header | Header | Continue | - | - | - | 
| igmp | Header | Header | Continue | - | - | - | 
| ip6 | Header | Header | Continue | - | - | - | 
| icmpv6 | Header | Header | Continue | - | - | - | 
| ah | Header | Header | Continue | - | - | - | 
| esp | Header | Header | Continue | - | - | - | 
| comp | Header | Header | Continue | - | - | - | 
| udp | Header | Header | Continue | - | - | - | 
| udplite | Header | Header | Continue | - | - | - | 
| tcp | Header | Header | Continue | - | - | - | 
| dccp | Header | Header | Continue | - | - | - | 
| sctp | Header | Header | Continue | - | - | - | 
| vxlan | Header; Payload | - | Continue | - | - | - | 
| geneve | Header; Payload | - | Continue | - | - | - | 
| gre | Header; Payload | - | Continue | - | - | - | 
| gretap | Header; Payload | - | Continue | - | - | - | 

## Legend

Columns that can hold several values separate them with a semicolon (for example `Header; Payload`).

- **Packet access**: which kinds of data the keyword reads while it is evaluated. Each kind has its own column, and the cell is `Yes` or `No`.
  - **Access: Metadata**: reads packet metadata that is not part of the packet bytes, such as the input or output interface, mark, packet type, cgroup, or socket owner.
  - **Access: Header**: reads a protocol header field by name, such as `ip saddr` or `tcp dport`.
  - **Access: Payload**: reads raw packet bytes by base, offset and length (`ll`, `nh`, `th`).
  - **Access: Conntrack**: reads connection tracking state, such as `ct state`, `ct mark` or the tracked connection's helper.

- **Packet modification**: what the keyword changes. The values use the same kinds of data as Packet access, and a cell may combine them.
  - -: nothing is modified.
  - Header: rewrites a protocol header field.
  - Payload: rewrites raw packet bytes.
  - Metadata: sets packet metadata, such as the mark, the trace flag, or the socket assigned to the packet.
  - Conntrack: changes connection tracking state, such as the connection mark, helper, timeout policy or expectation.

- **Evaluation flow**: how the keyword affects the evaluation of the rule and the chain that contain it.
  - Continue: does not by itself stop evaluation. Evaluation proceeds to the next statement in the rule.
  - Conditional: can break the rule, for example when a limit is exceeded or when the object it looks at is absent. The rest of the rule is skipped and evaluation resumes at the next rule.
  - Next rule: ends the current rule and resumes at the next rule in the same chain (`continue`).
  - Terminate: ends evaluation of the chain by issuing a verdict or a chain traversal action.
  - N/A: no runtime effect, because the parser rejects the keyword (`xt`).

- **Chain traversal**: how the keyword moves evaluation between chains.
  - -: evaluation stays in the current chain.
  - Jump: evaluates another chain and returns to the statement after the jump when that chain ends or issues `return`.
  - Goto: evaluates another chain without leaving a return point. When that chain ends, the packet is handled according to the policy of the base chain.
  - Return: leaves the current chain. In a regular chain evaluation resumes in the caller, and in a base chain the chain policy applies.

- **Verdict**: the final disposition of the packet that the keyword can issue.
  - -: no packet verdict is issued.
  - Accept: ends evaluation in the current hook and lets the packet continue through the stack. Later base chains on the same hook may still see the packet.
  - Drop: discards the packet immediately.
  - Reject: discards the packet and sends an error to the sender (ICMP, ICMPv6 or TCP reset).
  - Queue: passes the packet to a userspace program, which returns the final verdict.

- **Side effects**: effects beyond the packet itself and the verdict.
  - -: no effect beyond the columns above.
  - Counting: updates packet and byte counters.
  - Logging: emits log, NFLOG, audit or trace output.
  - Tracing: marks the packet so that each rule it traverses is reported to `nft monitor trace`.
  - Rate limiting: matches or rejects packets according to a rate (token bucket).
  - Quota: matches according to an amount of traffic, in bytes.
  - Packet generation: creates a new packet, such as a reject reply or a duplicate.
  - NAT: creates or changes a NAT mapping in connection tracking.
  - Userspace: involves a userspace process.
  - Conntrack: changes state held by connection tracking, or limits the connection count.
  - Flow offload: adds the flow to a flowtable so that later packets bypass the normal ruleset.
