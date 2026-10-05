<!-- source: https://wiki.gentoo.org/wiki/Ceph/Rados_Block_Device | group: Gentoo Wiki (Main) | wiki-title: Ceph/Rados Block Device -->
---
title: Ceph/Rados Block Device
url: https://wiki.gentoo.org/wiki/Ceph/Rados_Block_Device
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: "3f3bbc5b5fe1b788"
license: CC BY-SA 4.0
---

# Ceph/Rados Block Device

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

With a Rados Block Device, a regular block device file can be used by components such as virtualization infrastructure to host their raw data on a highly available distributed storage cluster.

## Rados block device in a Ceph cluster

Ceph provides a kernel module for the RADOS Block Device (RBD) and a librados library which libvirt and KVM can be linked against. This is essentially a virtual disk device that distributes its "blocks" across the OSDs in the Ceph cluster. An RBD provides the following capabilities:

- thin provisioning
- i/o striping and redundancy across the Cluster
- resizeable
- snapshot with revert capability
- directly useable as a KVM guest's disk device
- a variant of COW where a VM starts with a "golden image" which the VM diverges from as it operates
- Data Replication between datacenters starting with the Dumpling Release

A major selling point for the RBD is the fact that it can be used as a virtual machine's drive store in KVM. Because it spans the OSD server pool, the guest can be hot migrated between cluster CPUs by literally shutting the guest down on one CPU and booting it on another. Libvirt and Virt-Manager have provided this support for some time now, and it is probably one of the main reasons why RedHat (a major sponsor of QEMU/KVM, Libvirt and Virt-Manager) has acquired Inktank.
