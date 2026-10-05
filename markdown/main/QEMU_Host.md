<!-- source: https://wiki.gentoo.org/wiki/QEMU/Host | group: Gentoo Wiki (Main) | wiki-title: QEMU/Host -->
---
title: QEMU/Host
url: https://wiki.gentoo.org/wiki/QEMU/Host
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-09"
fingerprint: "9999cd95d5223a0"
license: CC BY-SA 4.0
---

# QEMU/Host

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describes host-side configuration and management of [QEMU](https://wiki.gentoo.org/wiki/QEMU) virtual machines.

## Networking

### IPv6 setup

For IPv6 networking, see [QEMU/Networking/KVM IPv6 Support](https://wiki.gentoo.org/wiki/QEMU/Networking/KVM_IPv6_Support).

### User-mode networking

#### SSH access

With QEMU user-mode networking, the guest can access host services through the gateway address 10.0.2.2.

To makes the host's SSH service available to the guest while restricting the connection to the specified host user and destination network, use:

**`/etc/nftables/rules/main.nft`**

**Gentoo canonical nftables command file**

```
table ip qemu_nat {
  chain output {
    type nat hook output priority -100; policy accept;
    ...
    ip saddr 127.0.0.1 ip daddr 127.0.0.1 tcp dport 22
        meta skuid 1000 dnat to 192.168.1.any
    }
  chain postrouting {
    type nat hook postrouting priority 100; policy accept;
    ...
    ip saddr 127.0.0.1 ip daddr 192.168.1.any
        meta skuid 1000 masquerade
  }
...
}
table ip qemu_filter {
  chain output {
    type filter hook output priority 0; policy accept;
    ...
    ip saddr 192.168.1.1 ip daddr 192.168.1.0/24
        meta skuid 1000 drop
  }
...
}
```
By default, the rule restricts SSH access to the specified user ID 1000:

```
   meta skuid 1000
```
To allow any user on the host to access the SSH service, remove the meta skuid 1000 portion from the nftable rule.



The IP addresses, user ID, interface, and service port must be adjusted to match the host configuration.

Enable IPv4 routing of loopback-originated packets:

`host-platform-root#``sysctl -w net.ipv4.conf.eth0.route_localnet=1`
After modifying /etc/nftables/rules/main.nft, the nftables rules must be loaded.



## Storage

Various filesystem accesses between host and guest are:

- Host-to-guest - a virtualization mechanism shares filesystem access between host and running guest, like [Virtiofs](https://wiki.gentoo.org/wiki/Virtiofs).
- Offline guest - the guest is stopped; the host attaches or inspects its virtual disk using NBD, libguestfs, etc.
- Guest-side - the guest mounts and owns the filesystem; the host reaches files through a guest service such as [NFS](https://wiki.gentoo.org/wiki/NFS) or [SSH](https://wiki.gentoo.org/wiki/SSH).

### Host-to-guest filesystem access

Using a networking shim, host and active guest can access shared files.

#### Virtiofs

[Virtiofs](https://wiki.gentoo.org/wiki/Virtiofs) provides filesystem sharing between the host and active guest.

[app-emulation/virtiofsd](https://packages.gentoo.org/packages/app-emulation/virtiofsd) provides the userspace Virtiofs daemon. See [virtiofsd](https://wiki.gentoo.org/wiki/Virtiofsd#Installation) for installation instructions.

### Offline guest filesystem access

Inactive guest's disk images can be accessed using NBD, libguestfs, and other tools.

#### NBD

Linux Network Block Device (NBD) driver exposes a guest disk image to the host as a block device.

Mount the NBD block device then ls, {{c|cp||, {{c|dd} are some commands supported.

[app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) provides qemu-nbd for attaching QEMU disk images to the host's NBD subsystem.

#### libguestfs

Without mount, [libguestfs](https://wiki.gentoo.org/index.php?title=Libguestfs&action=edit&redlink=1) provides userspace tools for inspecting and modifying guest disk images directly from host.

[app-emulation/libguestfs](https://packages.gentoo.org/packages/app-emulation/libguestfs) provides guestfish and related utilities for offline guest filesystem access.

### Guest-side filesystem access

A running guest accesses its own filesystems normally and can provide file access to the host through network services.



#### NFS

[NFS](https://wiki.gentoo.org/wiki/NFS) can export a guest filesystem to the host.

#### SSH

[SSH](https://wiki.gentoo.org/wiki/SSH) provides guest file access through sftp and scp.

#### User-mode networking

Private host/guest network for services such as NFS and SSH is done using QEMU user-mode networking.

For a private network (not in host route table), see also [QEMU/Networking/User-mode](https://wiki.gentoo.org/wiki/QEMU/Networking/User-mode).



## Domain management

### Starting a domain

Start a domain:

`host-platform-root#``virsh start my_vm_domain_name`
### Shutting down a domain

Request a graceful shutdown of a domain:

`host-platform-root#``virsh shutdown my_vm_domain_name`
### Suspending a domain

Suspend a running domain:

`host-platform-root#``virsh suspend my_vm_domain_name`
A suspended domain remains allocated in memory but does not execute instructions until resumed.

### Resuming a domain

Resume a suspended domain:

`host-platform-root#``virsh resume my_vm_domain_name`
### Destroying a domain

Immediately terminate a running domain:

`host-platform-root#``virsh destroy my_vm_domain_name`
### Updating a domain configuration

After modifying a domain's XML configuration, redefine the domain:

`host-platform-root#``virsh define /etc/libvirt/qemu/my_vm_domain_name.xml`
Start the domain:

`host-platform-root#``virsh start my_vm_domain_name`
### Renaming a domain

A domain must be inactive and must not have snapshots before it can be renamed.

Rename the domain:

`host-platform-root#``virsh domrename OLD_NAME NEW_NAME`
Verify the new domain name:

`host-platform-root#``virsh list --all`
