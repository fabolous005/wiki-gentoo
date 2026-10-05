<!-- source: https://wiki.gentoo.org/wiki/Traffic_shaping | group: Gentoo Wiki (Main) | wiki-title: Traffic shaping -->
---
title: Traffic shaping
url: https://wiki.gentoo.org/wiki/Traffic_shaping
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-04"
fingerprint: "2d1f291be1b411c5"
license: CC BY-SA 4.0
---

# Traffic shaping

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article aims to give a basic foundation to start traffic shaping to improve responsiveness (ping, RTT) on internet links. Especially asymmetric links like DSL benefit from this. PING round trip time can be improved as much as 10x during heavy download/upload with this traffic shaping in place.

## Installation

### Prerequisites

You need to configure your Kernel with QoS support. You should add HTB, FQ\_CODEL and Ingress queuing disciplines as well as IFB (Intermediate Functional Block device) and U32 match support.

### Kernel

You need to activate the following kernel options:

If you have IPv6 support on your network or plan to use IPv6 tunnels, you may set IPv6 Protocol to \<M>. You should also enable Netfilter options, which enables the use of iptables firewall.

### Iproute2


| [+iptables](https://packages.gentoo.org/useflags/+iptables) | include support for iptables filtering | 
| [atm](https://packages.gentoo.org/useflags/atm) | Enable Asynchronous Transfer Mode protocol support | 
| [berkdb](https://packages.gentoo.org/useflags/berkdb) | build programs that use berkdb (just arpd) | 
| [bpf](https://packages.gentoo.org/useflags/bpf) | Use dev-libs/libbpf | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [elf](https://packages.gentoo.org/useflags/elf) | support loading eBPF programs from ELFs (e.g. LLVM's eBPF backend) | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | only install ip and tc programs, without eBPF support | 
| [nfs](https://packages.gentoo.org/useflags/nfs) | Support RPC lookups via net-libs/libtirpc in ss | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 

Install [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2):

`root #``emerge --ask iproute2`
The iproute2 package installs the powerful tool called "tc". It is used to control and modify queuing and filters on network links.

Lets begin with a little example:

`root #``tc qdisc show dev eth0`
By default, the pfifo\_fast queuing discipline is used by the Linux kernel. It should not be listed by "tc qdisc show".

## Theory

There are two modes of traffic shaping, INGRESS and EGRESS. INGRESS handles incoming traffic and EGRESS outgoing traffic. Linux does not support shaping/queuing on INGRESS, but only policing. Therefore IFB exists, which we can attach to the INGRESS queue while we can add any normal queuing like FQ\_CODEL as EGRESS queue on the IFB device.

The main reason for traffic shaping is that we cannot control the packet queues or prioritisation made by our ISP or in the external link. By limit our maximum bandwidth to 90% of our link speed we can make sure that any buffers (queues) that our ISP and our external link has will remain empty.

FQ\_CODEL is a queuing discipline that is based on AQM (Active Queue Management). It aims to create fair bandwidth for all flows, while attempting to minimise buffers (and hence delays).

The INGRESS shaping below works like this:

1. Create ingress filter on external interface
2. Copy all incoming data to the IFB device
3. Create an EGRESS qdisc on the IFB device and limit the bandwidth to 90%
4. Attach the FQ\_CODEL queuing discipline.

## Note

The original version of this page was wildly incorrect, in particular the ingress portion of the shaper didn't work at all. The protocol filter was ip only (no arp, no ipv6), the ifb0 device was not up, the default htb destination was to the direct, not fq\_codel qdisc. Also the htb "quantum" is different and has a different purpose than the the fq\_codel quantum - the htb quantum is there to lighten the cpu load of htb under higher rates, the fq\_codel quantum is there to optimize for different packet sizes....

It was good to know everything that can go wrong on a fq\_codel rate shaper... -- dtaht

I generally recomend treating the ecn idea gently on anything but strictly controlled networks (like data centers)

## Traffic Shaping Script

It is important that you start by setting your upload and download speeds to about 90% of your maximum link speed. After you get satisfying results, you can generally try increasing your upload speed to 95% or higher, and twiddle with download speed. On some links you can get away with 95% too, on some, 85% is safer. ADSL has special framing problems which require usage of the htb STAB parameter (not shown here).

You also need to load all modules beforehand.

`root #``modprobe ifb``root #``modprobe sch_fq_codel``root #``modprobe act_mirred`
#### Statistics

To see if the new shaping is activated you should run some heavy traffic through it, and check your work. This example uses netperf. It is helpful to run multiple instances in both directions to really exercise things.....

`user $``ping -c 70 somewhere > log &` `user $``netperf -l 60 -H somewhere -t TCP_MAERTS &``user $``netperf -l 60 -H somewhere -t TCP_STREAM &`
ping -c 25 -H 172.21.2.1 & # watch your ping times go by. Should generally only increase by 10ms max
root@ida:\~/gen# netperf -H 172.21.2.1 -t TCP\_MAERTS
MIGRATED TCP MAERTS TEST from 0.0.0.0 (0.0.0.0) port 0 AF\_INET to 172.21.2.1 () port 0 AF\_INET : demo
Recv   Send    Send                          
Socket Socket  Message  Elapsed              
Size   Size    Size     Time     Throughput  
bytes  bytes   bytes    secs.    10^6bits/sec  
 87380  65536  65536    10.00       6.78   
root@ida:\~/gen# netperf -H 172.21.2.1 -t TCP\_STREAM
MIGRATED TCP STREAM TEST from 0.0.0.0 (0.0.0.0) port 0 AF\_INET to 172.21.2.1 () port 0 AF\_INET : demo
Recv   Send    Send                          
Socket Socket  Message  Elapsed              
Size   Size    Size     Time     Throughput  
bytes  bytes   bytes    secs.    10^6bits/sec  
 87380  16384  16384    10.16       0.75

Then you can check your work:

`user $``tc -s qdisc show dev eth0`
qdisc htb 1: root refcnt 2 r2q 10 default 11 direct\_packets\_stat 0
 Sent 4211186 bytes 16165 pkt (dropped 0, overlimits 10147 requeues 0) 
 backlog 0b 0p requeues 0 
qdisc fq\_codel 801a: parent 1:11 limit 10240p flows 1024 quantum 300 target 5.0ms interval 100.0ms 
 Sent 4211186 bytes 16165 pkt (dropped 343, overlimits 0 requeues 0) 
 backlog 0b 0p requeues 0 
  maxpacket 1514 drop\_overlimit 0 new\_flow\_count 2234 ecn\_mark 0
  new\_flows\_len 0 old\_flows\_len 18
qdisc ingress ffff: parent ffff:fff1 ---------------- 
 Sent 24181003 bytes 19329 pkt (dropped 0, overlimits 0 requeues 0) 
 backlog 0b 0p requeues 0

`user $``tc -s filter show dev eth0 parent ffff:`
root@ida:\~/gen# tc -s filter show dev eth0 parent ffff:
filter protocol all pref 49152 u32 
filter protocol all pref 49152 u32 fh 800: ht divisor 1 
filter protocol all pref 49152 u32 fh 800::800 order 2048 key ht 800 bkt 0 terminal flowid ??? 
  match 00000000/00000000 at 0
	action order 1: mirred (Egress Redirect to device ifb0) stolen
 	index 13 ref 1 bind 1 installed 517 sec used 0 sec
 	Action statistics:
	Sent 34015624 bytes 29704 pkt (dropped 0, overlimits 0 requeues 0) 
	backlog 0b 0p requeues 0

`user $``tc -s -d class show dev ifb0`
qdisc htb 1: root refcnt 2 r2q 10 default 11 direct\_packets\_stat 1
 Sent 23852231 bytes 19336 pkt (dropped 0, overlimits 25453 requeues 0) 
 backlog 0b 0p requeues 0 
qdisc fq\_codel 8019: parent 1:11 limit 10240p flows 1024 quantum 300 target 5.0ms interval 100.0ms ecn 
 Sent 23852120 bytes 19335 pkt (dropped 463, overlimits 0 requeues 0) 
 backlog 0b 0p requeues 0 
  maxpacket 1514 drop\_overlimit 0 new\_flow\_count 2170 ecn\_mark 0
  new\_flows\_len 1 old\_flows\_len 1

## Some notes

If you see a maxpacket greater than 1514, you have some tso/gso/gro offload on on some interface. If you don't see ecn\_marking, one side or another of your hosts doesn't have ecn on, or it's disabled as per this script.



## Excplicit Congestion Notification (ECN)

You can enable ECN (Explicit Congestion Notification) since FQ\_CODEL will this to notify the internet hosts about congestions (bandwidth over limit), without actually dropping the packet.

`root #``sysctl -w net.ipv4.tcp_ecn=1`
However usage of ecn over the wide internet is kind of dubious, so this script only enables it on ingress, and in these examples, ecn\_mark=0 as it is not enabled on the other side of the tested connection.
