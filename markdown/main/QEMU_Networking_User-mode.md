<!-- source: https://wiki.gentoo.org/wiki/QEMU/Networking/User-mode | group: Gentoo Wiki (Main) | wiki-title: QEMU/Networking/User-mode -->
---
title: QEMU/Networking/User-mode
url: https://wiki.gentoo.org/wiki/QEMU/Networking/User-mode
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: a9c9c83dce3876d0
license: CC BY-SA 4.0
---

# QEMU/Networking/User-mode

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describes QEMU user-mode networking using the SLIRP networking backend by default.

QEMU user-mode networking provides network connectivity to a guest without requiring a host-side bridge, TAP device, or privileged network configuration.

The default user-mode networking backend is SLIRP. QEMU provides the guest with a private network and performs Network Address Translation (NAT) for connections originating from the guest.

The guest network is implemented entirely in QEMU's userspace networking stack. It therefore does not appear as a network interface or route in the host's routing table.



### Network configuration

The default QEMU user-mode network uses the 10.0.2.0/24 subnet. A typical guest configuration is:

10.0.2.15 — guest
   10.0.2.2 — default gateway
   10.0.2.3 — DNS server

The guest can establish connections to external networks through the QEMU NAT, but connections originating from the external network cannot normally reach the guest.

The 10.0.2.0/24 network is not added to the host's routing table. QEMU handles packets for the network internally.



### Port forwarding

QEMU can forward connections received on a host port to a port on the guest. The hostfwd parameter is supplied to the -netdev user backend.

For example, the following forwards TCP port 2222 on the host to TCP port 22 on the guest:

`host-platform$``qemu-system-x86_64 ... -netdev user,id=net0,hostfwd=tcp:127.0.0.1:2222-10.0.2.15:22`
The host can then connect to the guest's SSH service through the forwarded port:

`host-platform$``ssh -p 2222 user@127.0.0.1`
The SSH connection still uses the authentication methods configured by the guest's sshd. Port forwarding does not bypass SSH authentication.

#### Public-key authentication

Gentoo's OpenSSH configuration commonly uses public-key authentication. Consequently, the host must have a private key corresponding to a public key authorized for the guest account.

If an SSH key pair does not already exist on the host, create one:

`host-platform$``ssh-keygen -t ed25519`
The public key must then be installed in the guest user's \~/.ssh/authorized\_keys file.

For example, from the guest console:

`guest-vm$````
install -d -m 700 ~/.ssh cat >> ~/.ssh/authorized_keys ssh-ed25519 AAAA... host-key
```
^D

chmod 600 ~/.ssh/authorized_keys
The public key can then be used when connecting through the forwarded port:

`host-platform$``ssh -i ~/.ssh/id_ed25519 -p 2222 user@127.0.0.1`
If the private key is stored at the default location, ssh normally selects it automatically:

`host-platform$``ssh -p 2222 user@127.0.0.1`
If sshd is configured to require public-key authentication, attempting to connect without an authorized key results in an authentication failure. Changing PasswordAuthentication solely to make the initial connection work is unnecessary; the guest's authorized SSH key should instead be configured.

Go next to [verify SSH configuration](https://wiki.gentoo.org#Verify_SSH) section.



#### Password authentication

Password authentication may be enabled for temporary testing instead of configuring public-key authentication.

Set a password for the guest user, on the guest host:

`guest-vm-root#``passwd <user-name>`
Create an sshd\_config.d drop-in:

**`/etc/ssh/sshd_config.d/20-password-auth.conf`**

**/etc/ssh/sshd\_config.d/20-password-auth.conf**

`guest-vm-root#``ssh -t | echo "syntax ok."`


#### Restart SSH daemon

Restart sshd:

`guest-vm-root#``rc-service sshd restart`
The host can then connect using the guest user's password:

`guest-vm-root#``ssh -p 2222 user@127.0.0.1`


### Network traffic

Because QEMU SLIRP does not create a host-side network interface for the guest network, the 10.0.2.0/24 network cannot be captured on the host with tcpdump or Wireshark}}.

Network traffic can instead be captured from within the guest:

`guest #``tcpdump -ni enp1s0`
Traffic leaving the QEMU user-mode network can also be captured on the host-side physical or virtual interface through which the translated traffic is transmitted.

### virt-manager

When virt-manager uses a libvirt user-mode network interface, \<interface type='user'> normally uses QEMU's SLIRP networking backend.

For example:

**`gentoo.xml`**

**gentoo.xml**

```
<interface type='user'>
    <mac address='yo:ur:ma:ca:dd:re:ss'/>
    <model type='virtio'/>
    <alias name='net0'/>
    <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
</interface>
```

The QEMU hostfwd parameter is not represented by an element of the \<interface type='user'> configuration. It must be supplied as a QEMU command-line option.

For configuring user-mode networking with passt through virt-manager, see [QEMU/Networking/passt](https://wiki.gentoo.org/wiki/QEMU/Networking/passt).

## See also

- [QEMU/Networking](https://wiki.gentoo.org/wiki/QEMU/Networking)
- [QEMU/Networking/passt](https://wiki.gentoo.org/wiki/QEMU/Networking/passt) — userspace networking backend for QEMU
- [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
