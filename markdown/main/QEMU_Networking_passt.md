<!-- source: https://wiki.gentoo.org/wiki/QEMU/Networking/passt | group: Gentoo Wiki (Main) | wiki-title: QEMU/Networking/passt -->
---
title: QEMU/Networking/passt
url: https://wiki.gentoo.org/wiki/QEMU/Networking/passt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
fingerprint: ab49cc4e582b7ce0
license: CC BY-SA 4.0
---

# QEMU/Networking/passt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

passt is a userspace networking backend for QEMU.

### Configuration

passt can be used as the networking backend for a QEMU user-mode network interface:

**`gentoo.xml`**

**gentoo.xml**

```
<interface type='user'>
    <mac address='yo:ur:ma:ca:dd:re:ss'/>
    <model type='virtio'/>
    <backend type='passt'/>
</interface>
```
The \<backend type='passt'/> element selects passt rather than QEMU's default SLIRP backend.

### Port forwarding

Host connections can be forwarded to services on the guest. For example, this forwards TCP port 2222 on the host to port 22 on the guest:

**`gentoo.xml`**

**gentoo.xml**

```
<interface type='user'>
    <mac address='yo:ur:ma:ca:dd:re:ss'/>
    <model type='virtio'/>
    <backend type='passt'/>
    ...
    <portForward proto='tcp'>
        <range start='2222' to='22'/>
    </portForward>
    <alias name='net0'/>
    <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
    ...
</interface>
```
The guest's SSH service is then reachable from the host with:

`host-platform#``ssh -p 2222 user@127.0.0.1`
No guest address or route is required on the host; passt handles the forwarding.

### virt-manager

virt-manager can create a libvirt user-mode network interface, but its GUI does not expose all passt options.

For options not available in the GUI, edit the libvirt domain XML:

`host-platform#``virsh edit VM_NAME`
The VM must be shut down before changing hardware-related domain XML.

## See also

- [QEMU/Networking/User-mode](https://wiki.gentoo.org/wiki/QEMU/Networking/User-mode) — QEMU user-mode networking using the SLIRP networking backend
- [QEMU/Networking](https://wiki.gentoo.org/wiki/QEMU/Networking)
