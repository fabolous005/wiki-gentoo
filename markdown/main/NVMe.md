<!-- source: https://wiki.gentoo.org/wiki/NVMe | group: Gentoo Wiki (Main) | wiki-title: NVMe -->
---
title: NVMe
url: https://wiki.gentoo.org/wiki/NVMe
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-27"
fingerprint: be11041f9ddbb9c8
license: CC BY-SA 4.0
---

# NVMe

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[NVM Express](https://en.wikipedia.org/wiki/NVM_Express) (**NVMe**) devices are flash memory chips connected to a system via the PCI-E bus (use four-lane max). They are among the fastest memory chips available on the market, faster than [Solid State Drives](https://wiki.gentoo.org/wiki/SSD) (SSD) connected over the SATA bus.

**NVM Express block device** (`CONFIG_BLK_DEV_NVME`) must be activated to gain NVMe device support:

```
Device Drivers --->
  NVME Support --->
    <*> NVM Express block device 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BLK_DEV_NVME</code> to find this item.
Devices will show up under /dev/nvme\*.

These are the defaults on other GNU/Linux distributions.

User space tools are available via:

`root #``emerge --ask sys-apps/nvme-cli`
## Configuration

[Partition tables](https://wiki.gentoo.org/wiki/Partition) and formatting can be performed the same as any other block device.

There are minor differences in the naming scheme for devices and partitions when compared to SATA devices.

NVMe partitions generally show a **p** before the partition number. NVMe devices also include namespace support, using a **n** before listing the namespace. Therefore the first device in the first namespace with one partition will be at the following location: /dev/nvme0n1p1. The device name is nvme0, in namespace 1, and partition 1.

[Hdparm](https://wiki.gentoo.org/wiki/Hdparm) can be used to get the raw read/write speed of a NVMe device. Passing the `-t` option instructs hdparm to perform  timings  of device reads, `-T` performs  timings  of  cache reads, and `--direct` bypasses the page cache and causes reads to go directly from the drive into hdparm's buffers in raw mode:

`root #``hdparm -tT --direct /dev/nvme0n1`
Since NVMe devices share the flash memory technology basis with common SSDs, the same performance and longevity considerations apply. For details consult the [SSD](https://wiki.gentoo.org/wiki/SSD) article.

Thanks to very fast random access times provided by NVMe devices, it is recommended to use the simplest kernel I/O scheduling strategy available[\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. Recent kernels name this strategy as *none*.

Name of the currently used I/O scheduler can be obtained from [sysfs](https://wiki.gentoo.org/wiki/Sysfs). For example, a /dev/nvme0n1 device using the *none* scheduler would look as:

`user $``cat /sys/block/nvme0n1/queue/scheduler`
none

In case of having multiple I/O schedulers available in the kernel:

`user $``cat /sys/block/nvme0n1/queue/scheduler`
\[none\] mq-deadline kyber bfq

It is possible to change the scheduler by writing name of the desired scheduler to the sysfs file:

`root #``echo "none" > /sys/block/nvme0n1/queue/scheduler`
This can also be achieved automatically by [udev](https://wiki.gentoo.org/wiki/Udev) rules:

**`/etc/udev/rules.d/60-ioschedulers.rules`**

**NVMe I/O scheduler rules file**

```
# Set scheduler for NVMe devices
ACTION=="add|change", KERNEL=="nvme[0-9]n[0-9]", ATTR{queue/scheduler}="none"
```
- [SSD](https://wiki.gentoo.org/wiki/SSD) — provides guidelines for basic maintenance, such as enabling discard/trim support, for **SSD**s ([Solid State Drives](https://en.wikipedia.org/wiki/Solid-state_drive)) on Linux.

## External resources

- [https://metebalci.com/blog/a-quick-tour-of-nvm-express-nvme](https://metebalci.com/blog/a-quick-tour-of-nvm-express-nvme) - An excellent article describing the differences in recent disk drive technology, but focusing on NVMe.
- [https://wiki.archlinux.org/index.php/NVMe](https://wiki.archlinux.org/index.php/NVMe)
- [Switching Scheduler — The Linux Kernel documentation](https://www.kernel.org/doc/html/latest/block/switching-sched.html)
- [BFQ, Multiqueue-Deadline, or Kyber? Performance Characterization of Linux Storage Schedulers in the NVMe Era](https://atlarge-research.com/pdfs/2024-io-schedulers.pdf)
- [A Systematic Configuration Space Exploration of the Linux Kyber I/O Scheduler](https://atlarge-research.com/pdfs/hotcloudperf24-kyber.pdf)
