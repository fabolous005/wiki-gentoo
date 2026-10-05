<!-- source: https://wiki.gentoo.org/wiki/Ceph/Object_Store_Device | group: Gentoo Wiki (Main) | wiki-title: Ceph/Object Store Device -->
---
title: Ceph/Object Store Device
url: https://wiki.gentoo.org/wiki/Ceph/Object_Store_Device
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: df48c2519df50ffc
license: CC BY-SA 4.0
---

# Ceph/Object Store Device

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Ceph object store device represents a storage area for Ceph in which objects can be placed. When a user or application places objects inside a Ceph cluster, a [pool](https://wiki.gentoo.org/wiki/Ceph/Pool) is passed. Based on the pool and the object name, the correct *placement group* is deduced.

Each placement group has a distribution map (which Ceph calls the [CRUSH map](http://ceph.com/docs/master/rados/operations/crush-map/)) that informs Ceph how the object should be stored (which OSDs take part) whereas the pool itself contains information on how many replicas of the object should be made.

## Object store devices within the cluster

Two object stores mark the beginning of a Ceph cluster and they may be joined by potentially thousands more.  They sit on top of an existing filesystem such as [ext4](https://wiki.gentoo.org/wiki/Ext4), [xfs](https://wiki.gentoo.org/wiki/XFS), [zfs](https://wiki.gentoo.org/wiki/ZFS) or [btrfs](https://wiki.gentoo.org/wiki/Btrfs) and are created and maintained by an Object Store Device Daemon (OSD).  While the underlying filesystem may provide for redundancy, error detection and repair on its own, Ceph implements its own layer of error detection, recovery and n-way replication.  There is a trade-off between using a RAID 1, 5, 6 or 10 scheme with the underlying filesystem and then having a single OSD server versus having individual drives and multiple OSD servers.  The former provides a defense in depth strategy against data loss, but the latter has less of an impact on the cluster when a drive fails and requires replacement. The latter also potentially provides better performance than a software RAID or a filesystem built on top of a number of JBOD devices.

An OSD will take advantage of advanced features of the underlying filesystem such as Extents, Copy On Write (COW), and snapshotting. It can make extended use of the xattr feature to store metadata about an object, but this will often exceed the 4kb limitation of ext4 filesystems such that an alternative metadata store will be necessary. The ceph.com site documentation recommends either ext4 or xfs in production for OSDs, but it is obvious that zfs or btrfs will become better because of their ability to self-repair, snapshot and handle COW. Ultimately btrfs will become the preferred underlying filesystem for a Linux based OSD when the majority is satisfied that it is stable enough.

The task of the OSD is to handle the distribution of objects by Ceph across the cluster. The user can specify the number of copies of an object to be created and distributed amongst the OSDs. The default is 2 copies with a minimum of 1, but those values can be increased up to the number of OSDs that are implemented. Since this redundancy is on top of whatever may be provided the underlying RAID arrays, the cluster enjoys an added layer of protection that guards against catastrophic failure of a disk array. When a drive array fails, only the OSD or OSDs that make use of it are brought down.

Objects are broken down into extants, or shards, when distributed instead of having them treated as a single entity. In a 2-way replication scheme where there are more than 2 OSD servers, an object's shards will actually end up distributed across potentially all of the OSD servers.

## Mounting

The default location where an OSD daemon stores its objects is at /var/lib/ceph/osd/\<clustername>-\<osdid>. For instance, the OSD daemon with id 0 in a Ceph cluster called `ceph` will have its location at /var/lib/ceph/osd/ceph-0.

This is of course configurable through the ceph.conf configuration file.
