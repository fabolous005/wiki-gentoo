<!-- source: https://wiki.gentoo.org/wiki/QEMU/Networking/Open_vSwitch_network | group: Gentoo Wiki (Main) | wiki-title: QEMU/Networking/Open vSwitch network -->
---
title: QEMU/Networking/Open vSwitch network
url: https://wiki.gentoo.org/wiki/QEMU/Networking/Open_vSwitch_network
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-31"
fingerprint: "6201d0461598d8e6"
license: CC BY-SA 4.0
---

# QEMU/Networking/Open vSwitch network

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is designed to show the steps needed to create a basic and minimal **Open vSwitch** network to be used by a [QEMU](https://wiki.gentoo.org/wiki/QEMU) virtual machines(s) managed with **libvirt**. This type of network provides a much more powerful switching solution than the legacy NAT and bridge forwarding solutions.

## Context

There all quite a few different networking designs that can be created; but for the sake of keeping this simple we are going to work under the assumption that we are trying to configure a host OS on a computer that has 2 physical ethernet ports. The first port (eth0) is to be exclusively used by the host OS. The second port (eth1) is to be reserved for use by the vSwitch, which has the guest OS(s) attached to it. We also assume that the two physical ports are connected to a simple hardware ethernet switch, without anything advanced like VLAN tagging for instance.

## Background on OpenFlow and Open vSwitch

Open vSwitch has 3 service components to it: database, server and controller. The database daemon keeps track of interfaces that are created or modified, so that after a reboot they can be automatically re-created and configured. The server daemon actually sets up and manages the network, as a well as interfaces with the switching functionality within the kernel. The controller daemon may be the part that you are not very familiar with OpenFlow. In non-openflow switchers, there is the hardware that does all the work, and there is a controller. It is the "brains" of the switch that contains all the logic used to determine how packets are routed. With OpenFlow, the controller logic and packet routing functionality are decoupled from each other. This allows for the centralization of the controller logic into one controller that provides the logic for any number of switches. This provides the advantages of quicker deployment, cheaper hardware, and tight integration of both physical and virtual switchers. This also blurs the distinction between the roles of switcher and router. The controller daemon provides this functionality and can control multiple Open vSwitchers, as well as hardware switchers support OpenFlow. Or it can be turned off with the controller functionality provided by a controller elsewhere in the network.

For the scope of this article, we will just assume that the controller daemon is only being used by the single virtual switch we are creating.

## System Setup

### Kernel

You need to activate the following kernel options:

### libvirt

You will want to make sure [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) is built with support for the various virtual networking solutions:

**`/etc/portage/package.use`**

`root #``emerge --ask libvirt`
### Open vSwitch


| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [monitor](https://packages.gentoo.org/useflags/monitor) | Build the Python and GUI dependent monitor applications | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Add support for call stack unwinding and function name resolution | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [xdp](https://packages.gentoo.org/useflags/xdp) | Enable support for AF\_XDP high performance packet processing | 

Install [net-misc/openvswitch](https://packages.gentoo.org/packages/net-misc/openvswitch):

`root #``emerge --ask openvswitch`
We will be using the default settings, however feel free to view them.

- /etc/conf.d/ovsdb-server - Database daemon
- /etc/conf.d/ovs-vswitchd - vSwitch daemon
- /etc/conf.d/ovs-controller - Controller daemon

#### Startup and enable the daemons:

Database daemon:

`root #````
rc-update add ovsdb-server default
```
`root #``/etc/init.d/ovsdb-server start`
If the database daemon complains about the non-existing conf.db please make sure you did run the emerge --config command.

Controller daemon:

`root #````
rc-update add ovs-controller default
```
`root #``/etc/init.d/ovs-controller start`
vSwitch daemon (this also starts/enables the database daemon since it is a dependency):

`root #````
rc-update add ovs-vswitchd default
```
`root #``/etc/init.d/ovs-vswitchd start`
### Setup eth1

We don't want eth1 to be used by the host, and we also don't want it to be assigned an IP address.

Change the net config file:

**`/etc/conf.d/net`**

```
config_eth1="null"
```
If eth1 was configured differently before changing it, you might want to restart your system to apply the changes. (Restarting net.eth1 by itself is a mess)

### Setup vSwitch

First, create the bridge. We'll call it "vbr0".

`root #``ovs-vsctl add-br vbr0`
Next, we will add eth1 to this bridge.

`root #``ovs-vsctl add-port vbr0 eth1`
The final change we need to make is to assign the bridge a controller. Without it, the bridge doesn't know what to do with the packets.

`root #``ovs-vsctl set-controller vbr0 ptcp:`
One setting that is optional, but very highly recommended, is to turn on the spanning tree protocol.

`root #``ovs-vsctl set bridge vbr0 stp_enable=true`
### Setup libvirt

Recent versions of libvirt support this type of bridge, we just have to configure the virtual machine to use it.

Connect to the local QEMU manager:

`root #``virsh --connect qemu:///system`
Once logged into the virsh shell, get a listing of the virtual machines:

`root #``list --all`
Select the virtual machine you need to configure:

`root #``edit foo-vm`
This will open up the xml config in your default [text editor](https://wiki.gentoo.org/wiki/Text_editor). Find the section that defines the virtual OS's network interfaces. It will usually look like something similar to this:

There will probably be a few other tags inside the \<interface> tag, however, we are only showing the ones you will need to change. They will need to change to the following:

If you virtual machines is setup up with more than one network interface, you will need to edit each additional network interface tag accordingly.

Save the file and exit. You will return to the virsh shell, which should confirm that the virtual machine config has been updated.

Start the virtual machine:

`root #``start foo-vm`
### OpenRC

All the pieces are in place, and will be automatically restored on reboot. However there is nothing in place to automatically bring up the bridge interface and eth1. We will need to add an init service to do that for us.

Create the new file /etc/init.d/virt-net.vbr0:

**`/etc/init.d/virt-net.vbr0`**

```
#!/sbin/openrc-run
depend() {
        need ovs-controller ovs-vswitchd
        after *
}
start() {
        ebegin "Enabling virtual switch ports"
                ovs-ofctl mod-port vbr0 vbr0 up
                ovs-ofctl mod-port vbr0 eth1 up
        eend $?
}
```
Remember to set the correct mode bits:

`root #``chmod 755 /etc/init.d/virt-net.vbr0`
Next start and enable the service:

`root #````
rc-update add virt-net.vbr0
```
`root #``/etc/init.d/virt-net.vbr0 start`

At this point the virtual machine should be just like another computer on the network connected to the physical switch.

## Closing

There is a lot of functionality available with Open vSwitch that this article does not touch. You can setup VLAN tagging, QoS rules, create re-routing rules, block IP address, block ports, and much more. Feel free to reference online documentation and man pages if you are interested in adding complex functionality to the controller.
