<!-- source: https://wiki.gentoo.org/wiki/QEMU/Networking | group: Gentoo Wiki (Main) | wiki-title: QEMU/Networking -->
---
title: QEMU/Networking
url: https://wiki.gentoo.org/wiki/QEMU/Networking
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-29"
fingerprint: ee01882d8d39cdc2
license: CC BY-SA 4.0
---

# QEMU/Networking

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## QEMU networking

### MACVTAP

Example: one physical device, `eno1`.  Goal is to get two network devices, `macvtap0` for
the host configured with DHCP, `macvtap1` for the guest with configuration
inside the guest. If the guest uses DHCP configured inside the guest, it gets it from the same router where the host gets it from. 
For the router it looks like there are two different hosts. 
The new tap\* devices get the `kvm` group so qemu can run as user.

Communication between host and guest needs the cable plugged into `eno1` and the router
be up.

Configuration for systemd-networkd:

**`/etc/systemd/network/40-macvtap0.netdev`**

```
[NetDev]
Name=macvtap0
Kind=macvtap
[MACVTAP]
Mode=bridge
```
**`/etc/systemd/network/40-macvtap1.netdev`**

```
[NetDev]
Name=macvtap1
Kind=macvtap
[MACVTAP]
Mode=bridge
```
**`/etc/systemd/network/41-macvtap0.network`**

```
[Match]
Name=macvtap0
[Network]
DHCP=yes
```
**`/etc/systemd/network/41-macvtap1.network`**

```
[Match]
Name=macvtap1
[Link]
ActivationPolicy=up
```
**`/etc/systemd/network/50-eno1-macvtap.network`**

```
[Match]
Name=eno1
[Network]
MACVTAP=macvtap0
MACVTAP=macvtap1
```
Configruation for udev to set group owner and permission on the tap devices:

**`/etc/udev/rules.d/65-mykvm.rules`**

```
KERNEL=="tap*", GROUP="kvm", MODE="0660"
```
- `-nic tap,model=virtio-net-pci,mac=$(cat /sys/class/net/macvtap1/address),fd=3 3<>/dev/tap$(cat /sys/class/net/macvtap1/ifindex)`

### Network bridge

With this setup, we create a TAP interface (see above) and connect it to a virtual switch (the bridge).

Please first read about [network bridging](https://wiki.gentoo.org/wiki/Network_bridge) and [QEMU](https://wiki.gentoo.org/wiki/QEMU) about configuring kernel to support bridging.

#### OpenRC

Assuming a simple case with only one virtual machine with a tap0 net interface and only one net interface on the host with eth0.

**`/etc/conf.d/net`**

```
# Bridge setup
tuntap_tap0="tap"
config_tap0="null"
config_eth0="null"
bridge_br0="eth0 tap0"
# Bridge static config
config_br0="10.0.42.1 netmask 255.255.255.0"
routes_br0="default via 10.0.42.100"
bridge_forward_delay_br0=0
bridge_hello_time_br0=1000
depend_br0() {
    need net.eth0
    need net.tap0
}
```
Host and guest can be on the same subnet.
Configuration based on this forum post. [\[1\]](https://forums.gentoo.org/viewtopic-p-7565900.html#7565900)

#### systemd

Create the bridge:

**`/etc/systemd/network/vmbridge.netdev`**

```
[NetDev]
Name=vmbridge
Kind=bridge
```
Configure the bridge's address:

**`/etc/systemd/network/10-vmbridge.network`**

```
[Match]
Name=vmbridge
[Network]
Description=Your awesome VM bridge
Address=10.0.42.1/24
```
**`/your/path/to/qemu/stuff/addtobridge.sh`**

```
#!/usr/bin/env bash
# Bring the QEMU TAP device up and add it to the bridge
ip link set "$1" master vmbridge
ip link set "$1" up
```

After configuration of OpenRC or systemd, now we can run VM with the TAP networking option:

`-device virtio-net-pci,netdev=n0,mac=13:37:yourchoice:42 -netdev tap,id=n0,ifname=vmtap,script=/your/path/to/qemu/stuff/addtobridge.sh,downscript=no`

When the VM boots, the script will add the newly created device to the bridge. When you start another VM, both devices are in the bridge, and the VMs can communicate with each other.

### NAT

A more advanced networking concept is outlined below, which enables guest access to an external network and also works with both wired and wireless adapters on the host.  If desired, a DHCP server can also be set up on the host to allow for dynamic guest IP configurations.  There are many different tutorials available online to further understand these concepts.[\[2\]](https://blog.san-ss.com.ar/2016/04/setup-nat-network-for-qemu-macosx)[\[3\]](https://shanetomlinson.com/2009/bridging-a-wireless-card-in-kvmqemu/)[\[4\]](https://forums.gentoo.org/viewtopic-t-987790-start-0.html)[\[5\]](https://superuser.com/questions/694929/wireless-bridge-on-kvm-virtual-machine)[\[6\]](http://www.dedoimedo.com/computers/kvm-bridged.html)

![Qemu network diag.png](https://wiki.gentoo.org/images/b/ba/Qemu_network_diag.png)

#### Required packages

This example networking configuration needs some extra software installed:

`root #``emerge --ask net-firewall/iptables`
#### Host configuration

##### Creating TUN/TAP device

This allows the guest to communicate with the bridge. QEMU's default group is `kvm`, ensure that the correct group is given permissions to control the TAP. Enabling promiscuous mode (`promisc`) for the adapter might be unnecessary.

`root #````
ip tuntap add dev tap0 mode tap group kvm
```
`root #````
ip link set dev tap0 up promisc on
```
`root #````
ip addr add 0.0.0.0 dev tap0
```
##### Create network bridge

Creating a network bridge seems necessary, even if only 1 guest is configured. Create the bridge and add each TAP to it. Spanning tree protocol (`stp`) is disabled because there is only 1 bridge.[\[7\]](https://en.wikibooks.org/wiki/QEMU/Networking#TAP_interfaces)

`root #````
ip link add br0 type bridge
```
`root #````
ip link set br0 up
```
`root #````
ip link set tap0 master br0
```
`root #````
echo 0 > /sys/class/net/br0/bridge/stp_state
```
`root #````
ip addr add 10.0.1.1/24 dev br0
```
##### Packet forwarding and NAT

Allows for proper packet routing (be sure to replace `eth1` with an appropriate network interface name):

`root #````
sysctl net.ipv4.conf.tap0.proxy_arp=1
```
`root #````
sysctl net.ipv4.conf.eth1.proxy_arp=1
```
`root #````
sysctl net.ipv4.ip_forward=1
```
`root #````
iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE
```
`root #````
iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
```
`root #````
iptables -A FORWARD -i br0 -o eth1 -j ACCEPT
```
#### Guest configuration

The following should be added to the configuration:

or, with QEMU 2.12.0 or newer:

After starting the guest, the IP should be configured to be on the VLAN and the gateway should be the IP given to the bridge. The exact process will vary based on OS.

### IPv6

For IPv6 networking, see the [IPv6 subarticle](https://wiki.gentoo.org/wiki/QEMU/KVM_IPv6_Support).
