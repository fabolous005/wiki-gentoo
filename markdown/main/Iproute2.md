<!-- source: https://wiki.gentoo.org/wiki/Iproute2 | group: Gentoo Wiki (Main) | wiki-title: Iproute2 -->
---
title: iproute2
url: https://wiki.gentoo.org/wiki/Iproute2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-03"
fingerprint: e691f959992ab983
license: CC BY-SA 4.0
---

# iproute2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**iproute2** is a tool developed to unify network interface configuration, routing, and tunneling for Linux systems.

iproute2 provides the ip command for this purpose.

## Installation

### USE flags


| [+iptables](https://packages.gentoo.org/useflags/+iptables) | include support for iptables filtering | 
| [atm](https://packages.gentoo.org/useflags/atm) | Enable Asynchronous Transfer Mode protocol support | 
| [berkdb](https://packages.gentoo.org/useflags/berkdb) | build programs that use berkdb (just arpd) | 
| [bpf](https://packages.gentoo.org/useflags/bpf) | Use dev-libs/libbpf | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [elf](https://packages.gentoo.org/useflags/elf) | support loading eBPF programs from ELFs (e.g. LLVM's eBPF backend) | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | only install ip and tc programs, without eBPF support | 
| [nfs](https://packages.gentoo.org/useflags/nfs) | Support RPC lookups via net-libs/libtirpc in ss | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 

### Emerge

[sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2) is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>).

`root #``emerge --ask sys-apps/iproute2`
## Usage

### IPv4 and IPv6 support

iproute2 supports both IP address families, IPv4 and IPv6. By default the ip command refers only to IPv4 protocol, explicit IP address family naming for IPv4 ip -4 is also possible. IPv6 uses the ip -6 command.

### Listing interfaces

Use ip link for displaying the interface link of all interfaces:

`user $``ip link````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN qlen 1000
    link/ether 00:22:68:13:da:7d brd ff:ff:ff:ff:ff:ff
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
    link/ether 00:1e:65:6b:ef:ca brd ff:ff:ff:ff:ff:ff
```
### Showing IP addressing

Either ip addr for displaying all IP information of all interfaces, or use ip address show for displaying the configuraton of a specific interface with assigned IP address:

`user $``ip addr show wlan0````
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
    link/ether 00:1e:65:6b:ef:ca brd ff:ff:ff:ff:ff:ff
    inet 192.0.2.10/24 brd 192.0.2.255 scope global wlan0
       valid_lft forever preferred_lft forever
```
### Activating interfaces

Interface links can be activated and deactivated (respectively) using the set subcommand:

`root #````
ip link set wlan0 down
```
`root #````
ip link set eth0 up
```
### Showing active interfaces

Display only active network interfaces. Notice, interface `wlan0` has been deactivated using previous example, and is not shown in the output:

`user $``ip link show up````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 00:22:68:13:da:7d brd ff:ff:ff:ff:ff:ff
```
### Configuring IP addressing

Removing IP addressing from the wlan0 interface:

`root #``ip addr del 192.0.2.10/24 dev wlan0`
Adding IP addressing to the eth0 interface:

`root #````
ip addr add 192.0.2.11/24 dev eth0
```
`root #````
ip -6 addr add 2001:db8::11/64 dev eth0
```
### Configure IP routing

Adding IP default route for both IP address families, IPv4 and IPv6:

`root #````
ip route add default via 192.0.2.1
```
`root #````
ip -6 route add default via fe80::1
```
Display the IPv4 routing table information:

`user $``ip route` default via 192.0.2.1 dev eth0  metric 2 
127.0.0.0/8 via 127.0.0.1 dev lo 
192.0.2.0/24 dev eth0  proto kernel  scope link  src 192.0.2.11

Display the IPv6 routing table information:

`user $``ip -6 route`
2001:db8::10/64 dev eth0  proto kernel  metric 2003  mtu 1492
fe80::/64 dev eth0  proto kernel  metric 256  pref medium
ff00::/8 dev eth0  metric 256  
default via fe80::1 dev eth0  metric 2003  mtu 1492

### Creating a VLAN interface

ip link can be used to create a new VLAN interface using an existing ethernet link:

`root #``ip link add link eth0 name vlan100 type vlan id 100``user $``ip link show vlan100````
4: vlan100@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 00:22:68:13:da:7d brd ff:ff:ff:ff:ff:ff
```
### Showing VLAN interfaces

By default ip link does not show the configured VLAN ID on the command line output. This VLAN ID can displayed using the  **-d** option. It is displayed after *id* on the line starting with *vlan protocol 802.1Q*:

`user $``ip -d link show vlan100````
4: vlan100@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 00:22:68:13:da:7d brd ff:ff:ff:ff:ff:ff promiscuity 0 allmulti 0 minmtu 0 maxmtu 65535 
    vlan protocol 802.1Q id 100 <REORDER_HDR> numtxqueues 1 numrxqueues 1 gso_max_size 65536 gso_max_segs 65535 tso_max_size 65536 tso_max_segs 65535 gro_max_size 65536 gso_ipv4_max_size 65536 gro_ipv4_max_size 65536 
```
### Showing interface statistics

Display the physical interface statistics using the *-s* option.

`user $``ip -s link show eth0``user $``ip -s link show eth0````
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:22:68:13:da:7d brd ff:ff:ff:ff:ff:ff
    RX:  bytes packets errors dropped  missed   mcast           
    1435359247 5262381      0       0       0   50212 
    TX:  bytes packets errors dropped carrier collsns           
     137919759 1505388      0       0       0       0 
    altname enp3s0f0
```
## iproute2 for net-tools swappers

The following table can be used as a reference point for substituting commands from the [sys-apps/net-tools](https://packages.gentoo.org/packages/sys-apps/net-tools) package for the equivalent [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2) commands:

| net-tools to iproute2 command equivalents |  | 
|---|---|
| net-tools | iproute2 | 
|---|---|
| ifconfig (interface list) | ip link | 
| ifconfig (ip configuration) | ip addr | 
| ifconfig (interface stats) | ip -s link | 
| route | ip route | 
| arp | ip neigh | 
| brctl addbr | ip link add ... type bridge | 
| brctl addif | ip link set master | 
| netstat | ss | 
| netstat -g | ip maddr | 
| netstat -i | ip -s link | 
| netstat -r | ip route | 
| iptunnel | ip tunnel | 
| ipmaddr | ip maddr | 
| tunctl | ip tuntap <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | 
| (none) for interface rename | ip link set dev *OLDNAME* name *NEWNAME* | 
| brctl | bridge <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> | 

## See also

- [Static routing](https://wiki.gentoo.org/wiki/Static_routing) — covers routing of the IP protocol in the Linux kernel.

## External resources

- [Why iproute2](http://lartc.org/howto/lartc.iproute2.html#LARTC.IPROUTE2.WHY)
- [Task-centered iproute2 user guide](https://baturin.org/docs/iproute2/)
- [Routing for multiple uplinks](http://lartc.org/howto/lartc.rpdb.multiple-links.html)
- [net-tools deprecation in favour of iproute2 - debian mailing list](https://lists.debian.org/debian-devel/2009/03/msg00780.html)
