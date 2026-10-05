<!-- source: https://wiki.gentoo.org/wiki/QEMU/Guest/Gentoo_Linux | group: Gentoo Wiki (Main) | wiki-title: QEMU/Guest/Gentoo Linux -->
---
title: QEMU/Guest/Gentoo Linux
url: https://wiki.gentoo.org/wiki/QEMU/Guest/Gentoo_Linux
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: "4f91983cc19a63e4"
license: CC BY-SA 4.0
---

# QEMU/Guest/Gentoo Linux

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

For installing Gentoo Linux as a QEMU guest, this page is an adaptation and a supplemental to Gentoo [Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) reference guide.

## Introduction

The [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) provides the general installation procedure. This page describes considerations specific to installing Gentoo Linux as a QEMU guest.



## Virtual hardware

Using physical hardware directly requires additional setup called [GPU passthrough](https://wiki.gentoo.org/wiki/GPU_passthrough_with_virt-manager,_QEMU,_and_KVM) or PCI passthrough.

The hardware available to the guest depends on the virtual devices configured when QEMU is started.

### BIOS/UEFI

QEMU can boot a guest using either traditional BIOS firmware or UEFI firmware.

When using UEFI, QEMU commonly uses [OVMF](https://wiki.gentoo.org/index.php?title=OVMF&action=edit&redlink=1) as its firmware. The
choice of firmware determines which boot procedure from the
[Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) applies.

### CPU

The CPU model and features depend on the QEMU configuration.

CPU microcode is managed by the host; the guest does not normally require CPU microcode packages.

### Graphics

When using a VirtIO GPU, the guest kernel requires CONFIG\_DRM\_VIRTIO\_GPU.

The appropriate guest kernel and userspace support depends on the graphics device selected for the virtual machine.

**`/etc/portage/package.use/00video_cards`**

**Package Usage of Virtual GPU**

```
 VIDEO_CARDS: -* virgl
```
#### GPU Passthrough

To allow guest to directly use the graphic card by hardware access, go to [GPU\_passthrough\_with\_virt-manager,\_QEMU,\_and\_KVM](https://wiki.gentoo.org/wiki/GPU_passthrough_with_virt-manager,_QEMU,_and_KVM).

### Firmware

VirtIO and QEMU-emulated devices generally do not require firmware files in the guest.

[sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) may be required when physical
hardware is assigned to the guest, such as through PCI passthrough.
In this case, the guest may require the same firmware and kernel
drivers as a physical Gentoo installation.

Physical hardware that can be assigned to a guest includes GPUs, network adapters, Wi-Fi adapters, NVMe controllers, USB controllers, and Host Bus Adapters (HBAs).

#### SR-IOV

Some physical devices support Single Root I/O Virtualization (SR-IOV), which allows a physical device to provide Virtual Functions (VFs) that can be assigned to guests.

The guest can use an assigned VF as virtualized hardware while the physical device remains managed by the host.

Firmware requirements depend on the hardware and its guest driver.

### Network

When using a VirtIO network device, the guest kernel requires CONFIG\_VIRTIO\_NET.

The interface name should be determined from within the guest rather than assumed. Obtain name of all network interfaces with:

`guest-vm-root#``ip link list`
Also, VM typically do not use WiFi, WiFi-extension, nor any analog modem.

**`/etc/portage/package.use/networkmanager`**

**Network Manager configuration file**

```
 -wifi -wext -modemmanager
```
#### IPv6 setup

For IPv6 networking setup, see [QEMU/Networking/KVM\_IPv6\_Support](https://wiki.gentoo.org/wiki/QEMU/Networking/KVM_IPv6_Support).



### Storage

When using a VirtIO block device, the device is normally named /dev/vda rather than /dev/sda.

For example, where the
[Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks) refers to
/dev/sda1, a guest using a VirtIO block device should use
/dev/vda1 instead.

[Virtiofs](https://wiki.gentoo.org/wiki/Virtiofs) provides filesystem sharing between the QEMU host and
guest. A directory exported by the host can be mounted in the guest
without using a network filesystem.

The guest kernel requires Virtiofs support:

 `[*] Virtio Filesystem support` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CONFIG_VIRTIO_FS</code> to find this item.
The QEMU host must export the filesystem and provide a Virtiofs device with a filesystem tag. The tag is used by the guest to identify the filesystem.

Mount the filesystem in the guest using the tag configured by the host:

`guest-vm-root#``mount -t virtiofs myfs /mnt`
Replace myfs with the filesystem tag configured on the QEMU host and /mnt with the desired mount point.

Host-side Virtiofs configuration is described in [QEMU/Host](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/QEMU/Host#Virtiofs).


Other QEMU storage configurations may result in different device
names.

### Serial port

QEMU can provide a virtual serial port to the guest.

A serial port can be used as a system console, particularly for headless virtual machines. The guest kernel and login service must be configured accordingly.

For a traditional QEMU serial port, the guest kernel requires serial console support.

For a VirtIO console, the guest kernel requires CONFIG\_VIRTIO\_CONSOLE.

#### Serial console

A serial console can be used to access a headless guest.

Add the following to /etc/default/grub:

**`/etc/default/grub`**

```
GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0"
GRUB_TERMINAL=console
```
For [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) systems, enable the serial console in
/etc/inittab:

**`/etc/inittab`**

```
# SERIAL CONSOLES
s0:12345:respawn:/sbin/agetty -L 115200 ttyS0 vt100
```
Regenerate the GRUB configuration:

`guest-vm-root#``grub-mkconfig -o /boot/grub/grub.cfg`
### Random number generator

QEMU can provide a virtual random number generator through a VirtIO RNG device.

The guest kernel requires CONFIG\_HW\_RANDOM\_VIRTIO when a VirtIO RNG device is used.

### Memory

The amount of memory available to the guest is determined by the QEMU virtual machine configuration.

#### Ballooning

QEMU can provide a VirtIO memory balloon device for dynamically adjusting the amount of memory assigned to the guest.

The guest kernel requires CONFIG\_VIRTIO\_BALLOON when VirtIO memory ballooning is used.

### Clock

QEMU provides a virtualized real-time clock and paravirtualized clock sources to the guest.

The available clock sources depend on the QEMU and guest kernel configuration.

The guest kernel may use the paravirtualized clock. See the
kernel configuration in the [Kernel](https://wiki.gentoo.org#Kernel) section.

## Installation

The [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) provides the general
installation procedure. The following sections describe
QEMU-specific considerations during installation.

### Installation media

The Gentoo installation media can be attached to the virtual machine as a virtual CD-ROM or other QEMU-supported boot device.

### Disk preparation

When following the disk preparation instructions in the
[Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks), use the
device name presented by QEMU.

For example, a VirtIO block device is normally /dev/vda.

### Kernel

The guest kernel must provide support for the virtual hardware selected for the virtual machine.

Common VirtIO support consists of:

\[\*\] VirtIO drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_VIRTIO\</code> to find this item.
 \[\*\] PCI driver for VirtIO devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_VIRTIO\_PCI\</code> to find this item.

Device-specific VirtIO support can be enabled as required:

\[\*\] VirtIO block driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_VIRTIO\_BLK\</code> to find this item.
 \[\*\] VirtIO network driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_VIRTIO\_NET\</code> to find this item.
 \[\*\] VirtIO console [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_VIRTIO\_CONSOLE\</code> to find this item.
 \[\*\] VirtIO filesystem [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_VIRTIO\_FS\</code> to find this item.
 \[\*\] VirtIO random number generator support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_HW\_RANDOM\_VIRTIO\</code> to find this item.
 \[\*\] VirtIO balloon driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_VIRTIO\_BALLOON\</code> to find this item.
 \[\*\] VirtIO GPU [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_DRM\_VIRTIO\_GPU\</code> to find this item.

For a traditional QEMU serial port:

\[\*\] 8250/16550 and compatible serial support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_SERIAL\_8250\</code> to find this item.
 \[\*\] Console on 8250/16550 and compatible serial port [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_SERIAL\_8250\_CONSOLE\</code> to find this item.

For the paravirtualized clock:

\[\*\] Paravirtualized guest support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_PARAVIRT\</code> to find this item.
 \[\*\] Paravirtualized clock [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_PARAVIRT\_CLOCK\</code> to find this item.

When a required device driver is provided as a kernel module, the module must be available before the device is required during boot.

#### Kernel configuration

When running Gentoo as a guest system, enable the following kernel options on the guest system (either built-in or as modules) to get proper support for the hardware emulated by VirtualBox:

**Bus options -> PCI (kernel 5.14 and before)**

 `[*] Mark VGA/VBE/EFI FB as generic system framebuffer` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CONFIG_SYSFB_SIMPLEFB</code> to find this item.
**Device drivers -> Firmware drivers (kernel 5.15 and after)**

 `[*] Mark VGA/VBE/EFI FB as generic system framebuffer` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CONFIG_SYSFB_SIMPLEFB</code> to find this item.
**Generic Framebuffer kernel 5.15 and later**

**Support for VirtualBox hardware**

### initramfs

If a driver required to access the guest root filesystem is provided as a kernel module, it must be available in the initramfs.

For example, a guest whose root filesystem resides on a VirtIO block device requires the corresponding VirtIO support to be available during early boot.

### Bootloader

The QEMU firmware configuration determines whether the guest uses the BIOS or UEFI boot procedure.

Follow the corresponding bootloader procedure in the
[Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page).

No special kernel command line config GRUB\_CMDLINE\_LINUX= is required.

#### GRUB

For a minimal grub BIOS install:

`guest-vm-root / #````
echo 'GRUB_PLATFORMS="pc"' >> /etc/portage/make.conf
```
`guest-vm-root / #````
echo 'sys-boot/grub -fonts -nls -themes' > /etc/portage/package.use/grub
```
`guest-vm-root / #``emerge --ask sys-boot/grub:2`
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

`guest-vm-root / #``grub-install /dev/vda`
Installing for i386-pc platform.
Installation finished. No error reported.

Configure grub for the kernel build earlier:

`guest-vm-root / #``grub-mkconfig -o /boot/grub/grub.cfg`
Generating grub.cfg ...
Found linux image: /boot/vmlinuz-4.9.16-gentoo
done



## Guest integration

### ACPI Power

Guest Linux OS requires [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid) for proper shutdown handling by [libvirt](https://wiki.gentoo.org/wiki/Libvirt).

Autostart acpid using rc-update or systemctl.

### SSH access

With QEMU user-mode networking, services provided by the host can be accessed through the gateway address 10.0.2.2.

For example, if the host provides an SSH service:

`guest-vm#``ssh 10.0.2.2`
Host-side configuration for exposing selected services to the guest
is described in [QEMU/Host - SSH access](https://wiki.gentoo.org/wiki/QEMU/Host#SSH_access).

### QEMU Guest Agent

The QEMU Guest Agent allows the QEMU host or management software to communicate with the guest.

The guest requires the QEMU Guest Agent and a VirtIO serial channel configured for guest-agent communication.

The guest kernel requires CONFIG\_VIRTIO\_CONSOLE.

#### Accessing host services

When using QEMU's default user-mode networking, the host is reachable from the guest through the gateway address 10.0.2.2.

For example, an SSH service exposed by the host can be accessed with:

`guest-vm#``ssh 10.0.2.2`
The host must be configured to permit access to the required service.
See [Accessing Host
Services](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/QEMU/Host#Accessing_Host_Services) for host-side configuration.

## Power management

### Suspend and hibernate

Guest suspend and hibernation are distinct from suspending or hibernating the physical host.

A QEMU virtual machine can instead be paused, resumed, saved, or restored by the host or QEMU management software.

Guest operating-system suspend and hibernation should therefore not be assumed to provide the same behavior as suspend and hibernation on physical hardware.

## Troubleshooting

### Boot hangs at syslog-ng

If the guest boots slowly or hangs at:

`* Checking your configfile (/etc/syslog-ng/syslog-ng.conf)`
kernel messages such as the following may indicate insufficient entropy:

`guest-vm-root#``dmesg | grep random`
random: dbus-daemon: uninitialized urandom read (12 bytes read)
random: crng init done

The latter message may occur several seconds after boot when the guest has insufficient entropy available during early boot.

Enable VirtIO Random Number Generator support in the guest kernel:

**CONFIG\_HW\_RANDOM\_VIRTIO**

 `[*] VirtIO Random Number Generator support` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CONFIG_HW_RANDOM_VIRTIO</code> to find this item.
The QEMU host must also provide a virtio-rng-pci device to the guest.


Alternatively, enable RANDOM\_TRUST\_CPU in the guest kernel:

**CONFIG\_RANDOM\_TRUST\_CPU**

 `[*] Trust the CPU manufacturer to initialize Linux's CRNG` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CONFIG_RANDOM_TRUST_CPU</code> to find this item.
### VM shutdown problems

Host control scripts may send a `system_powerdown` message to the virtual machine in order to shut it down. For this to work properly, ACPI functionality on the guest is necessary. Also, ACPI daemon [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid) should be installed and running on the guest.

## See also

- [Handbook](https://wiki.gentoo.org/wiki/Handbook) — an effort to centralize essential documentation for initial Gentoo installation and basic system administration.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.
- [QEMU/Guest/GUI approach](https://wiki.gentoo.org/wiki/QEMU/Guest/GUI_approach) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.
- [QEMU/Guest/CLI approach](https://wiki.gentoo.org/wiki/QEMU/Guest/CLI_approach) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.
- [Virsh](https://wiki.gentoo.org/wiki/Virsh) — a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) management toolkit
