<!-- source: https://wiki.gentoo.org/wiki/Libvirt/Files | group: Gentoo Wiki (Main) | wiki-title: Libvirt/Files -->
---
title: Libvirt/Files
url: https://wiki.gentoo.org/wiki/Libvirt/Files
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-30"
fingerprint: "8f9a1a6166f7cf60"
license: CC BY-SA 4.0
---

# Libvirt/Files

From Gentoo Wiki

\< [Libvirt](https://wiki.gentoo.org/wiki/Libvirt)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page details the files and directories used by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt).

## Files and directories

| Path | Description | 
|---|---|
| /etc/libvirt/hooks/ | Custom scripts executed on libvirt daemon, domain, and network lifecycle events. | 
| /etc/libvirt/nwfilter/ | Network-filter XML definitions used to configure packet filtering for virtual machine interfaces. | 
| /etc/libvirt/secrets/ | Persistent secret-object XML definitions. | 
| /etc/libvirt/storage/ | Persistent storage-pool XML definitions, where configured. | 
| /proc/ | Process and kernel information exposed through the procfs virtual filesystem. | 
| /proc/sys/net/ipv4/ | IPv4 networking sysctl parameters, including forwarding, routing, and connection-related settings. | 
| /proc/sys/net/ipv6/conf/all/ | Global IPv6 interface configuration parameters applied across network interfaces. | 
| /proc/sys/net/ipv6/conf/%s/%s | Per-interface IPv6 configuration parameters; placeholders identify the network interface and parameter name. | 
| /run/libvirt/ | Runtime state, Unix-domain sockets, and transient files used by libvirt daemons. | 
| /sys/class/fc\_host/ | Fibre Channel host-adapter interfaces and attributes exported by the kernel. | 
| /sys/devices/system/cpu/ | CPU subsystem attributes, including CPU online state, topology, frequency scaling, and cache information. | 
| /sys/devices/system/node/node0/ | NUMA node attributes, including associated CPUs and memory information. | 
| /sys/fs/resctrl/info/%s/ | Resource-control information for hardware resources such as L2 cache, L3 cache, and memory bandwidth. | 
| /sys/fs/resctrl/info/MB/ | Memory-bandwidth allocation capabilities and configuration information exposed by the resctrl filesystem. | 
| /sys/kernel/mm/transparent\_hugepage/ | Transparent Huge Pages kernel controls, including enabled state, defragmentation, and allocation policy. | 
| /var/lib/libvirt/ | Persistent libvirt state and data maintained by the system-wide instance. | 
| /var/lib/libvirt/boot/ | Boot media and installation images downloaded or stored for virtual machine installation. | 
| /var/lib/libvirt/dnsmasq/ | Generated dnsmasq configuration and lease data for libvirt-managed virtual networks. | 
| /var/lib/libvirt/images/ | Default directory storage-pool location for virtual machine disk images. | 
| /var/lib/libvirt/isos/ | Optional directory for ISO installation media; not a standard libvirt directory. | 
| /var/lib/libvirt/secrets/ | Persistent secret-object data maintained by libvirt. | 
| /var/lib/libvirt/qemu/ | Persistent and auxiliary data associated with QEMU domains, including domain-specific files. | 

## See Also

- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [Virsh](https://wiki.gentoo.org/wiki/Virsh) — a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) management toolkit
- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
