<!-- source: https://wiki.gentoo.org/wiki/Ceph/Monitor | group: Gentoo Wiki (Main) | wiki-title: Ceph/Monitor -->
---
title: Ceph/Monitor
url: https://wiki.gentoo.org/wiki/Ceph/Monitor
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: efd9f2555c657f9b
license: CC BY-SA 4.0
---

# Ceph/Monitor

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Ceph monitor provides the entry towards the Ceph cluster for any operations on the cluster. Clients connect to monitor servers to obtain the CRUSH map (to know how the distribution of objects should be done), and whenever cluster management operations occur a majority of monitor servers need to acknowledge the operation. If not, then the operation is unsuccessful.

## Monitors within a ceph cluster

OSDs are watched over by Monitor Servers (MONs) which act as the coordinators for object traffic. The initial Ceph Cluster would consist of a MON and two OSD servers, and this is the example used in their documentation for a quick install. They also talk about an admin server, but this is only a system which is able to painlessly remote into the cluster members using ssh authorized\_keys. The admin server would be the system that the user has set up to run Chef, Puppet or other control systems that oversee the operation of the cluster.

A single MON would be a single point of failure for Ceph, so it is recommended that the Ceph Cluster be run with an odd number of MONs with a minimum number of 3 running to establish a quorum. For performance reasons, MONs should be put on a separate filesystem or device from OSDs because they tend to do a lot of fsyncs. Although they are typically shown as running on dedicated hosts, they can share a host with an OSD and often do in order to have enough MON servers for a decent quorum. MONs don't need a lot of storage space, so it is perfectly fine to have them run on the system drive, while the OSD takes over whatever large disk or array is in the server. When dedicating an SSD to handle OSD journals, the MON storage will only require another 2gb or so.

MONs coordinate object traffic by implementing the Controlled Replication Under Scalable Hashing (CRUSH) map. This is an algorithm that computes the locations for storing objects in the OSD pools. MONS also keep track of the map of daemons running the various flavors of Ceph server in the cluster. An "Initial Members" setting allows the user the specify the minimum number of MON servers that must be running in order to form a quorum. When there are not enough MONs to form a quorum, the Ceph cluster will stop processing until a quorum is re-established in order to avoid a "split-brain" situation.

The CRUSH map defaults to an algorithm that automatically computes where in the OSDs an object's shards should be placed, but it can be influenced by additional human specified policies. This way, a site administrator can sway CRUSH when making choices such as:

- use the sites faster OSDs by default
- divide OSDs into "hot" (SSD based), "normal" and "archival" (slow or tape backed) storage
- localize replication to OSDs sitting on the same switch or subnet
- prevent replication to OSDs on the same rack to avoid downtime when an entire RACK has a power failure

It is this spreading out of the load with the CRUSH map that allows Ceph to scale up to thousands of OSDs so easily while increasing performance as new stores are added. Because of the spreading, the bottleneck transfers from raw disk performance (about 100mb/sec for a SATA drive for example) to the bandwidth capacity of the network and switches.

There are a number of ways to work with the MON pool and Praxis database to monitor and administrate the cluster, but the most common is the `ceph` command.  This is a Python script that uses a number of Ceph supplied Python modules that use json to communicate with the MON pool.
