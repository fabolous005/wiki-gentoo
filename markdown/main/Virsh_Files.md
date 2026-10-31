<!-- source: https://wiki.gentoo.org/wiki/Virsh/Files | group: Gentoo Wiki (Main) | wiki-title: Virsh/Files -->
---
title: Virsh/Files
url: https://wiki.gentoo.org/wiki/Virsh/Files
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-30"
fingerprint: "8e991af973b3dac1"
license: CC BY-SA 4.0
---

# Virsh/Files

From Gentoo Wiki

\< [Virsh](https://wiki.gentoo.org/wiki/Virsh)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Files

This page details the files and directories used by [virsh](https://wiki.gentoo.org/wiki/Virsh) command.

| Path | Description | 
|---|---|
| /etc/libvirt/qemu/ | System-mode QEMU domain XML definitions. | 
| $HOME/.config/libvirt/qemu/ | Per-user QEMU domain XML definitions in session mode. | 
| /etc/libvirt/hooks/ | Administrator-defined scripts executed for libvirt daemon, domain, and network lifecycle events. | 
| /etc/libvirt/libvirt-admin.conf | Libvirt administration client configuration. | 
| /etc/libvirt/libvirt.conf | Libvirt client configuration. | 
| /etc/libvirt/libvirtd.conf | Monolithic libvirt daemon configuration. | 
| /etc/libvirt/libxl.conf | Xen libxl driver configuration. | 
| /etc/libvirt/libxl-lockd.conf | Xen libxl lock manager configuration. | 
| /etc/libvirt/libxl-sanlock.conf | Xen libxl sanlock configuration. | 
| /etc/libvirt/lxc.conf | LXC driver configuration. | 
| /etc/libvirt/nwfilter/ | Network filter XML definitions. | 
| /etc/libvirt/secrets/ | Secret driver configuration and persistent secret data, where configured. | 
| /etc/libvirt/storage/ | Storage driver configuration data, where configured. | 
| /etc/libvirt/virtlockd.conf | Virtual machine disk-lock manager configuration. | 
| /etc/libvirt/virtlogd.conf | Virtual machine log daemon configuration. | 
| /proc/cgroups | Available control groups and their hierarchy information. | 
| /proc/cpuinfo | Host processor information. | 
| /proc/modules | Loaded kernel modules. | 
| /proc/mounts | Host mount table. | 
| /proc/net/dev | Host network interface statistics. | 
| /proc/stat | Host CPU and system statistics. | 
| /proc/sys/net/ipv4/ip\_forward | IPv4 packet-forwarding setting. | 
| /proc/sys/net/ipv6/conf/all/forwarding | Global IPv6 packet-forwarding setting. | 
| /proc/sys/net/ipv6/conf/%s/%s | Per-interface IPv6 configuration; %s represents the interface and setting names. | 
| /proc/vz/vestat | OpenVZ virtual environment statistics; OpenVZ-specific. | 
| /sys/class/fc\_host/host0 | Fibre Channel host adapter attributes. | 
| /sys/class/fc\_remote\_ports | Fibre Channel remote-port attributes. | 
| /sys/class/scsi\_host | SCSI host adapter attributes. | 
| /sys/devices/system | Host system-device hierarchy, including CPU and NUMA topology. | 
| /sys/devices/system/%s/cpu/online | Online CPU state for a system device; %s represents the device name. | 
| /sys/devices/system/cpu/online | Set of online host CPUs. | 
| /sys/devices/system/node/node0/access1 | NUMA node memory-access attributes. | 
| /sys/devices/system/node/node0/meminfo | Memory statistics for NUMA node 0. | 
| /sys/devices/system/node/node0/memory\_side\_cache | Memory-side cache attributes for NUMA node 0. | 
| /sys/devices/system/node/online | Set of online NUMA nodes. | 
| /sys/fs/resctrl | Resource-control filesystem mount and control hierarchy. | 
| /sys/fs/resctrl/info/%s/num\_closids | Number of available CLOSIDs for a resource-control feature. | 
| /sys/fs/resctrl/%s/schemata | Resource allocation schemata for a control group. | 
| /sys/fs/resctrl/info/%s/min\_cbm\_bits | Minimum number of contiguous cache-mask bits. | 
| /sys/fs/resctrl/info/L3\_MON | Last-level cache monitoring capabilities. | 
| /sys/fs/resctrl/info/L3\_MON/num\_rmids | Number of available resource monitoring IDs. | 
| /sys/fs/resctrl/info/MB/bandwidth\_gran | Memory-bandwidth allocation granularity. | 
| /sys/fs/resctrl/info/MB/min\_bandwidth | Minimum memory-bandwidth allocation. | 
| /sys/fs/resctrl/info/MB/num\_closids | Number of available memory-bandwidth CLOSIDs. | 
| /sys/kernel/mm/ksm | Kernel Same-page Merging controls and status. | 
| /sys/kernel/mm/transparent\_hugepage/hpage\_pmd\_size | Transparent huge page size used for PMD mappings. | 
| /var/lib/libvirt/boot | Boot media and related files managed by libvirt. | 
| /var/lib/libvirt/dnsmasq | Runtime configuration and lease data for libvirt-managed dnsmasq instances. | 
| /var/lib/libvirt/images | Default system-mode directory for virtual machine disk images. | 
| /var/lib/libvirt/sanlock | Persistent sanlock state and lockspace data. | 

Some paths are driver-, kernel-, or configuration-dependent. The /proc/ and /sys/ entries are kernel interfaces, not libvirt-owned files. Paths containing %s represent variable path components.

## See Also

- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
