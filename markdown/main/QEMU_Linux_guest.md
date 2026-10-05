<!-- source: https://wiki.gentoo.org/wiki/QEMU/Linux_guest | group: Gentoo Wiki (Main) | wiki-title: QEMU/Linux guest -->
---
title: QEMU/Linux guest
url: https://wiki.gentoo.org/wiki/QEMU/Linux_guest
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-05"
fingerprint: "58b98c510d1aa3c4"
license: CC BY-SA 4.0
---

# QEMU/Linux guest

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



This article describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.

## Installation

#### Kernel

If you use [genkernel](https://wiki.gentoo.org/wiki/Genkernel) do not build the VirtIO drivers as modules, compile them into the kernel.

As an alternative, use these commands after emerging the kernel sources:

`(chroot) livecd /usr/src/linux #``make defconfig``(chroot) livecd /usr/src/linux #``make kvm_guest.config`
### Additional software

Guest Linux OS requires [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid) for proper shutdown handling by [libvirt](https://wiki.gentoo.org/wiki/Libvirt).

## Configuration

### Files

#### GRUB

For a minimal grub BIOS install:

`(chroot) livecd / #````
echo 'GRUB_PLATFORMS="pc"' >> /etc/portage/make.conf
```
`(chroot) livecd / #````
echo 'sys-boot/grub -fonts -nls -themes' > /etc/portage/package.use/grub
```
`(chroot) livecd / #``emerge --ask sys-boot/grub:2`
Optional: to make the guest work in the headless mode, add these lines:

**`/etc/default/grub`**

```
GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0"
GRUB_TERMINAL=console
```
and uncomment the following:

**`/etc/inittab`**

```
# SERIAL CONSOLES
s0:12345:respawn:/sbin/agetty -L 115200 ttyS0 vt100
```
Install grub on the guest disk:

`(chroot) livecd / #``grub-install /dev/vda`
Installing for i386-pc platform.
Installation finished. No error reported.

Configure grub for the kernel build earlier:

`(chroot) livecd / #``grub-mkconfig -o /boot/grub/grub.cfg`
Generating grub.cfg ...
Found linux image: /boot/vmlinuz-4.9.16-gentoo
done

### Host

To create a disk image for the virtual machine, run:

`user $``qemu-img create -f qcow2 Gentoo-VM.img 15G`
Download a minimal Gentoo LiveCD from [here](https://www.gentoo.org/downloads/).

Since QEMU requires a lot of [options](https://wiki.gentoo.org/wiki/QEMU/Options), it would be a good idea to put them into a shell script, e.g.:

**`start_Gentoo_VM.sh`**

```
#!/bin/bash
exec qemu-system-x86_64 -enable-kvm \
        -cpu host \
        -drive file=Gentoo-VM.img,if=virtio \
        -netdev user,id=vmnic,hostname=Gentoo-VM \
        -device virtio-net,netdev=vmnic \
        -device virtio-rng-pci \
        -m 512M \
        -smp 2 \
        -monitor stdio \
        -name "Gentoo VM" \
        "$@"
```
Change the path to your disk image Gentoo-VM.img in the script. You can add more options when calling the script. To boot the disk image, run:

`user $``./start_Gentoo_VM.sh -boot d -cdrom install-amd64-minimal-20120621.iso`
Install the guest per the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page). See the [guest section](https://wiki.gentoo.org#Guest) for optimum support. After the installation start the script without the additional options.

#### Headless server

If running on a headless server, you will need to tweak the settings a bit by removing `-monitor stdio` and adding `-nographic`:

**`start_Gentoo_VM.sh`**

```
#!/bin/bash
exec qemu-system-x86_64 -enable-kvm \
        -cpu host \
        -drive file=Gentoo-VM.img,if=virtio \
        -netdev user,id=vmnic,hostname=Gentoo-VM \
        -device virtio-net,netdev=vmnic \
        -device virtio-rng-pci \
        -m 512M \
        -smp 2 \
        -nographic \
        -name "Gentoo VM" \
        "$@"
```
and when prompted at boot time to select the kernel, you should input

**`start_Gentoo_VM.sh`**

```
 gentoo console=ttyS0
```
### Guest

#### Hard drive

The VirtIO hard drive is mapped to /dev/vda. Where the handbook refers to /dev/sdaX, always use /dev/vdaX when configuring the guest.



### Services

#### Defining a domain service

See **[virt-manager QEMU guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest)** (or [Libvirt/QEMU\_guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest)) for (un)defining a domain.

#### Starting a domain service

`root #``virsh start my_vm_domain_name`
#### Stopping a domain service

`root #``virsh destroy my_vm_domain_name`
This **virsh destroy** command is like pulling the power-cord on the computer's OS: very abrupt and curt.

#### Suspending a domain service

`root #``virsh shutdown my_vm_domain_name`
#### Tweaking VM settings

If you have made a change to the XML configuration file, KVM needs to reload before restarting its VM:

`root #``virsh define /etc/libvirt/qemu/my_vm_domain_name.xml`
Then restart the VM:

`root #``virsh start my_vm_domain_name`
One Gentoo user provides a bash script that is a front-end to the qemu-system-(arch)to directly which consolidates all the virsh subcommands which conveniently configure, start and stop a Linux (or any other) guest; check out [this QEMU init script](https://github.com/mm1ke/qemu-init).

## Advanced

### Expose images to LAN

Sometimes it is required that the image should get a proper IP address on the LAN network to allow other peers to access it.

Such a configuration is possible by using an existing [network bridge](https://wiki.gentoo.org/wiki/Network_bridge) and telling the machine to use it.

Assuming that there exists a bridge called `br0` on the machine, the following configuration exposes the image to the LAN.

**`start_Gentoo_VM.sh`**

```
#!/bin/bash
exec qemu-system-x86_64 -enable-kvm \
        -cpu host \
        -drive file=Gentoo-VM.img,if=virtio \
        -netdev bridge,id=net0,br=br0 \
        -device virtio-net-pci,netdev=net0 \
        -device virtio-rng-pci \
        -m 512M \
        -smp 2 \
        -nographic \
        -name "Gentoo VM" \
        "$@"
```
`root #``./start_Gentoo_VM.sh -boot d -cdrom install-amd64-minimal-20120621.iso`
### Optional post install guest IPv6 setup

For IPv6 networking see the [IPv6 subarticle](https://wiki.gentoo.org/wiki/QEMU/KVM_IPv6_Support).

### Mount guest image

To access the guest disk from the host (and e.g. chroot into the guest), use a "Network Block Device":

`root #` `modprobe nbd max_part=16` `root #` `qemu-nbd -c /dev/nbd0 Gentoo-VM.img` `root #` `mount /dev/nbd0p4 /mnt/gentoo`
Make any changes required and clean up:

`root #` `umount /mnt/gentoo` `root #` `qemu-nbd -d /dev/nbd0`
## Troubleshooting

### Boot hangs at syslog-ng

If the guest boots slow, or if the boot hangs on `* Checking your configfile (/etc/syslog-ng/syslog-ng.conf)` or there are syslog messages like `[    1.264763] random: dbus-deamon: uninitialized urandom read (12 bytes read)` or `[   12.667558] random: crng init done` (12 seconds after booting), this is likely due to the lack of entropy. A way to fix this is to enable the "VirtIO Random Number Generator support" (HW\_RANDOM\_VIRTIO=y) in the guest kernel and boot with the QEMU virtio-rng-pci device.

Another way to solve this is to enable "Trust the CPU manufacturer to initialize Linux's CRNG" (RANDOM\_TRUST\_CPU=y) in the guest kernel. However, [there are security concerns](https://forums.gentoo.org/viewtopic-t-1053732-postdays-0-postorder-asc-start-25.html) with this approach.

### VM shutdown problems

Host control scripts may send a `system_powerdown` message to the virtual machine in order to shut it down. For this to work properly, ACPI functionality on the guest is necessary. Also, ACPI daemon [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid) should be installed and running on the guest.

## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.
- [virsh](https://wiki.gentoo.org/wiki/Virsh) — a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) management toolkit
