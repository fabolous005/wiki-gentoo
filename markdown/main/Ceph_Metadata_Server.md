<!-- source: https://wiki.gentoo.org/wiki/Ceph/Metadata_Server | group: Gentoo Wiki (Main) | wiki-title: Ceph/Metadata Server -->
---
title: Ceph/Metadata Server
url: https://wiki.gentoo.org/wiki/Ceph/Metadata_Server
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: fdeb715b78824f95
license: CC BY-SA 4.0
---

# Ceph/Metadata Server

[Ceph](https://wiki.gentoo.org/wiki/Ceph)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Ceph metadata server is used to handle mounting a Ceph file system on a Linux client. This is an optional component as Ceph runs well without a file system (as plain object stores or through RBDs), but if the file system is wanted then the metadata server is necessary as well.

## Metadata server in a Ceph cluster

Ceph provides a MetaData Server (MDS) which provides a more traditional style of filesystem based on POSIX standards that translates into objects stored in the OSD pool. This is typically where a non-Linux platform can implement client support for Ceph. This can be shared via CIFS and NFS to non-Ceph and non-Linux based systems including Windows. This is also the way to use Ceph as a drop-in replacement for HADOOP. The filesystem component started to mature around the Dumpling release.

Ceph requires all of its servers to be able to see each other directly in the cluster. So this filesystem would also be the point where external systems would be able to see the content without having direct access to the Ceph Cluster. For performance reasons, the user may have all of the Ceph cluster participants using a dedicated network on faster hardware with isolated switches. The MDS server would then have multiple NICs to straddle the Ceph network and the outside world.

As of the Firefly release, there is only one active MDS server at a time. Other MDS servers run in a standby mode to quickly perform a failover when the active server goes down. The cluster will take about 30 seconds to determine whether the active MDS server has failed. This may appear to be a bottleneck for the cluster, but the MDS only does the mapping of POSIX file names to object ids. With an object id, a client then directly contacts the OSD servers to perform the necessary i/o of extents/shards.

Eventually Ceph will allow multiple active MDS servers, dividing the POSIX filesystem namespace with a mapping scheme that distributes the load.
