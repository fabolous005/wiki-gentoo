<!-- source: https://wiki.gentoo.org/wiki/LXC/Network_examples | group: Gentoo Wiki (Main) | wiki-title: LXC/Network examples -->
---
title: LXC/Network examples
url: https://wiki.gentoo.org/wiki/LXC/Network_examples
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-22"
fingerprint: ae01c7311d3c71e1
license: CC BY-SA 4.0
---

# LXC/Network examples

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Example configuration

### veth in bridge mode

This is an example of an OpenRC host, and the article on [Network\_bridge#systemd](https://wiki.gentoo.org/wiki/Network_bridge#systemd) provides instructions on how to create the bridge interface for a systemd host. The network setup below creates a bridge (**lxcbr0**) that includes the **host interface** (enp9s0)  and sets 192.168.12.104 as the IP of the bridge (see [Network\_bridge#Single\_NIC\_bridge](https://wiki.gentoo.org/wiki/Network_bridge#Single_NIC_bridge)).

**`/etc/conf.d/net`**

Create the init script, start the lxcbr0 interface and, if needed, add the init script to the system's default run level.

`root #````
ln -s /etc/init.d/net.lo /etc/init.d/net.lxcbr0
```
`root #````
rc-service net.lxcbr0 start
```
`root #````
rc-update add net.lxcbr0 default
```
The **container's configuration file** defines 192.168.12.140 as the IP of the container, and 192.168.12.1 as its gateway. It also sets the **name of the veth pair** to Alpine0 to denote an Alpine Linux container.

**`/var/lib/lxc/Alpine/config`**

Finally, we need to **NAT the traffic that leaves the bridge**. This is an example for [nftables](https://wiki.gentoo.org/wiki/Nftables):

`root #``nft add rule nat postrouting ip saddr 192.168.12.0/24 oif "enp9s0" snat to 192.168.12.104`
After starting the lxc container:

`root #``lxc-start Alpine`
The brigde (**lxcbr0**) will contain the host (**enp9s0**) and container (**Alpine0**) interfaces:

`root #``brctl show`
bridge name	bridge id		STP enabled	interfaces
lxcbr0		8000.aadeddc2462c	no		Alpine0
							enp9s0

### Host configuration for VLANs inside the bridge which are connected to container's virtual Ethernet pair device

Let's assume that we have a host with enp2s0 device connected to provider LAN network which connects to the Internet (WAN) through it using ppp0 interface. We also have our private LAN network on the enp3s6 interface side. As long as we don't have many spare network interfaces and we also want some container's network isolation let's create another VLAN interface (enp3s6.1) on the host assigned to our private LAN network's interface enp3s6. Then we put it inside the bridge br0.1 as a port.

**`/etc/conf.d/net`**

```
# VLAN (802.1q)
vlans_enp3s6="1"
# bridge ports defined empty to avoid DHCP being run for their configuration (bridge will have 1 IP)
config_enp3s6_1="null"
# Bridge (802.1d)
# To add port to bridge dynamically when the interface comes up
bridge_add_enp3s6_1="br0.1"
# Give the bridge an IP address - a static one
config_br0_1="192.168.10.1/24"
# One of the ports of bridge require extra configuration - VLAN enp3s6.1 on enp3s6 interface - we need to depend on it like so
rc_net_br0_1_need="net.enp3s6"
# Note that it is important to include 'bridge_forward_delay_br0=0' and 'bridge_hello_time_br0=1000' in order
# to bring the interface up quickly. Other values will cause network packets
# to be dropped for the first 30 seconds after the bridge has become active.
# This in turn could prevent DHCP from working.
bridge_forward_delay_br0_1=0
bridge_hello_time_br0_1=1000
bridge_stp_state_br0_1=0
```
Then create a bridge interface, restart the enp3s6 interface to get enp3s6.1, and add the bridge interface to the system's default run level:

`root #````
cd /etc/init.d/
```
`root #````
ln -s net.lo net.br0.1
```
`root #````
cd ~
```
`root #````
rc-service net.enp3s6 restart
```
`root #````
rc-service net.br0.1 start
```
`root #``rc-update add net.br0.1`
You will have something like the following configuration:

`root #``ip addr````
3: enp3s6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether xx:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.1/24 brd 10.0.0.255 scope global enp3s6
       valid_lft forever preferred_lft forever
    inet6 xxxx::xxxx:xxxx:xxxx:xxxx/64 scope link 
       valid_lft forever preferred_lft forever
4: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether xx:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    inet 10.55.1.101/24 brd 10.52.1.255 scope global enp2s0
       valid_lft forever preferred_lft forever
    inet6 xxxx::xxxx:xxxx:xxxx:xxxx/64 scope link 
       valid_lft forever preferred_lft forever
5: enp3s6.1@enp3s6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br0.1 state UP group default 
    link/ether xx:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    inet6 xxxx::xxxx:xxxx:xxxx:xxxx/64 scope link 
       valid_lft forever preferred_lft forever
6: ppp0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1400 qdisc pfifo_fast state UNKNOWN group default qlen 3
    link/ppp 
    inet 76.54.32.101 peer 76.54.20.10/32 scope global ppp0
       valid_lft forever preferred_lft forever
8: br0.1: <BROADCAST,MULTICAST,PROMISC,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether xx:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.1/24 brd 192.168.10.255 scope global br0.1
       valid_lft forever preferred_lft forever
    inet6 xxxx::xxxx:xxxx:xxxx:xxxx/64 scope link 
       valid_lft forever preferred_lft forever
```
Now start the container with veth assigned to our bridge br0.1. You'll get another network interface on the host's side which looks like this:

`root #``ip addr````
...
10: vethB004H3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master br0.1 state UP group default qlen 1000
    link/ether xx:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    inet6 xxxx::xxxx:xxxx:xxxx:xxxx/64 scope link 
       valid_lft forever preferred_lft forever
```
Both our host's enp3s6.1 VLAN and container's virtual Ethernet pair device vethB004H3 are ports of our bridge br0.1:

`root #``brctl show````
bridge name     bridge id               STP enabled     interfaces
br0.1           8000.blablablabla       no              enp3s6.1
                                                        vethB004H3
```
#### Host configuration with NAT networking (nftables)

Let's now give internet access to container. We'll use [Nftables](https://wiki.gentoo.org/wiki/Nftables) for that (see next section for iptables). As long as we don't want container access to our private network LAN or our provider's LAN, we'll only provide access to the ppp0 WAN device to the container. Let's assume you already have a configuration on your host similar to the following [Nftables/Examples#Simple\_stateful\_router\_example](https://wiki.gentoo.org/wiki/Nftables/Examples#Simple_stateful_router_example). Then you'll have to add several rules to it.

**`/home/rt/scripts/nft.sh`**

```
#!/bin/bash
 
nft="/sbin/nft";
...
LAN_PRIVATE_LXC=192.168.10.1/24
export WAN=ppp0
...
#4LXC
${nft} add rule nat postrouting oifname ${WAN} ip saddr ${LAN_PRIVATE_LXC} masquerade;
...
#4LXC
${nft} add rule filter forward ip saddr ${LAN_PRIVATE_LXC} ct state new accept;
echo ACCEPT ${LAN_PRIVATE_LXC} ALL;
...
/etc/init.d/nftables save;
```
This will give internet access to the container. You can later create more isolated containers inside each separate bridge br0.X or connect various container's interfaces inside one br0.Y.

#### Host configuration with NAT networking (iptables)

For simple network access from container to outside world via NAT using iptables - we can masquerade all connections from our container network to outside world via device, that connected to internet.

Before this, use the already created device `net.br0.1` (see topic above):

**`/etc/lxc/lxc-usernet`**

and your guest container have something like this (for full listing of this file and configuration, please, read next section or unprivileged container section)

**`~/.config/lxc/guest.conf`**

First, enable masquerade for IPV4 :

`root #``echo 1 > /proc/sys/net/ipv4/ip_forward`
If you want to enable it at every boot time:

**`/etc/sysctl.conf`**

Configure NAT to accept and masquerade all connections from container to outside. Command use output device `enp5s0`. Please, set-up correct device name of your network/wifi network card (you can find it via ip link output)

`root #````
iptables -P FORWARD ACCEPT
```
`root #````
iptables -t nat -A POSTROUTING -s 192.168.10.1/24 -o enp5s0 -j MASQUERADE
```
To save this rule for feature boot-ups:

`root #``service iptables save`
Now, start the container and check networking:

`user $``ping 8.8.8.8`
### Guest configuration for a virtual Ethernet pair device connected by bridge

The guest network configuration resides in the guest's /etc/lxc/\<lxcname>/config file. To auto-generate it we will use distributive-specific template scripts, but we need some network configuration base for generation. We will use /etc/lxc/guest.conf as such base config file. Documentation for both of this files is accessible with: man lxc.conf.

The configuration should include the following network-related lines:

**`/etc/lxc/guest.conf`**

```
lxc.network.type = veth
lxc.network.flags = up
lxc.network.link = br0.1
lxc.network.name = eth0
#lxc.network.ipv4 = 192.168.10.101/24
#lxc.network.hwaddr = b6:65:81:93:cb:a0
```
If you are using DHCP inside the container to get an IP address, then run it once as shown. LXC will generate a random MAC address for the interface. To keep your DHCP server from getting confused, you will want to use that MAC address all the time. So find out what it is, and then uncomment the 'lxc.network.hwaddr' line and specify it there.

### Adjusting guest config of the container after using template script

If you got unworkable network inside the container (after using template script) then you always can adjust your guest configuration on the host using /etc/lxc/\<lxcname>/config file. For example:

**`/etc/lxc/alpha/config`**

```
 = veth
lxc.network.flags = up
lxc.network.link = br0.1
lxc.network.name = eth0
lxc.network.ipv4 = 192.168.10.101/24
lxc.network.ipv4.gateway = 192.168.10.1
```
You can also always change network config inside container by adjusting it's configuration files (after login into container), for example:

**`/etc/resolv.conf`**

```
 8.8.8.8
```
