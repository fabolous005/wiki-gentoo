<!-- source: https://wiki.gentoo.org/wiki/Fast_Virtio_VM | group: Gentoo Wiki (Main) | wiki-title: Fast Virtio VM -->
---
title: Fast Virtio VM
url: https://wiki.gentoo.org/wiki/Fast_Virtio_VM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-26"
fingerprint: "76932572518629b4"
license: CC BY-SA 4.0
---

# Fast Virtio VM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**2024-08-26**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=Fast_Virtio_VM&action=edit).

This page explains a way to build a blazing fast Gentoo VM under [KVM](https://wiki.gentoo.org/wiki/QEMU) using Virtio and mdev.

## Steps

### Use a fast computer

No really, this is probably the most important thing of all, don't expect a 2 second booting VM if you are on a P3 with a slow IDE disk. This tutorial is Build on a CentOS 6.2 KVM HypverVisor on a Dual QuadCore Xeon 2.4Ghz with 8 (v)CPU's and 2GB RAM assigned to the VM. The disk file is on an SSD (old Intel x25-M 80GB, so there is more to gain if you use a faster SSD).

### Use ext2

It has the fastest mount time.

### Kernel

Compile the kernel with:

- VirtIO support
- Hugetlb support

- NO usb support (Unless planning to passthrough USB from the host).
- NO HID support (see above).
- NO extended video/device drivers/filesystem drivers that will not be used. (duh)
- NO fuse support (Slow...)
- NO modules (Slower...)

### Kernel boot options

- noapic (APIC is for hardware management, and since we are in VM and the hardware is managed by our host, we don't need it).
- notsc (TSC is CPU clock synchronization, not needed in a VM).

Because they cannot be removed from the kernel itself, no need these as kernel boot options.

### Use a static IP

DHCP is slow.

### Use mdev

Replace [udev](https://wiki.gentoo.org/wiki/Udev) with [mdev](https://wiki.gentoo.org/wiki/Mdev) (BusyBox), udev is dead slow vs mdev.

### Configure mdev

Remove useless mdev devices (tun,tap, all sound, hd\*, sd\*, fd\*, md\*, grsec(\*), zap, dvb, v4l stuff)

### Prelink

Prelink all binaries:

`root #``emerge --ask prelink``root #``prelink -amRf`
### OpenRC

Use [rc-update](https://wiki.gentoo.org/wiki/OpenRC) to delete all non-essential daemons (git, consolefont, devfs, keymaps, modules, sysfs, swclock, staticroute)

### tmpfs

Use [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs) for /tmp and /var/run (will not necessarily speedup boot, but for startup services that /tmp heavily, it might help).

## Results

I created my own boot time init script, which echoes the current kernel uptime.

Please keep in mind that all of this is without \*any\* dirty tricks like initrd or [initramfs](https://wiki.gentoo.org/wiki/Initramfs). It's just static sequential boot.
Even using parallel start of services (thru `RC_PARALLEL="yes"`) would slow down the system.
