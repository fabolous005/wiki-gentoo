<!-- source: https://wiki.gentoo.org/wiki/QEMU/Networking/Bridge_with_Wifi_Routing | group: Gentoo Wiki (Main) | wiki-title: QEMU/Networking/Bridge with Wifi Routing -->
---
title: QEMU/Networking/Bridge with Wifi Routing
url: https://wiki.gentoo.org/wiki/QEMU/Networking/Bridge_with_Wifi_Routing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-31"
fingerprint: ae8deb5dd80949c1
license: CC BY-SA 4.0
---

# QEMU/Networking/Bridge with Wifi Routing

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

- add systemd specific commands
- add nftables
- remove iptables

When there are several VMs running and the user wants them to interact them with each other, he or she creates a bridge. As discussed in [QEMU/Options](https://wiki.gentoo.org/wiki/QEMU/Options) a ethernet connection may easily be bridged for the virtual machine(s). However, there are situations when the system is using a wireless network connection and still wants to interact with the VMs. In this case, the wireless interface cannot be part of the bridge easily. Also, another possibility is that the network connected to the ethernet is desired to have an independent state from the VMs and should not be included in the bridge. This guide deals with such conditions.

## Prerequisites

Emerge the packages. Dnsmasq is not mandatory to install but it can be very handy in case one does not want to setup the networking section of the guest and the host manually or the QEMU VM(s) depend on having a DHCP server being available.

## Creating a bridge interface

In this example, 2 QEMU guests or VM's will use the bridge`br0` interface. If there is only one VM then use only one TAP interface in the setup`tap0`. If more QEMU guests are running add one TAP interface per QEMU guest. Each QEMU guest has its own separate and dedicated TAP interface configured.

Edit the main network configuration file:

**`/etc/conf.d/net`**

```
#Configure a TAP interface for the first VM
tuntap_tap0="tap"
config_tap0=null
#Configure a TAP interface for the second VM
tuntap_tap1="tap"
config_tap1=null
#Configure the bridge interface and put the created TAP interfaces to the bridge
config_br0="172.16.100.1/24"           # Configure a static IP address on the bridge interface
bridge_br0="tap0 tap1"                 # If you have more VMs, add them.
rc_net_br0_need="net.tap0 net.tap1"    # If you have more VMs, add them.
```
Create the needed interfaces like shown in the instruction below:

`root #````
ln -s /etc/init.d/net.lo /etc/init.d/net.br0
```
`root #````
ln -s /etc/init.d/net.lo /etc/init.d/net.tap0
```
`root #````
ln -s /etc/init.d/net.lo /etc/init.d/net.tap1
```
Start the bridge interface:

`root #````
rc-service net.br0 start
```
## Setting up a DHCP server

Configure dnsmasq:

**`/etc/dnsmasq.conf`**

```
# Confiugre dnsmasq to listen on br0 interface for incomming requests
interface=br0
# Create a DHCP range for the QEMU guests
dhcp-range=172.16.100.50,172.16.100.150,12h
```
Start the service:

`root #````
rc-service dnsmasq start
```
## Network options for QEMU guests

The interface has been specified as `tap0` and a specific MAC address has been set. **Assigning a specific MAC address is important** this helps the VMs to be distinguishable. Set the network options for the first QEMU VM:

`-netdev tap,ifname=tap0,script=no,downscript=no,id=mynet0 -device virtio-net-device,netdev=mynet0,mac=00:12:35:56:78:9a`

Repeat the procedure for the second QEMU VM as above, but put it to its distinct `tap1` interface instead and assign a different MAC address:

`-netdev tap,ifname=tap1,script=no,downscript=no,id=mynet1 -device virtio-net-device,netdev=mynet1,mac=00:12:34:56:78:9a`

Here the network adapter `virtio-net-device` has been utilized for better performance. Make sure that the necessary steps have been followed to build the driver inside the VM and provide the support in the host. For more information refer to the necessary kernel configurations in [QEMU](https://wiki.gentoo.org/wiki/QEMU) and [QEMU/Linux\_guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) and [QEMU/Windows\_guest](https://wiki.gentoo.org/wiki/QEMU/Windows_guest).

## Configuring routing to the Wi-Fi interface

At this point both QEMU guests are working and should have an IP address aquired from dnsmasq DHCP server. If the VM's do not get the an IP address, verify the system logging using the `journalctl` command or review the /var/log/messages file. While the QEMU guest should have IP connection to each other, still they have no connection to the outside network, to the internet. Neither the host can interact with them. This can even be a desired behaviour if the user wants to have the VMs operate in an isolated network from the internet. If the ethernet interface `eth0` had been added to the bridge `br0`, the issue would have been solved, however, as it was stated at the beginning, maybe it is not want to include the `eth0` as it might be configured by another application or maybe the host system is connected through the wireless network. In the second case simply including the wireless interface in the bridge will not work.

There are several approaches here, however, these three lines configuration commands for [net-firewall/iptables](https://packages.gentoo.org/packages/net-firewall/iptables) will provide the simplest solution. What is more, the versatility is in the fact that it does not matter that what interface one wants to route to--it can be `wlan0` or `eth0` or whatever.

Enable IP forwarding on the system by creating following file:

**`/etc/sysctl.d/00_ipforwarding.conf`**

Reload the values to activate IP forwarding on the host:

`root #``sysctl --system`
In this example the bridge interface is `br0` and the host system is connected using the `wlan0` interface:

First allow forwading of IP traffic to get through the `wlan0` interface:

`root #````
iptables -A FORWARD -i br0 -o wlan0 -j ACCEPT
```
`root #````
iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
```
Second, let the system know that the known traffic can get back to the brige `br0` interface:

`root #````
iptables -A FORWARD -i wlan0 -o br0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```
Now the QEMU guests get IP access outside of the host, while the main host network interface `eth0` is not part of the bridge.
