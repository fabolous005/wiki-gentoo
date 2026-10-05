<!-- source: https://wiki.gentoo.org/wiki/QEMU/Networking/KVM_IPv6_Support | group: Gentoo Wiki (Main) | wiki-title: QEMU/Networking/KVM IPv6 Support -->
---
title: QEMU/Networking/KVM IPv6 Support
url: https://wiki.gentoo.org/wiki/QEMU/Networking/KVM_IPv6_Support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: e2b5b0fa75b4bbc7
license: CC BY-SA 4.0
---

# QEMU/Networking/KVM IPv6 Support

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes IPv6 support in QEMU/KVM.

You have rented a whole server in a data center somewhere and are running a few Kernel Virtual Machines (KVMs). Your provider gives you a single IPv4 address and a IPv6 /64 subnet. Its a nuisance to use IPv4 with non standard ports on the KVMs and you are too parsimonious to invest in more IPv4 addresses, its a hobby server after all. You get a single IPv4 address and a whole IPv6 /64 subnet, since your provider cannot give you any less.

Lets put the KVMs onto IPv6 so that they have global scope IPv6 addresses.

This guide is written around the use of libvirtd

## Prerequsites

- A working Gentoo host with IPv6 support.
- One or more Gentoo KVM Guests with IPv6 support.
- A IPv6 /64 prefix, like 2001:db8:dead:beef::/64
- IPv6 working on the host.

IPv6 support means `USE="ipv6"` in /etc/portage/make.conf, kernel IPv6 support and packages like [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2) installed.

Other than iproute2, this is the Gentoo default.

## Getting started

On the hosts and KVM:

`root #``emerge --ask sys-apps/iproute2`
Do check that it is built with `USE=ipv6`.  If emerge shows that its a rebuild it can be skipped...there is nothing new to compile.

## The host setup

Your KVM setup will have created an interface called virbr0.

The settings for this are stored in /etc/libvirt/qemu/networks/default.xml. You might want to backup this file before you start.

### Edit virbr0

Stop the network, so you can edit the settings file.  Don't even *think* of using `$EDITOR`.

`root #````
virsh net-destroy default
```
`root #````
virsh net-edit default
```
Add the following two lines above the existing closing

\</network>

tag.


Use your own /64 and make up your own prefix extension.

\<ip family='ipv6' address='2001:db8:dead:beef:fe::2' prefix='96'>
  \</ip>

virsh net-edit will syntax check your edit and complain loudly if you mess up. This adds an IPv6 address to virbr0.

If you want to use DHCP for IPv6 in your KVMs, you probably don't, you can add a range statement here too. The range must be part of the prefix being assigned to virbr0

```
  <ip family='ipv6' address='2001:db8:dead:beef:fe::2' prefix='96'>
     <dhcp>
       <range start='2001:db8:dead:beef:fe::1000' end='2001:db8:dead:beef:fe::2000' />
     </dhcp>
   </ip>
```
Restart the network:

`root #``virsh net-start default`
### Check virbr0

`root #` `ip link show virbr0````
2: enp5s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 74:56:3c:64:cf:26 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.45/24 brd 192.168.0.255 scope global dynamic noprefixroute enp5s0
       valid_lft 67816sec preferred_lft 57016sec
    inet6 fe80::ab3d:b093:a223:776a/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
    inet6 fe80::8c32:bb30:5827:8c34/64 scope link
       valid_lft forever preferred_lft forever
```
Notice that the interface virbr0 has aquired a /96 from your /64. That's as many IP addresses as there is in the entire IPv4 address space.

You also have a self assigned link local IP, where wwww:xxxx:yyyy:zzzz is related to the virbr0 MAC addr.

It's a really good idea not to use addresses from 2001:db8:dead:beef:fe::/96 outside of virbr0

## KVM setup

### IPv6 address and default route

The KVM IPv6 setup is static. You could install a IPv6 aware DHCP client but they tend to support all the IPv6 auto configuration. I have not tested any in a KVM, so its left as an exercise for the reader.

Choose an IPv6 address in the 2001:db8:dead:beef:fe::/96 prefix but not 2001:db8:dead:beef:fe::2 which is virbr0. There are plenty to choose from, and use it in your config\_eth0= statement as in the example below.

You can still use dhcpcd for IPv4 if you wish.

`root #``nano /etc/conf.d/net````
# make sure use use iproute2
modules="iproute2"
config_eth0="192.168.122.104/24
             2001:db8:dead:beef:fe::8001/96"
routes_eth0="default via 192.168.122.1
             default via fe80::wwww:xxxx:yyyy:zzzz"  
```
The default route is a bit harder. Its the virbr0 link address. You can discover that with the ping all routers multicast.

`root #``# ping6 ff02::2 -I eth0`
PING ff02::2(ff02::2) from fe80::wwww:xxxx:yyyy:zzzz eth0: 56 data bytes
64 bytes from fe80::wwww:xxxx:yyyy:zzzz: icmp\_seq=1 ttl=64 time=0.020 ms
64 bytes from fe80::aaaa:bbbb:cccc:dddd: icmp\_seq=1 ttl=64 time=0.985 ms (DUP!)
...

In theory, you can use any router that responds. You should use virbr0 as its your next hop. That's the fe80::wwww:xxxx:yyyy:zzzz response.

When you are done restart the eth0 interface:

`root #``# /etc/init.d/net.eth0 restart`
That should all work, test it.

### Testing

For IPv4, test with:

`root #``# ping google.com -c3`
PING google.com (173.194.116.101) 56(84) bytes of data.
64 bytes from fra02s27-in-f5.1e100.net (173.194.116.101): icmp\_seq=1 ttl=55 time=22.8 ms
64 bytes from fra02s27-in-f5.1e100.net (173.194.116.101): icmp\_seq=2 ttl=55 time=29.2 ms
64 bytes from fra02s27-in-f5.1e100.net (173.194.116.101): icmp\_seq=3 ttl=55 time=26.3 ms
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 22.823/26.131/29.265/2.639 ms

For IPv6, test with

`root #``# ping6 google.com -c3`
PING google.com(fra02s27-in-x06.1e100.net) 56 data bytes
64 bytes from fra02s27-in-x06.1e100.net: icmp\_seq=1 ttl=55 time=37.3 ms
64 bytes from fra02s27-in-x06.1e100.net: icmp\_seq=2 ttl=55 time=26.7 ms
64 bytes from fra02s27-in-x06.1e100.net: icmp\_seq=3 ttl=55 time=36.4 ms
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 26.727/33.505/37.360/4.810 ms

That all looks good but its not quite working as it should.

## Nameservers

You only have IPv4 name servers in /etc/resolv.conf. That may not matter, they will still return IPv6 addresses for host that have them. However, one day, (not soon) IPv4 will be switched off and your KVM will not be able to reach any nameservers /etc/resolv.conf can contain up to three IPv4 nameservers and three IPv6 nameservers. Copy over the IPv6 nameservers from the hosts /etc/resolv.conf or use some of the public IPv6 nameservers. Google has one.

### Footnote for the curious

The 2001:db8::/32 prefix is reserved for use in documentation, its not supposed to be routable. You won't find any of my hosts there.
