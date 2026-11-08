<!-- source: https://wiki.gentoo.org/wiki/Libvirt/Networking | group: Gentoo Wiki (Main) | wiki-title: Libvirt/Networking -->
---
title: Libvirt/Networking
url: https://wiki.gentoo.org/wiki/Libvirt/Networking
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-07"
fingerprint: b7294a3dc83b30c0
license: CC BY-SA 4.0
---

# Libvirt/Networking

[Libvirt](https://wiki.gentoo.org/wiki/Libvirt)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes network device and network backend configuration in [QEMU](https://wiki.gentoo.org/wiki/QEMU).

## Network configuration

QEMU networking connects a virtual network interface card (NIC) to a network backend. The NIC provides the guest with a virtual network device; the backend provides the connection to the host network or another endpoint.

Configuring networking for QEMU provides two basic ways:

- -nic configures a NIC and its network backend together.
- -device and -netdev configure the NIC and backend separately.

### Network interface devices

The -device option adds a virtual network interface card to the guest.

The device model determines the network hardware presented to the guest. Examples include virtio-net-pci, e1000, and rtl8139.

List available network device models with:

`root #``qemu-system-x86_64 -device help`
### Network backends

The -netdev option configures a host-side network backend. Each backend is assigned an identifier with {{{1}}}, which connects it to a guest NIC through the NIC's {{{1}}} property.

Available backend types include:

- user - user-mode networking without requiring administrator privileges.
- tap - connects to a host TAP interface.
- bridge - connects a TAP interface to a host bridge.
- socket - connects QEMU instances through network sockets.
- passt - uses the external [net-misc/passt](https://packages.gentoo.org/packages/net-misc/passt) networking process.
- vhost-user - connects to a vhost-user network backend.
- l2tpv3 - transports Ethernet frames using L2TPv3.
- vde - connects to a Virtual Distributed Ethernet switch.

Backend availability depends on the QEMU build and host environment.

### Combined NIC and backend configuration

The -nic option is a shortcut for configuring a NIC and its network backend together.

For example:

`root #``qemu-system-x86_64 disk.img -nic user,model=e1000`
This configures an emulated Intel E1000 NIC connected to the user-mode network backend.

### Separate NIC and backend configuration

The following example configures the same arrangement using separate options:

`root #``qemu-system-x86_64 disk.img -netdev user,id=net0 -device e1000,netdev=net0`
The net0 identifier associates the guest NIC with the network backend.

### No network device

Use -nic none to disable the default network device configuration:

`root #``qemu-system-x86_64 disk.img -nic none`
## Legacy networking options

The older -net option provides legacy NIC and network backend configuration. New configurations should use -nic, -device, and -netdev.

## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.
- [Libvirt/Networking] — network device and network backend configuration in [QEMU](https://wiki.gentoo.org/wiki/QEMU).
- [QEMU/Networking/Open vSwitch network](https://wiki.gentoo.org/wiki/QEMU/Networking/Open_vSwitch_network)
- [QEMU/Networking/KVM IPv6 Support](https://wiki.gentoo.org/wiki/QEMU/Networking/KVM_IPv6_Support) — describes IPv6 support in QEMU/KVM.
- [QEMU/Networking/Bridge with Wifi Routing](https://wiki.gentoo.org/wiki/QEMU/Networking/Bridge_with_Wifi_Routing)
