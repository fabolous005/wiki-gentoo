<!-- source: https://wiki.gentoo.org/wiki/Ceph | group: Gentoo Wiki (Main) | wiki-title: Ceph -->
---
title: Ceph
url: https://wiki.gentoo.org/wiki/Ceph
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-25"
fingerprint: "2d98fe595481d7c4"
license: CC BY-SA 4.0
---

# Ceph

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Ceph** is a distributed object store and filesystem designed to provide excellent performance, reliability, and scalability. It is designed to have no single point of failure, and scale towards thousands of nodes participating in the distributed object store cluster.

## Use cases

Ceph provides three main storage features:

1. Within a Ceph cluster, pools are made available in which objects can be stored and retrieved. Gateways (such as the Rados Gateway) and other applications can use this to keep their data in a highly available manner.
2. A pool can be exposed as a file system. Each Ceph cluster can currently hold one file system which can be made accessible by Linux clients. This file system is POSIX compliant and can be used to build highly available NFS services (or directly use the mounted file system).
3. Rados block devices are stored in the Ceph cluster and can be used to build highly available virtualized infrastructure (for instance virtual guest images on a highly available cluster).

## Resources

In order to support a Ceph cluster, some insights in how Ceph operates as well as its various components is necessary. An [introductory guide to Ceph](https://wiki.gentoo.org/wiki/Ceph/Guide) is available on the wiki which not only explains how Ceph operates, but also introduces an example 3-host setup for Ceph.

For more in-depth information, please refer to the following resources.

| Concepts |  | 
|---|---|
| [Cluster](https://wiki.gentoo.org/wiki/Ceph/Cluster) | A Ceph cluster is the basic setup for any Ceph deployment. | 
| [Object Store Device (OSD)](https://wiki.gentoo.org/wiki/Ceph/Object_Store_Device) | A representation of a storage area in which Ceph uses to store objects. | 
| [Monitor (MON)](https://wiki.gentoo.org/wiki/Ceph/Monitor) | A quorum-supporting monitor to enable highly available operations even when some resources are unavailable. | 
| [Metadata Server (MDS)](https://wiki.gentoo.org/wiki/Ceph/Metadata_Server) | Metadata handling server used as the entrypoint for mounting Ceph's POSIX file system. | 
| [Rados Block Device](https://wiki.gentoo.org/wiki/Ceph/Rados_Block_Device) | A Ceph-supported block device. | 
| User guides |  | 
| [Installation](https://wiki.gentoo.org/wiki/Ceph/Installation) | Installing Ceph on a Gentoo Linux environment. | 
| [Administration](https://wiki.gentoo.org/wiki/Ceph/Administration) | Administering a Ceph cluster. |
