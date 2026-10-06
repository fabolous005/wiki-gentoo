<!-- source: https://wiki.gentoo.org/wiki/Ceph/Guide | group: Gentoo Wiki (Main) | wiki-title: Ceph/Guide -->
---
title: Ceph/Guide
url: https://wiki.gentoo.org/wiki/Ceph/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-01"
fingerprint: "37a9d05579a587d1"
license: CC BY-SA 4.0
---

# Ceph/Guide

[Ceph](https://wiki.gentoo.org/wiki/Ceph)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[Ceph](http://ceph.com) is a distributed object store and filesystem designed to provide excellent performance, reliability, and scalability.  According to the [Ceph wikipedia entry](<https://en.wikipedia.org/wiki/Ceph_(software)#History>), the first stable release (Argonaut) was in 2012.  It arose from a doctoral dissertation by Sage Weil at the University of California, Santa Cruz. Significant funding came from the US DOE as the software has found early adoption in clusters in use at Lawrence Livermore, Los Alamos, and Sandia National Labs.  The main commercial backing for Ceph comes from a company founded by Weil ([Inktank](http://www.inktank.com/)) which was acquired by RedHat in April 2014.

The Floss Weekly podcast interviewed Sage Weil in 2013 for their [250th show](https://twit.tv/shows/floss-weekly/episodes/250).  The interview was done around the time that the "Cuttlefish" release was created.  One of the points of discussion was the need for data centers to handle disaster recovery, and Sage pointed out that starting with Dumpling, Ceph would provide for replication between data centers.  Another bit of trivia came out in the podcast: Sage Weil was one of the inventors of the [WebRing](https://en.wikipedia.org/wiki/Webring) concept in the early days of the World Wide Web.

Ceph's largest customer was (and probably still is) CERN which uses the object store for researcher virtual machines. Its size is on the order of Petabytes. This howto will show that it installs and runs well on cheap consumer hardware using as few as 3 machines and only hundreds of gigabytes or some number of Terabytes of disk capacity. An ex-military colleague of the author described how he used to string together a number of their standard issue Panasonic Toughbooks running a variant of BSD or Linux to run impromptu clusters out in the field. Ceph running on top of Gentoo would make an excellent reliable file store in just such a situation.

A standard SATA "spinning rust" hard drive will max out performance at about 100mb/sec under optimal conditions for writing. Ceph spreads out the writing to however many drives and hosts you give it to work with for storage. Even though standard settings have it create three different replicas of the data as it writes, the use of multiple drives and hosts will easily allow Ceph to blow past this speed limit.

## Overview

Ceph consists of six major components:

- Object Store Device Server
- Monitor Server
- Manager Server
- RADOS API using librados and support for a number of languages including Python and systems like libvirt
- Metadata Server providing a POSIX compliant filesystem that can be shared out to non-Linux platforms with NFS and/or Samba
- Kernel support for the RADOS block device and cephfs filesystem

### Object store device

Two object stores mark the beginning of a Ceph cluster and they may be joined by potentially thousands more.  In earlier releases of Ceph, they sit on top of an existing filesystem such as [ext4](https://wiki.gentoo.org/wiki/Ext4)
, [xfs](https://wiki.gentoo.org/wiki/XFS), [zfs](https://wiki.gentoo.org/wiki/ZFS) or [btrfs](https://wiki.gentoo.org/wiki/Btrfs) and are created and maintained by an Object Store Device Daemon (OSD).  While the underlying filesystem may provide for redundancy, error detection and repair on its own, Ceph implements its own layer of error detection, recovery and n-way replication.  There is a trade off between using a RAID1, 5, 6, or 10 scheme with the underlying filesystem and then having a single OSD server versus having individual drives and multiple OSD servers.  The former provides a defense in depth strategy against data loss, but the latter has less of an impact on the cluster when a drive fails and requires replacement.  The latter also potentially provides better performance than a software RAID or a filesystem built on top of a number of JBOD devices.

Inktank/Redhat used lessons learned to develop a new underlying filestore called Bluestore or BlueFS. This is starting to replace the other filesystems as the default for new cluster installations. The current version of this howto is being written as the author replaces his original ceph cluster based on btrfs with a completely new install based on BlueFS.

An OSD will take advantage of advanced features of the underlying filesystem such as Extents, Copy On Write (COW), and snapshotting. It can make extended use of the xattr feature to store metadata about an object, but this will often exceed the 4kb limitation of ext4 filesystems such that an alternative metadata store will be necessary. Up until the Luminous release, the ceph.com site documentation recommended either ext4 or xfs in production for OSDs, but it was obvious that zfs or btrfs would be better because of their ability to self-repair, snapshot and handle COW. BlueFS is a response to findings that zfs and btrfs did more than ceph needed and that something a bit more stripped down would buy extra performance. It is the default store as of version Luminous.

The task of the OSD is to handle the distribution of objects by Ceph across the cluster. The user can specify the number of copies of an object to be created and distributed amongst the OSDs. The default is 2 copies with a minimum of 1, but those values can be increased up to the number of OSDs that are implemented. Since this redundancy is on top of whatever may be provided the underlying RAID arrays, the cluster enjoys an added layer of protection that guards against catastrophic failure of a disk array. When a drive array fails, only the OSD or OSDs that make use of it are brought down.

Objects are broken down into extents, or shards, when distributed instead of having them treated as a single entity. In a 2-way replication scheme where there are more than 2 OSD servers, an object's shards will actually end up distributed across potentially all of the OSD servers. Each shard replica ends up in a Placement Group (PG) in an OSD pool somewhere out in the cluster. Scrubber processes running in the background will periodically check the shards in each PG for any errors that may crop up due to bad block development on the hard drives. In general, every PG in the cluster will be verified at least once every two weeks by the background scrubbing, and errors will be automatically corrected if they can be.

#### Monitor server

Monitor Servers (MONs) which act as the coordinators for object and other traffic. The initial Ceph Cluster would consist of a MON and two OSD servers, and this is the example used in their documentation for a quick install. They also talk about an admin server, but this is only a system which is able to painlessly remote into the cluster members using ssh authorized\_keys. The admin server would be the system that the user has set up to run Chef, Puppet or other control systems that oversee the operation of the cluster.

A single MON would be a single point of failure for Ceph, so it is recommended that the Ceph Cluster be run with an odd number of MONs with a minimum number of 3 running to establish a quorum and avoid single host failures and MON errors. For performance reasons, MONs should be put on a separate filesystem or device from OSDs because they tend to do a lot of fsyncs. Although they are typically shown as running on dedicated hosts, they can share a host with an OSD and often do in order to have enough MON servers for a decent quorum. MONs don't need a lot of storage space, so it is perfectly fine to have them run on the system drive, while the OSD servers take over whatever other disks are in the server. If you dedicate an SSD to handle OSD journals for non-BlueFS based OSD servers, the MON storage will only require another 2gb or so.

MONs coordinate shard replication and distribution by implementing the Controlled Replication Under Scalable Hashing (CRUSH) map. This is an algorithm that computes the locations for storing shards in the OSD pools. MONS also keep track of the map of daemons running the various flavors of Ceph server in the cluster. An "Initial Members" setting allows the user the specify the minimum number of MON servers that must be running in order to form a quorum. When there are not enough MONs to form a quorum, the Ceph cluster will stop processing until a quorum is re-established in order to avoid a "split-brain" situation.

The CRUSH map defaults to an algorithm that computes a deterministic uniform random distribution of where in the OSDs an object's shards should be placed, but it can be influenced by additional human specified policies. This way, a site administrator can sway CRUSH when making choices such as:

- Use the sites faster OSDs by default
- Divide OSDs into "hot" (SSD based), "normal" and "archival" (slow or tape backed) storage
- Localize replication to OSDs sitting on the same switch or subnet
- Prevent replication to OSDs on the same rack to avoid downtime when an entire RACK has a power failure
- Take underlying drive size in consideration so that for example an osd based on a 6tb drive gets 50% more shards than a 4tb based one.

It is this spreading out of the load with the CRUSH map that allows Ceph to scale up to thousands of OSDs so easily while increasing performance as new stores are added. Because of the spreading, the bottleneck transfers from raw disk performance (about 100mb/sec for a SATA drive for example) to the bandwidth capacity of your network and switches.

There are a number of ways to work with the MON pool and the rocksdb database to monitor and administrate the cluster, but the most common is the `/usr/bin/ceph` command.  This is a Python script that uses a number of Ceph supplied Python modules that use json to communicate with the MON pool.



#### Manager Server

Starting with the Luminous release, there is a new server called a Manager Server. The documentation recommends that there should be one set up to run alongside each MON on the same host. It appears to roll up the old Ceph dashboard optional product as well as other add-ons that run as plugins. This guide will be updated over time as we get more experience using it.



#### RADOS block device and RADOS gateway

Ceph provides a kernel module for the RADOS Block Device (RBD) and a librados library which libvirt and KVM can be linked against. This is essentially a virtual disk device that distributes its "blocks" across the OSDs in the Ceph cluster. An RBD provides the following capabilities:

- Thin provisioning
- I/O striping and redundancy across the Cluster
- Resizeable
- Snapshot with revert capability
- Directly useable as a KVM guest's disk device
- A variant of COW where a VM starts with a "golden image" which the VM diverges from as it operates
- Data replication between datacenters

A major selling point for the RBD is the fact that it can be used as a virtual machine's drive store in KVM. Because it spans the OSD server pool, the guest can be hot migrated between cluster CPUs by literally shutting the guest down on one CPU and booting it on another. Libvirt and Virt-Manager have provided this support for some time now, and it is probably one of the main reasons why RedHat (a major sponsor of QEMU/KVM, Libvirt, and Virt-Manager) has acquired Inktank.

The RBD and the RADOS Gateway provide the same sort of functionality for Cloud Services as [Amazon S3](https://en.wikipedia.org/wiki/Amazon_S3) and [OpenStack Swift](https://en.wikipedia.org/wiki/OpenStack#Object_Storage_.28Swift.29).  The early adopters of Ceph were interested primarily in Cloud Service object stores.  Cloud Services also drove the intial work on replication between datacenters.

#### Metadata server

Ceph provides a Metadata Server (MDS) which provides a more traditional style of filesystem based on POSIX standards that translates into objects stored in the OSD pool. This is typically where a non-Linux platform can implement client support for Ceph. This can be shared via CIFS and NFS to non-Ceph and non-Linux based systems including Windows. This is also the way to use Ceph as a drop-in replacement for HADOOP. The filesystem component started to mature around the Dumpling release.

Ceph requires all of its servers to be able to see each other directly in the cluster. So this filesystem would also be the point where external systems would be able to see the content without having direct access to the Ceph Cluster. For performance reasons, the user may have all of the Ceph cluster participants using a dedicated network on faster hardware with isolated switches. The MDS server would then have multiple NICs to straddle the Ceph network and the outside world.

When the author first rolled out ceph using the Firefly release, there was only one active MDS server at a time. Other MDS servers run in a standby mode to quickly perform a failover when the active server goes down. The cluster will take about 30 seconds to determine whether the active MDS server has failed. This may appear to be a bottleneck for the cluster, but the MDS only does the mapping of POSIX file names to object ids. With an object id, a client then directly contacts the OSD servers to perform the necessary i/o of extents/shards. Non-cephfs based traffic such as a VM running in an RBD device would continue without noticing any interruptions.

Multiple active MDS server support appeared in Jewel and became stable in Kraken. This allows the request load to be shared between more than one MDS server by divying up the namespace.

#### Storage pools

You can and will have more than one pool for storing objects. Each can use either the default CRUSH map or have an alternative in effect for its object placement. There is a default pool which is used for the generic ceph object store which your application can create and manipulate objects using the librados API. Your RBD devices go into another pool by default. The MDS server will also use its own pool for storage so if you intend to use it alongside your own RADOS aware application, get the MDS set up and running first. There is a well known layout scheme for the MDS pool that doesn't seem to be prone to change and that your RADOS aware app can take advantage of.

### Installation

As of this writing the stable version of Ceph in portage is `ceph-12.x` which corresponds to "Luminous".  In Gentoo unstable are versions `ceph-13.x` aka "Mimic".  However the author has yet to get a version of Mimic to emerge on a gentoo 17.0 desktop stable profile:

- `ceph-0.56.x` "Bobtail"
- `ceph-0.67.x` "Cuttlefish"
- `ceph-0.72.x` "Dumpling"
- `ceph-0.80.x` "Firefly" - The initial release that the author used to roll out ceph, "experimental" MDS support
- `ceph-0.87.x` "Giant" - Redhat buys up Inktank around now
- `ceph-0.94.x` "Hammer - The MDS server code wasn't considered stable until either Giant or Hammer... the author forgets
- `ceph-9.x` "Infernalis" - Redhat marketing has obviously taken over.  Last release packaged for RHEL/Centos 6.x servers
- `ceph-10.x` "Jewel" - systemd aware.  Unstable support for more than one "active" MDS server but there were "issues"
- `ceph-11.x` "Kraken" - Initial BlueFS support for OSD storage.  Multiple active MDS support marked stable
- `ceph-12.x` "Luminous" - current gentoo stable version, BlueFS marked stable and becomes default store for new OSD servers
- `ceph-13.x` "Mimic" - CephFS snapshots with multiple MDS servers, RBD image deep-copy
- `ceph-14.x` "Nautilus" - Placement-group decreasing, v2 wire protocol, rbd image live-migration between pools, rbd image namespaces for fine-granular access rights

In general, RedHat releases a new major version [every year](https://docs.ceph.com/en/latest/releases/).

#### Kernel configuration

If you want to use the RADOS block device, you will need to put that into your kernel .config as either a module or baked in. Ceph itself will want to have FUSE support enabled if you want to work with the POSIX filesystem component and you will also want to include the driver for that in Network File Systems. For your backend object stores, you will want to have xfs support because of the xattr limitations in Ext4 and btrfs because it really is becoming stable now.

```
Device Drivers
  Block devices
    Rados block device (RBD)
File systems
  XFS filesystem support
    XFS POSIX ACL support
  Btrfs filesystem support
    Btrfs POSIX Access Control Lists
  FUSE (Filesystem in Userspace) support
Network File Systems
  Ceph distributed file system
```
#### Network configuration

Ceph is sensitive to IP address changes, so you should make sure that all of your Ceph servers are assigned static IP addresses. You also may want to proactively treat the Ceph cluster members as an independent subnet from your existing network by multi-homing your existing network adapters as necessary. That way if an ISP change or other topology changes are needed, you can keep your cluster setup intact. It also gives you the luxury of migrating the ceph subnet later on to dedicated nics, switches and faster hardware such as 10Gbit ethernet or Infiniband. If the cluster subnet is small enough, consider keeping the hostnames in your /etc/hosts files, at least until things grow to the point where a pair of DNS servers among the cluster members becomes a compelling solution.



The author's initial rollout of Ceph back in the Firefly days was on four nodes, but that grew to 7 hosts. This updated guide reflects a real world use implementation of ceph Luminous 12.2.11 which should be considered a new "from scratch" install. The old btrfs based OSD store was backed off to a btrfs filesystem mirror on the mater host so that the old install could be burned down.

The DNS servers did not get set up to define an inside domain and zones for the ceph subnet. Instead the author used /etc/hosts on each machine.

**`/etc/conf.d/net`**

**Example with a multi-homed eth0 for host kroll1 (aka thufir)**

```
#
#  An example multi-homed eth0 where 192.168.1 subnet is the entire LAN and access to the outside world
#  The 192.168.2 subnet is dedicated to the ceph cluster
#
config_eth0="192.168.1.10/24 192.168.2.1/24"
routes_eth0="default via 192.168.1.1"
dns_domain_eth0="example.com"
dns_servers_eth0="192.168.1.2 192.168.1.3"
dns_search-eth0="example.com"
```


**`/etc/hosts`**

**Example of a 7 node ceph cluster with hosts kroll1-7**

```
# /etc/hosts: Local Host Database
#
# This file describes a number of aliases-to-address mappings for the for 
# local hosts that share this file.
#
# In the presence of the domain name service or NIS, this file may not be 
# consulted at all; see /etc/host.conf for the resolution order.
#
# IPv4 and IPv6 localhost aliases
127.0.0.1       localhost
::1             localhost
192.168.2.1     kroll1
192.168.2.2     kroll2
192.168.2.3     kroll3
192.168.2.4     kroll4
192.168.2.5     kroll5
192.168.2.6     kroll6
192.168.2.7     kroll7
```
#### An example Ceph cluster

This is a ceph cluster based on a collection of "frankenstein" AMD based machines in a home network. The author also had a small 3 node "personal" setup at their desk at a previous job at a major defense contractor that was based on HP and Supermicro based systems. Back in the Firefly days, 1tb drives were the norm so the "weighting" factor units for crush maps corresponded to sizes in Terabytes. The kroll home network hosts are as follows:

- kroll1 (aka Thufir) - An AMD FX9590 8 core CPU with 32GB of memory, 256GB SSD root drive and a 4x4TB SATA array formatted as a RAID5 btrfs with the default volume mounted on `/thufirraid`.  Thufir had been our admin server since the ssh keys for its root user have been pushed out to the other nodes in their `/root/.ssh/authorized_keys` files.  Over time, the disks in the 4x1tb array were replaced with 4tb drives with one going to a dedicated home mount.  The other three were set up as raid1 for a new osd daemon.  For the new rollout, these three will become individual 4tb osd servers.
- kroll2 (aka Figo) - An AMD FX8350 8 core CPU with 16GB of memory, 256GB SSD root drive and a 4x3TB SATA array formatted as btrfs RAID1. Kroll2 acted as a MON and a OSD server in the initial install for Firefly. The MON was eventually deleted and the array has been replaced by 4x4tb drives with one going to a dedicated home mount. The motherboard has been swapped out for an AMD Rzyen 7 2700x 8 core CPU installation with 32 gb of memory. The system drive is now a 512mb SSD replacing the old 256mb OCZ Vertex. Figo will get used as a host for three 4tb OSD servers.
- kroll3 (aka Mater) - An AMD FX8350 8 core CPU with 16GB of memory, 256GB SSD and 4x1TB SATA array formatted as a RAID5 btrfs. The old mater was originally both the fourth MON and an OSD server. The MON was eventually deleted when the author was researching performance as a function of the number of MON servers. Mater got hardware refreshed to an AMD Ryzen 7 1700x motherboard with 32gb of memory and a 4x4tb disk array. The existing Samsung 256gb SSD system drive was kept. Since Mater is hooked to a nice 4k display panel, this will become the new admin server for the cluster. It will just be the single MDS server in the new cluster for the moment since the old cluster contents are living on its SATA array formatted as a btrfs RAID10 mirror.
- kroll4 (aka Tube) - An AMD A10-7850K APU with 16GB of memory, 256GB SSD and a 2x2TB SATA array. Tube was originally set up as a MON and OSD server but the MON was deleted over time. The 2tb drives were swapped out for 4tb drives. In the new deployment, it will run a single OSD server with one of the drives.
- kroll5 (aka Refurb) - An AMD A10-7870K APU with 16gb of memory and a 1tb ssd with 2x4tb raid array. It wasn't part of a the old Firefly install initially, but it later got set up as a MON and as an MDS server since it was on the same KVM switch as thufir and topshelf. In the new deployment, it will be one of the three MONs (thufir, refurb, and topshelf).
- kroll6 (aka Topshelf) - An AMD FX8350 8 core CPU with 16gb of memory and a 256gb ssd drive. It wasn't part of the original Firefly deployment, but it later got set up as a MON and as the other MDS server in the active/hot backup MDS scheme that was used. The hardware was refreshed to an AMD Ryzen 7 2700x with 32gb of memory and a 1tb SSD drive. It originally had a 4x3tb array in it, but they were members of a problematic generation of Seagate drives that only has one survivor still spinning. That may eventually be refreshed, but topshelf will only be used as a MON in the new deployment for now.
- kroll7 (aka Mike) - An AMD Ryzen 7 1700x 8 core processor with 32gb of memory, 1tb SSD drive and 2x4tb raid drive. It will be used to deploy a pair of 4tb osd servers in the new cluster.

All 7 systems are running Gentoo stable profiles, but the Ryzen 7 processors are running unstable kernels in place of the stable series in order to have better AMD Zen support. The two Ryzen 1700x based hosts suffer from the dreaded idling problems of the early fab versions of Zen, but firmware tweaks on the motherboards and other voodoo rituals have kept them at bay (mostly).

#### Editing the ceph config file

We will be following the [manual guide](http://docs.ceph.com/docs/master/install/manual-deployment/) for ceph installation on their site.  There is also a Python based script call ceph-deploy which is packaged for a number of distros but not directly available for Gentoo.  If you can manage to get it working, it would automate a good bit of the process of rolling out a server from your admin node.

**`/etc/ceph/ceph.conf`**

**Initial ceph config file**

```
#
#  New version of ceph.conf for BlueFS rollout on Luminous
#  The manual installation guide offered by ceph seems to also use the host names as the names for the monitors and thus may confuse
#  the poor users who may be doing this for real.  Use mon.a, mon.b and mon.c for the monitor names instead.
#j
[global]
	fsid = fb3226b4-c5ff-4bf3-92a8-b396980c4b71
	cluster = kroll
	public network = 192.168.2.0/24
	auth cluster required = cephx
	auth service required = cephx
	auth client required = cephx
	mon initial members = mon.a, mon.b, mon.c
	mon host = 192.168.2.1:6789, 192.168.2.5:6789, 192.168.2.6:6789
#
#  Ceph example conf file suggests 1gb journal but experience shows that it
#  should be more like 10gb.  The point is moot because BlueFS will not use
#  one.  Ceph site osd documentation suggests a 5gb journal setting still.
#
	osd journal size = 10240
#
#  Set replica count to 3 with a minimum of two PGs to be available from OSDs
#  to do an initial write
#
	osd default size = 3
	osd defualt min size = 2
	
#
#  PG is glossed over in the example conf file but looked at in more detail at
#  http://docs.ceph.com/en/latest/rados/operations/placement-groups/#choosing-number-of-placement-groups
#  The preselection rule of thumb section there suggests pg=128 for 5 osds or
#  less, pg=512 for 5-10 osds and pg=1024 for between 10-50 osds
#
#  Actually, looking at their example, the author suspects that someone hasn't
#  bothered to revisit the 333 number there since Inktank had a different idea
#  for settings in the earlier days.  Their 3 osd example should only have this
#  at 128.  kroll will initially have 8 osd servers on three hosts so will use
#  512
#
	osd pool default pg num = 512
	osd pool default pgp num = 512
#
#  It's not documented well on the ceph site, but this is the crushmap setting
#  where you can get over the having 3 replicas with only 1 to 2 hosts.
#  Set it to 0 if you have fewer nodes than replicas (ie only 1 osd
#  host)
#
#  An entry from a random blog author confirms this.  Also, this link at
#  serverfault shows the pulling of an operational crush map and having up to
#  ten different types available in it with anything greater than 2 having
#  meaning only for enormous clusters like CERN.
#
#  https://serverfault.com/questions/845927/is-ceph-replication-based-on-nodes-or-disks
#
	osd crush chooseleaf type = 1
```

We used `uuidgen` to generate a new random uuid for the entire cluster.  We will rename the cluster name from the default `ceph` to `kroll` to match our host naming scheme.  We specify the 192.168.2 network to be the "public" network for the cluster.  Other default settings come from the manual install url mentioned earlier, including a default to replicate two copies of each object with a minimum of 1 copy allowed when the cluster is in "degraded" state.

The example conf file has only a single MON but we use a quorum of three using kroll1, kroll5 and kroll6.

We override the OSD journal size as noted, but the entire thing is moot since we will be using BlueFS.

We use a 3 replica setup which matches the ceph example but read our comments above. Their example glosses over pg sizing and will not work if you have less than 3 hosts running osd servers.

`root #``uuidgen`
fb3226b4-c5ff-4bf3-92a8-b396980c4b71

After editing the file, we copy it around to the other cluster members from our admin node kroll1 using `scp`

#### /etc/conf.d/ceph file

There is a conf.d file for the ceph service but it is pretty barebones and doesn't need changing unless services are being juggled for more than one cluster with different conf files. Since we changed the cluster name from ceph to kroll but still use the default ceph.conf name for the file, we change it to uncomment out the setting

**`/etc/conf.d/ceph`**

**Barebones ceph file**

```
# Example
# default ceph conf file
ceph_conf="/etc/ceph/ceph.conf"
# Set RADOSGW_WANT_NAME_PARAM=y in order to make the init script add
# a --name=client.${RC_SVCNAME} parameter to command_args for radosgw.*
# service instances. This will make the service use a key by the name
# of client.${RC_SVCNAME} instead of the default client.admin key.
# A setting like this in the ceph config file can be used to customize
# the rgw_data and keyring paths used by radosgw instances:
# [client]
# rgw_data = /var/lib/ceph/radosgw/$cluster-$id
# keyring = /var/lib/ceph/radosgw/$cluster-$id/keyring
RADOSGW_WANT_NAME_PARAM=n
```
#### Creating Keyrings For MON rollout

Ceph uses its own shared secret concept when handling communications among cluster members.  We must generate keyring files that will then be distributed out to the servers that will be set up among the cluster members.  The keyrings are generated by the `ceph-authtool` command.  The first keyring is for the mon servers.  The manual install url has it going to a file on `/tmp`, but we are more inclined to keep it around by parking it in `/etc/ceph`

`root #``ceph-authtool --create-keyring /etc/ceph/ceph.mon.keyring --gen-key -n mon. --cap mon 'allow *'`
creating /etc/ceph/ceph.mon.keyring


The result is a readable text file:

**`/etc/ceph/ceph.mon.keyring`**

**ceph-authtool generated file**

```
[mon.]
        key = (redacted key text)
        caps mon = "allow *"
```

Next we create an admin keyring file which goes into the /etc/ceph/ceph.client.admin.keyring file.

`root #``ceph-authtool --create-keyring /etc/ceph/ceph.client.admin.keyring --gen-key -n client.admin --cap mon 'allow *' --cap osd 'allow *' --cap mds 'allow'`
creating /etc/ceph/ceph.client.admin.keyring


The resulting text file may actually be shorter than the complicated command line used to create it.  The redacted key here is the same as the one that appears in our mon keyring, so it must be based on the UUID parked in the /etc/ceph/ceph.conf config file.

**`/etc/ceph/ceph.client.admin.keyring`**

**ceph-authtool generated admin file**

```
[client.admin]
        key = (redacted key text)
        caps mds = "allow"
        caps mon = "allow *"
        caps osd = "allow *"
```
#### Creating /var/lib/ceph

ceph uses /var/lib/ceph for various server settings and storage. Since the author had a legacy install of ceph to start with, there was already a /var/lib/ceph tree with ownership set to ceph:ceph. Daemons ran as the root user in ceph release up until around Giant and then changed to run as the user ceph in later releases so this ownership needed a reset at some point. Depending on the class of ceph servers running on the host there would then be msd, mon and osd subdirectories under this tree with the appropriate files. There is also likely to be a tmp subdir there that gets created at some point due to commands. YMMV for a fresh install so you may need to create a tree like this. The author had to create a new /var/lib/ceph/bootstrap-osd subdir for himself for the next keyring:

`root #``ceph-authtool --create-keyring /var/lib/ceph/bootstrap-osd/ceph.keyring --gen-key -n client.bootstrap-osd --cap mon 'profile bootstrap-osd'`
creating /var/lib/ceph/bootstrap-osd/ceph.keyring

**`/var/lib/ceph/bootstrap-osd/ceph.keyring`**

**ceph-authtool generated client.bootstrap-osd file**

```
[client.bootstrap-osd]
        key = (redacted key text)
        caps mon = "profile bootstrap-osd"
```
#### Merging the three keyrings together into the mon.keyring file

The ceph manual guide then uses the authtool with `--import-keyring` options to merge the three keys together into the mon.keyring file.  You can save a bit of typing just by using good 'ole cat to slap everything together.

`root #``cat client.admin.keyring /var/lib/ceph/bootstrap-osd/ceph.keyring >>ceph.mon.keyring`
mater /etc/ceph #



**`/etc/ceph/ceph.mon.keyring`**

**Resulting merged ceph.mon.keyring file**

```
[client.bootstrap-osd]
        key = (redacted key text)
        caps mon = "profile bootstrap-osd"
[mon.]
        key = (redacted key text)
        caps mon = "allow *"
[client.admin]
        key = (redacted key text)
        caps mds = "allow *"
        caps mgr = "allow *"
        caps mon = "allow *"
        caps osd = "allow *"
[client.bootstrap-osd]
        key = (redacted key text)
        caps mon = "profile bootstrap-osd"
```
#### Creating the initial monmap file

The OSD and MDS servers use the /etc/ceph/ceph.conf for discovering MON servers, but the MON servers themselves have a much stricter consistency scheme in order to form and maintain their quorum. When up and running the quorum uses a majority rule voting system with each maintaining a local rocksdb database in the filesystem, but MONs do work with an initial binary file called a monmap when you first set up the Ceph cluster.

The [manual deployment](http://docs.ceph.com/docs/master/install/manual-deployment/) page covers the example where only a single MON is used to form the quorum.  It's simply a matter of using more `--add` stanzas to define our initial 3 member monitor map.

The monmap command is used to create the initial monmap binary file.  We essentially give it the addresses corresponding to our `mon.a on kroll1, mon.b on kroll5, and mon.c kroll6` `initial members` and the cluster fsid from `/etc/ceph/ceph.conf` file.  We will park this file in /etc/ceph and then pass it around to the right place when we configure our MON hosts.

`root #``monmaptool  --create --add mon.a 192.168.2.1 --add mon.b 192.168.2.5 --add mon.c 192.168.2.6 --fsid 1798897a-f0c9-422d-86b3-d4933a12c7ac initial-monmap`
monmaptool: monmap file initial-monmap
monmaptool: set fsid to fb3226b4-c5ff-4bf3-92a8-b396980c4b71
monmaptool: writing epoch 0 to initial-monmap (3 monitors)

Once the three monitors are up and running and have established a quorum, they will begin to automatically revise this initial monitor map.   Each revision is called an epoch, and the epoch number will get bumped whenever it happens.  It will change when the OSDs get added and as the initial CRUSH map and PG pools get set up.  It also changes as events happen such as the scrubbing processes scrub a PG and move on to the next.  So this initial map will no longer be needed after the quorum is established.  In fact, when a new monitor is added to the cluster following [add-or-rm-mons](https://docs.ceph.com/en/latest/rados/operations/add-or-rm-mons/), there's a point where you retrieve the current monitor map from the quorum to a file and then use that to create the new monitor's filesystem.  Part of the process of joining the new monitor to the quorum involves it figuring out what needs to be changed to go from the old epoch number to the current one that the quorum is working with.

We push the initial monmap file over to /etc/ceph directories on kroll1, kroll5 and kroll6. However this isn't the only file that needs to go over since ceph.conf, the admin keyring and the mon keyring files need to go as well. So we will use rsync instead of scp. The output isn't shown since the author's install has junk in /etc/ceph leftover from the old cluster install that may go over as well, and that may confuse the reader.

`root #``rsync -av /etc/ceph/ kroll1:/etc/ceph/``root #``rsync -av /etc/ceph/ kroll5:/etc/ceph/``root #``rsync -av /etc/ceph/ kroll6:/etc/ceph/`


#### Creating kroll1 server mon.a on thufir

Ceph servers look for their file trees in /var/lib/ceph. Mon servers look for their server id name subtree under /var/lib/ceph/mon/clustername-monname where clustername is ceph because we are using the /etc/ceph/cepf.conf file for kroll and monname is the mon.a monitor name we just added to the initial monmap file. Kroll1 (thufir) will host mon.a, so we shell into it and create /var/lib/ceph/mon/ceph-mon.a for the filesystem.

`root #``mkdir -p /var/lib/ceph/mon/ceph-mon.a``root #``chown -R ceph:ceph /var/lib/ceph`
Before continuing on, you may want to look at /var/log/ceph to clear out anything that may be in there. The next command will create an empty /var/log/ceph/ceph-mon.mon.a.log file if it doesn't already exist.

`root #``cd /var/log/ceph``root #``rm -rf *``root #``cd /etc/ceph`
The ceph-mon command will populate the mon.a directory with a copy of the ceph.mon.keyring file renamed to keyring\< and a store.db directory tree which is a rocksdb database reflecting the contents of the initial monmap file.

`root #``ceph-mon --mkfs -i mon.a --monmap initial-monmap --keyring ceph.mon.keyring``root #``ls -l /var/lib/ceph/mon/ceph-mon.a`
/var/lib/ceph/mon/ceph-mon.a:
total 12
-rw------- 1 root root   77 Mar 27 11:30 keyring
-rw-r--r-- 1 root root    8 Mar 27 11:30 kv\_backend
drwxr-xr-x 2 root root 4096 Mar 27 11:30 store.db
/var/lib/ceph/mon/ceph-mon.a/store.db:
total 24
-rw-r--r-- 1 root root  820 Mar 27 11:30 000003.log
-rw-r--r-- 1 root root   16 Mar 27 11:30 CURRENT
-rw-r--r-- 1 root root   37 Mar 27 11:30 IDENTITY
-rw-r--r-- 1 root root    0 Mar 27 11:30 LOCK
-rw-r--r-- 1 root root   13 Mar 27 11:30 MANIFEST-000001
-rw-r--r-- 1 root root 4143 Mar 27 11:30 OPTIONS-000005

As mentioned previously, the ceph daemons changed around the Giant or Hammer releases to setuid to ceph from root in order to drop privileges. So we reset the ownership of the tree or else we will get permission errors when trying to start the mon.

`root #``chown -R ceph:ceph /var/lib/ceph/mon/ceph-mon.a`
We set up the mon.a server startup in /etc/init.d by softlinking.  The naming here is crucial since the init script checks the daemon type by chopping off `ceph-` in front and then chopping off everything after the first period.  If you looked in /var/log/ceph and had used the same mon.a monitor name as this example, you would have seen a `ceph-mon.mon.a.log` file get created.  So we use this for the softlink name.

`root #``cd /etc/init.d``root #``ln -s ceph ceph-mon.mon.a``root #``rc-update add ceph.mon.mon.a default`
\* service ceph-mon.mon.a added to runlevel default

We repeated the same process to create mon.b and mon.c on the other kroll member hosts.

##### kroll2 (refurb)

`root #``mkdir -p /var/lib/ceph/mon/ceph-mon.b``root #``cd /etc/ceph``root #``ceph-mon --mkfs -i mon.b --monmap initial-monmap --keyring ceph.mon.keyring``root #``chown -R ceph:ceph /var/lib/ceph/mon``root #``cd /etc/init.d``root #``ln -s ceph ceph-mon.mon.b``root #``rc-update add ceph.mon-1 default`
\* service ceph-mon.mon.b added to runlevel default

##### kroll3 (topshelf)

`root #``mkdir -p /var/lib/ceph/mon/ceph-2``root #``ceph-mon --mkfs -i 2 --monmap initial-monmap --keyring ceph.mon.keyring`
ceph-mon: set fsid to 1798897a-f0c9-422d-86b3-d4933a12c7ac
ceph-mon: created monfs at /var/lib/ceph/mon/ceph-2 for mon.2

`root #``cd /etc/init.d``root #``ln -s ceph ceph.mon-2``root #``rc-update add ceph.mon-2 default`
\* service ceph.mon-2 added to runlevel default

#### Starting the Mon servers

With all three kroll hosts configured with mons, we now go back and start the services beginning with mon.0 on kroll1.

`root #``/etc/init.d/ceph.mon.mon.a start`
\* Caching service dependencies ...                                                                         \[ ok \]
\* Starting ceph-mon.mon.a ...                                                                              \[ ok \]

After starting the other two mons over on the other servers we come back to the log directory on thufir.  There is now a new ceph.log along with the three log files associated with `mon.a`.

`root #``cd /var/log/ceph``root #``ls -l`
total 24
-rw------- 1 ceph ceph  1789 Mar 27 13:21 ceph.log
-rw------- 1 ceph ceph     0 Mar 27 13:21 ceph.mon.a-stderr.log
-rw------- 1 ceph ceph     0 Mar 27 13:21 ceph.mon.a-stdout.log
-rw-r--r-- 1 root root 16567 Mar 27 13:20 ceph-mon.mon.a.log

`root #``cat ceph.log`
2019-03-27 13:21:32.024640 mon.mon.a unknown.0 - 0 :  \[INF\] mkfs fb3226b4-c5ff-4bf3-92a8-b396980c4b71
2019-03-27 13:21:26.972132 mon.mon.a mon.0 192.168.2.1:6789/0 2 : cluster \[INF\] mon.mon.a is new leader, mons mon.a,mon.b in quorum (ranks 0,1)
2019-03-27 13:21:21.953110 mon.mon.b mon.1 192.168.2.5:6789/0 1 : cluster \[INF\] mon.mon.b calling monitor election
2019-03-27 13:21:24.715493 mon.mon.c mon.2 192.168.2.6:6789/0 1 : cluster \[INF\] mon.mon.c calling monitor election
2019-03-27 13:21:26.995067 mon.mon.b mon.1 192.168.2.5:6789/0 2 : cluster \[INF\] mon.mon.b calling monitor election
2019-03-27 13:21:26.995098 mon.mon.a mon.0 192.168.2.1:6789/0 3 : cluster \[INF\] mon.mon.a calling monitor election
2019-03-27 13:21:27.013385 mon.mon.c mon.2 192.168.2.6:6789/0 2 : cluster \[INF\] mon.mon.c calling monitor election
2019-03-27 13:21:32.011413 mon.mon.a mon.0 192.168.2.1:6789/0 4 : cluster \[INF\] mon.mon.a is new leader, mons mon.a,mon.b in quorum (ranks 0,1)
2019-03-27 13:21:32.028219 mon.mon.c mon.2 192.168.2.6:6789/0 3 : cluster \[INF\] mon.mon.c calling monitor election
2019-03-27 13:21:32.028945 mon.mon.a mon.0 192.168.2.1:6789/0 6 : cluster \[INF\] pgmap 0 pgs: ; 0B data, 0B used, 0B / 0B avail
2019-03-27 13:21:32.048768 mon.mon.a mon.0 192.168.2.1:6789/0 10 : cluster \[INF\] overall HEALTH\_OK
2019-03-27 13:21:32.052923 mon.mon.b mon.1 192.168.2.5:6789/0 3 : cluster \[INF\] mon.mon.b calling monitor election
2019-03-27 13:21:32.069181 mon.mon.a mon.0 192.168.2.1:6789/0 11 : cluster \[INF\] mon.mon.a calling monitor election
2019-03-27 13:21:34.777644 mon.mon.a mon.0 192.168.2.1:6789/0 12 : cluster \[INF\] mon.mon.a is new leader, mons mon.a,mon.b,mon.c in quorum (ranks 0,1,2)
2019-03-27 13:21:34.797593 mon.mon.a mon.0 192.168.2.1:6789/0 17 : cluster \[INF\] overall HEALTH\_OK

ceph -s will now work, but of course we don't have any OSDs spun up yet.

`root #``ceph -s````
   cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: no daemons active
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   0B used, 0B / 0B avail
    pgs:     
```


#### Creating mgr.a on kroll1

This appears to consist of creating a keyring file for the new mgr service out in /var/lib/ceph and then adding it as a new service to the default runlevel.

`root #``mkdir -p /var/lib/ceph/mgr/ceph-a``root #``ceph auth get-or-create mgr.a mon 'allow profile mgr' osd 'allow *' mds 'allow *'````
[mgr.a]
        key = (redacted key text)
```
We simply dumped that out into /var/lib/ceph/mgr/ceph-a/keyring and then reset ownership on everything to ceph.

`root #``chown -R ceph:ceph /var/lib/ceph/mgr`
Then softlinked a new init script for mgr.a and added it to the default runlevel.

`root #``cd /etc/init.d``root #``ln -s ceph ceph-mgr.a``root #``./ceph-mgr.a start`
\* Caching service dependencies ...                                                                                               \[ ok \]
 \* Starting ceph-mgr.a ...                                                                                                        \[ ok \]

`root #``rc-update add ceph-mgr.a default`
\* service ceph-mgr.a added to runlevel default

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active)
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   0B used, 0B / 0B avail
    pgs:     
```
On kroll5 and kroll6 we simply piped the output from the authtool to the keyring files directly for mgr.b and mgr.c

(on kroll5)

`root #``cd /var/lib/ceph``root #``mkdir -p mgr/ceph-b``root #``cd mgr/ceph-b``root #``ceph auth get-or-create mgr.b mon 'allow profile mgr' osd 'allow *' mds 'allow *' >keyring``root #``chown -R ceph:ceph /var/lib/ceph/mgr``root #``cd /etc/init.d``root #``ln -s ceph ceph-mgr.b``root #``./ceph-mgr.b start`
\* Caching service dependencies ...                                                                                               \[ ok \]
 \* Starting ceph-mgr.b ...                                                                                                        \[ ok \]

`root #``rc-update add ceph-mgr.b default`
\* service ceph-mgr.b added to runlevel default

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   0B used, 0B / 0B avail
    pgs:     
```

(on kroll6)

`root #``cd /var/lib/ceph``root #``mkdir -p mgr/ceph-c``root #``cd mgr/ceph-c``root #``ceph auth get-or-create mgr.c mon 'allow profile mgr' osd 'allow *' mds 'allow *' >keyring``root #``chown -R ceph:ceph /var/lib/ceph/mgr``root #``cd /etc/init.d``root #``ln -s ceph ceph-mgr.c``root #``./ceph-mgr.c start`
\* Caching service dependencies ...                                                                                               \[ ok \]
 \* Starting ceph-mgr.c ...                                                                                                        \[ ok \]

`root #``rc-update add ceph-mgr.c default`
\* service ceph-mgr.c added to runlevel default

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   0B used, 0B / 0B avail
    pgs:     
```


#### Creating osd.0 on kroll1

Creating and starting osd servers has changed radically since the Firefly release when this guide was first written.  The author first followed the "short form" section for Bluestore in the [deployment guide](http://docs.ceph.com/en/latest/install/manual-deployment/) but immediately ran into a couple of stumbling blocks that required a bit more prerequiste work and some deviation from the steps.

The first problem was the way Bluestore wants to use a disk. It requires LVM, a layer of bureaucracy that the author merely tolerated in the RHEL/Centos world and would happily avoid if allowed to build his own servers. So, it never ended getting activated on any of his home Gentoo servers. This got resolved by making sure that lvm and lvmetad got thrown into the boot runlevel with lvm-monitoring getting put into the default runlevel.

The second problem is that Redhat's fanatical devotion to systemd has infected the folk at Inktank.  The `ceph-volume create` command assumes that the user wants to activate the service after the osd filesystem is created.  It tries to run systemctl to get that done, promptly panics when the aforementioned virus isn't found and then proceeds to rollback everything to a non-osd state.  This requires that the `ceph-volume` command only gets used to prepare the osd and then we take things from there.

We had leftovers from the previous abortive attempts so you might just want to breeze by `osd.0` and look at `osd.1` creation unless you want to see how to take out the garbage first.

kroll1 (thufir) has 3 4tb drives at /dev/sdc, /dev/sdd and /dev/sde that will be used to make the first three osd servers.

`root #``ceph osd purge 0 --yes-i-really-mean-it``root #``ceph osd destroy 0 --yes-i-really-mean-it``root #``ceph-volume lvm zap /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735``root #``ceph-volume lvm prepare --data /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735`
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new a23b9c03-bc6c-4c11-844d-f95eb8a5aeae
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: mount -t tmpfs tmpfs /var/lib/ceph/osd/ceph-0
--> Absolute path not found for executable: restorecon
--> Ensure $PATH environment variable contains common executable locations
Running command: chown -h ceph:ceph /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735
Running command: chown -R ceph:ceph /dev/dm-0
Running command: ln -s /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735 /var/lib/ceph/osd/ceph-0/block
Running command: ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring mon getmap -o /var/lib/ceph/osd/ceph-0/activate.monmap
 stderr: got monmap epoch 1
Running command: ceph-authtool /var/lib/ceph/osd/ceph-0/keyring --create-keyring --name osd.0 --add-key (key redacted)
 stdout: creating /var/lib/ceph/osd/ceph-0/keyring
added entity osd.0 auth auth(auid = 18446744073709551615 key=(key redacted) with 0 caps)
Running command: chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/keyring
Running command: chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/
Running command: /usr/bin/ceph-osd --cluster ceph --osd-objectstore bluestore --mkfs -i 0 --monmap /var/lib/ceph/osd/ceph-0/activate.monmap --keyfile - --osd-data /var/lib/ceph/osd/ceph-0/ --osd-uuid a23b9c03-bc6c-4c11-844d-f95eb8a5aeae --setuser ceph --setgroup ceph
--> ceph-volume lvm prepare successful for: ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735

The resulting filesystem looks like this:

`root #``cd /var/lib/ceph/osd/ceph-0``root #``ls -l`
total 48
-rw-r--r-- 1 ceph ceph 393 Mar 27 22:42 activate.monmap
lrwxrwxrwx 1 ceph ceph  93 Mar 27 22:42 block -> /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735
-rw-r--r-- 1 ceph ceph   2 Mar 27 22:42 bluefs
-rw-r--r-- 1 ceph ceph  37 Mar 27 22:42 ceph\_fsid
-rw-r--r-- 1 ceph ceph  37 Mar 27 22:42 fsid
-rw------- 1 ceph ceph  56 Mar 27 22:42 keyring
-rw-r--r-- 1 ceph ceph   8 Mar 27 22:42 kv\_backend
-rw-r--r-- 1 ceph ceph  21 Mar 27 22:42 magic
-rw-r--r-- 1 ceph ceph   4 Mar 27 22:42 mkfs\_done
-rw-r--r-- 1 ceph ceph  41 Mar 27 22:42 osd\_key
-rw-r--r-- 1 ceph ceph   6 Mar 27 22:42 ready
-rw-r--r-- 1 ceph ceph  10 Mar 27 22:42 type
-rw-r--r-- 1 ceph ceph   2 Mar 27 22:42 whoami

The keyring file is a new unique key that was generated for `osd-0` by the ceph-authtool.  It actually would have appeared in the output above if the author hadn't redacted it.

`root #``cat keyring````
[osd.0]
        key = (redacted, unique to osd.0)
```

`ceph -s` now shows there's an osd, but it is marked as both down and out.  That means that both the service isn't running and that there are no placement groups (PGs) present on it.

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 1 osds: 0 up, 0 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   0B used, 0B / 0B avail
    pgs:     
```
The `ceph osd tree` command shows that `ceph-volume` did some additional work for us behind the scenes to configure things for the CRUSH map.  However, since `osd.0` has yet to be started, the cluster doesn't yet know just how big the drive is for the weighting part.

`root #``ceph osd tree`
\# id    weight  type name       up/down reweight
-1      0       root default
-2      0               host kroll1
0       0       osd.0   down    0

All that is left is to enable and start the osd.0 service.

`root #``cd /etc/init.d``root #``ln -s ceph ceph.osd-0``root #``rc-update add ceph.osd-0 default`
\* service ceph.osd-0 added to runlevel default

`root #``./ceph.osd-0 start`
\* Caching service dependencies ...                                                                     \[ ok \]
 \* Starting Ceph osd.0 ...
starting osd.0 at :/0 osd\_data /var/lib/ceph/osd/ceph-0 /var/lib/ceph/osd/ceph-0/journal                \[ ok \]

With one osd spun up, our cluster is now operating in a degraded state. We don't want to go creating pools and cephfs until we have osds running on at least three hosts. In the old days, the cluster would have been in HEALTH\_WARN state with degraded PGs since it would have created some pools with default sizes from the getgo.

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 1 osds: 1 up, 1 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   1.00GiB used, 3.64TiB / 3.64TiB avail
    pgs:     
```
The `ceph osd tree` command now shows `osd.0` under the host thufir with a weight set to 3.63869 which shows just how much you don't get when you buy a 4tb drive.  We are going to reweight that to a nice round 4.0 number instead.

`root #``ceph osd crush reweight osd.0 4.0`
reweighted item id 0 name 'osd.0' to 4 in crush map



#### osd.1 and osd.2 on kroll1

`ceph-volume` is a lot more concise, now that we know what we are doing.

`root #``ceph-volume lvm prepare --data /dev/sdd`
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new ecfc1e8d-f21d-46cb-93f8-ad1065d4542a
Running command: vgcreate --force --yes ceph-1e733afa-e1b4-45a1-b5fe-031eb25ca379 /dev/sdd
 stdout: Physical volume "/dev/sdd" successfully created.
 stdout: Volume group "ceph-1e733afa-e1b4-45a1-b5fe-031eb25ca379" successfully created
Running command: lvcreate --yes -l 100%FREE -n osd-block-ecfc1e8d-f21d-46cb-93f8-ad1065d4542a ceph-1e733afa-e1b4-45a1-b5fe-031eb25ca379
 stdout: Logical volume "osd-block-ecfc1e8d-f21d-46cb-93f8-ad1065d4542a" created.
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: mount -t tmpfs tmpfs /var/lib/ceph/osd/ceph-1
--> Absolute path not found for executable: restorecon
--> Ensure $PATH environment variable contains common executable locations
Running command: chown -h ceph:ceph /dev/ceph-1e733afa-e1b4-45a1-b5fe-031eb25ca379/osd-block-ecfc1e8d-f21d-46cb-93f8-ad1065d4542a
Running command: chown -R ceph:ceph /dev/dm-1
Running command: ln -s /dev/ceph-1e733afa-e1b4-45a1-b5fe-031eb25ca379/osd-block-ecfc1e8d-f21d-46cb-93f8-ad1065d4542a /var/lib/ceph/osd/ceph-1/block
Running command: ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring mon getmap -o /var/lib/ceph/osd/ceph-1/activate.monmap
 stderr: got monmap epoch 1
Running command: ceph-authtool /var/lib/ceph/osd/ceph-1/keyring --create-keyring --name osd.1 --add-key (key redacted)
 stdout: creating /var/lib/ceph/osd/ceph-1/keyring
added entity osd.1 auth auth(auid = 18446744073709551615 key=(key redacted) with 0 caps)
Running command: chown -R ceph:ceph /var/lib/ceph/osd/ceph-1/keyring
Running command: chown -R ceph:ceph /var/lib/ceph/osd/ceph-1/
Running command: /usr/bin/ceph-osd --cluster ceph --osd-objectstore bluestore --mkfs -i 1 --monmap /var/lib/ceph/osd/ceph-1/activate.monmap --keyfile - --osd-data /var/lib/ceph/osd/ceph-1/ --osd-uuid ecfc1e8d-f21d-46cb-93f8-ad1065d4542a --setuser ceph --setgroup ceph
--> ceph-volume lvm prepare successful for: /dev/sdd

`root #``cd /etc/init.d``root #``ln -s ceph ceph-osd.1``root #``./ceph-osd.1 start`
\* Caching service dependencies ...                                                                                               \[ ok \]
 \* Starting ceph-osd.1 ...                                                                                                        \[ ok \]

`root #``rc-update add ceph-osd.1 default`
\* service ceph-osd.1 added to runlevel default

`root #``ceph osd crush reweight osd.1 4.0`
reweighted item id 1 name 'osd.1' to 4 in crush map

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 2 osds: 2 up, 2 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   2.00GiB used, 7.28TiB / 7.28TiB avail
    pgs:     
```

Adding the third osd on kroll using `/dev/sde` and getting it up and in leaves us like so before we move on to kroll2 (figo) and its "great tracts of land".

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 3 osds: 3 up, 3 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   3.01GiB used, 10.9TiB / 10.9TiB avail
    pgs:     
```
`root #``ceph osd tree`
ID CLASS WEIGHT   TYPE NAME       STATUS REWEIGHT PRI-AFF 
-1       12.00000 root default                            
-3       12.00000     host thufir                         
 0   hdd  4.00000         osd.0       up  1.00000 1.00000 
 1   hdd  4.00000         osd.1       up  1.00000 1.00000 
 2   hdd  4.00000         osd.2       up  1.00000 1.00000



#### osd.3, osd.4 and osd.5 on kroll2 (figo)

Before doing anything with figo, we pop over to mater real quick to make sure that the /etc/ceph directory gets an rsync over to figo.  When figo got its hardware refresh to Ryzen2, it ended up with a clean Gentoo install and no /etc/ceph directory to start with.  We also want to rsync the bootstrap-osd tree which `ceph-volume` uses for keys.

(on mater)

`root #``rsync -av --delete /etc/ceph/ figo:/etc/ceph/`
created directory /etc/ceph
./
ceph.client.admin.keyring
ceph.conf
ceph.conf.old
ceph.conf.orig
ceph.conf\~
ceph.keyring
ceph.mon.keyring
client.libvirt.key
initial-monmap
libvirt.secret.xml
root.secret
sent 30,153 bytes  received 431 bytes  20,389.33 bytes/sec
total size is 28,711  speedup is 0.94

As mentioned earlier, you will see a few odds and ends (eg libvirt stuff) that belong in the "nothing to see here, move along" category. That includes a couple of older versions of ceph.conf from the before-times that have a lot of now useless stuff in them.

`root #``rsync -av /var/lib/ceph/bootstrap-osd/ figo:/var/lib/ceph/bootstrap-osd/`
sending incremental file list
created directory /var/lib/ceph/bootstrap-osd
./
ceph.keyring
sent 222 bytes  received 88 bytes  620.00 bytes/sec
total size is 107  speedup is 0.35


(moving on to figo now)

It isn't in the /etc/fstab of the new ssd drive, but we can see that there is still a btrfs filesystem lying around from the old figo from when it was running as the old `osd.1`.  We will be letting `ceph-volume` have its way with these three drives.

`root #``btrfs fi show````
Label: 'ROOT'  uuid: 5559cde3-d3f2-4617-af34-5476453ec37c
        Total devices 1 FS bytes used 37.72GiB
        devid    1 size 465.51GiB used 45.01GiB path /dev/sda3
Label: 'HOME'  uuid: b9515249-ae10-4d56-b212-f629be369ace
        Total devices 1 FS bytes used 1.45TiB
        devid    1 size 3.64TiB used 1.46TiB path /dev/sdc
Label: 'FIGOSYS'  uuid: c9c536b9-99a2-4efc-b2f9-fa4fa550bd24
        Total devices 1 FS bytes used 45.86GiB
        devid    1 size 200.17GiB used 200.17GiB path /dev/sdb3
Label: 'cephosd1'  uuid: 0663dbb2-1a57-4660-b43b-f40a4a0e2dab
        Total devices 3 FS bytes used 1.77TiB
        devid    1 size 3.64TiB used 1.18TiB path /dev/sde
        devid    2 size 3.64TiB used 1.18TiB path /dev/sdf
        devid    3 size 3.64TiB used 1.18TiB path /dev/sdd
```
We realized that we had gotten a little ahead of ourselves when trying to spin up `osd.3` and saw the following errors.  LVM wasn't running yet and /var/lib/ceph/osd didn't exist yet either.

`root #``ceph-volume lvm prepare --data /dev/sdd`
ceph-volume lvm prepare --data /dev/sdd
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new 5c140ed9-b126-4906-9db3-7ed9357e385f
Running command: vgcreate --force --yes ceph-78c23580-df79-4bad-b437-7e1d88b73f41 /dev/sdd
 stderr: /run/lvm/lvmetad.socket: connect failed: No such file or directory
  WARNING: Failed to connect to lvmetad. Falling back to internal scanning.
 stdout: Wiping btrfs signature on /dev/sdd.
 stdout: Physical volume "/dev/sdd" successfully created.
 stdout: Volume group "ceph-78c23580-df79-4bad-b437-7e1d88b73f41" successfully created
Running command: lvcreate --yes -l 100%FREE -n osd-block-5c140ed9-b126-4906-9db3-7ed9357e385f ceph-78c23580-df79-4bad-b437-7e1d88b73f41
 stderr: /run/lvm/lvmetad.socket: connect failed: No such file or directory
  WARNING: Failed to connect to lvmetad. Falling back to internal scanning.
 stdout: Logical volume "osd-block-5c140ed9-b126-4906-9db3-7ed9357e385f" created.
Running command: /usr/bin/ceph-authtool --gen-print-key
--> Was unable to complete a new OSD, will rollback changes
--> OSD will be fully purged from the cluster, because the ID was generated
Running command: ceph osd purge osd.3 --yes-i-really-mean-it
 stderr: purged osd.3
-->  OSError: \[Errno 2\] No such file or directory: '/var/lib/ceph/osd/ceph-3'

Remedying lvm first...

`root #``rc-update add lvm boot`
\* service lvm added to runlevel boot

`root #``/etc/init.d/lvm start`
\* /run/lvm: creating directory
 \* Starting lvmetad ...                                                                                                           \[ ok \]
 \* Setting up the Logical Volume Manager ...                                                                                      \[ ok \]

`root #``rc-update add lvm-monitoring default`
\* service lvm-monitoring added to runlevel default

`root #``/etc/init.d/lvm-monitoring start`
\* Starting dmeventd ...                                                                                                          \[ ok \]
 \* Starting LVM monitoring for VGs   ceph-78c23580-df79-4bad-b437-7e1d88b73f41: ...
  1 logical volume(s) in volume group "ceph-78c23580-df79-4bad-b437-7e1d88b73f41" monitored                                       \[ ok \]

Moving on to creating the osd subtree in /var/lib/ceph now...

`root #``ls -l /var/lib/ceph`
total 0
drwxr-xr-x 1 root root 24 Mar 23 10:40 bootstrap-osd
drwxr-xr-x 1 ceph ceph 48 Mar 18 21:35 tmp

`root #``mkdir /var/lib/ceph/osd``root #``chown ceph:ceph /var/lib/ceph/osd`
As you may have noticed, ceph-volume rolled back the new osd.3 changes to the crush map when it couldn't create the ceph-3 subtree.  However, the lvm volume is still lying around so we need to use that instead of the device name `/dev/sdd` when running the prepare again like when we botched the creation of `osd.0` on thufir eearlier.  Otherwise we get this error.

`root #``ceph-volume lvm prepare --data /dev/sdd`
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new d7259989-477c-4abf-b31c-eb7d028428d4
Running command: vgcreate --force --yes ceph-4b0080d9-a107-4c7b-b14d-f54efd5e1dae /dev/sdd
 stderr: Can't open /dev/sdd exclusively.  Mounted filesystem?
--> Was unable to complete a new OSD, will rollback changes
--> OSD will be fully purged from the cluster, because the ID was generated
Running command: ceph osd purge osd.3 --yes-i-really-mean-it
 stderr: purged osd.3
-->  RuntimeError: command returned non-zero exit status: 5

We have to use `lsblk` to figure out where lvm is hiding the block device for that new lvm volume and then use it for the `ceph-volume` command now instead of /dev/sdd

`root #``lsblk -a````
NAME                                                                                                MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0                                                                                                 7:0    0         0 loop 
loop1                                                                                                 7:1    0         0 loop 
loop2                                                                                                 7:2    0         0 loop 
loop3                                                                                                 7:3    0         0 loop 
loop4                                                                                                 7:4    0         0 loop 
loop5                                                                                                 7:5    0         0 loop 
loop6                                                                                                 7:6    0         0 loop 
loop7                                                                                                 7:7    0         0 loop 
sda                                                                                                   8:0    0 465.8G  0 disk 
├─sda1                                                                                                8:1    0     2M  0 part 
├─sda2                                                                                                8:2    0   256M  0 part /boot
└─sda3                                                                                                8:3    0 465.5G  0 part /
sdb                                                                                                   8:16   0 232.9G  0 disk 
├─sdb1                                                                                                8:17   0     1M  0 part 
├─sdb2                                                                                                8:18   0   500M  0 part 
├─sdb3                                                                                                8:19   0 200.2G  0 part 
├─sdb4                                                                                                8:20   0  10.8G  0 part 
├─sdb5                                                                                                8:21   0  10.8G  0 part 
└─sdb6                                                                                                8:22   0  10.8G  0 part 
sdc                                                                                                   8:32   0   3.7T  0 disk /figoraid
sdd                                                                                                   8:48   0   3.7T  0 disk 
└─ceph--78c23580--df79--4bad--b437--7e1d88b73f41-osd--block--5c140ed9--b126--4906--9db3--7ed9357e385f
                                                                                                    252:0    0   3.7T  0 lvm  
sde                                                                                                   8:64   0   3.7T  0 disk 
sdf                                                                                                   8:80   0   3.7T  0 disk 
md0                                                                                                   9:0    0         0 md   
```
We can't just copy and paste that as is because the logical volume name and volume group name are both mashed together.  However we still had the abortive osd.3 creation attempt showing both names separately.  We just have to know how to mash it together for the `--data` directive to pick up the block device properly.  We can look at how the `block` softlink is set up on any of the running osd servers on thufir to figure out how to specify it properly.

`root #``ls -l /var/lib/ceph/osd/ceph-0`
total 48
-rw-r--r-- 1 ceph ceph 393 Mar 27 22:42 activate.monmap
lrwxrwxrwx 1 ceph ceph  93 Mar 27 22:42 block -> /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735
-rw-r--r-- 1 ceph ceph   2 Mar 27 22:42 bluefs
-rw-r--r-- 1 ceph ceph  37 Mar 27 22:42 ceph\_fsid
-rw-r--r-- 1 ceph ceph  37 Mar 27 22:42 fsid
-rw------- 1 ceph ceph  56 Mar 27 22:42 keyring
-rw-r--r-- 1 ceph ceph   8 Mar 27 22:42 kv\_backend
-rw-r--r-- 1 ceph ceph  21 Mar 27 22:42 magic
-rw-r--r-- 1 ceph ceph   4 Mar 27 22:42 mkfs\_done
-rw-r--r-- 1 ceph ceph  41 Mar 27 22:42 osd\_key
-rw-r--r-- 1 ceph ceph   6 Mar 27 22:42 ready
-rw-r--r-- 1 ceph ceph  10 Mar 27 22:42 type
-rw-r--r-- 1 ceph ceph   2 Mar 27 22:42 whoami



`root #``ceph-volume prepare --data /dev/ceph-78c23580-df79-4bad-b437-7e1d88b73f41/osd-block-5c140ed9-b126-4906-9db3-7ed9357e385f`
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new 0e2e5166-1b57-47bf-a39a-88f376db6317
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: mount -t tmpfs tmpfs /var/lib/ceph/osd/ceph-3
--> Absolute path not found for executable: restorecon
--> Ensure $PATH environment variable contains common executable locations
Running command: chown -h ceph:ceph /dev/ceph-78c23580-df79-4bad-b437-7e1d88b73f41/osd-block-5c140ed9-b126-4906-9db3-7ed9357e385f
Running command: chown -R ceph:ceph /dev/dm-0
Running command: ln -s /dev/ceph-78c23580-df79-4bad-b437-7e1d88b73f41/osd-block-5c140ed9-b126-4906-9db3-7ed9357e385f /var/lib/ceph/osd/ceph-3/block
Running command: ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring mon getmap -o /var/lib/ceph/osd/ceph-3/activate.monmap
 stderr: got monmap epoch 1
Running command: ceph-authtool /var/lib/ceph/osd/ceph-3/keyring --create-keyring --name osd.3 --add-key (redacted key)
 stdout: creating /var/lib/ceph/osd/ceph-3/keyring
added entity osd.3 auth auth(auid = 18446744073709551615 key=(redacted key) with 0 caps)
Running command: chown -R ceph:ceph /var/lib/ceph/osd/ceph-3/keyring
Running command: chown -R ceph:ceph /var/lib/ceph/osd/ceph-3/
Running command: /usr/bin/ceph-osd --cluster ceph --osd-objectstore bluestore --mkfs -i 3 --monmap /var/lib/ceph/osd/ceph-3/activate.monmap --keyfile - --osd-data /var/lib/ceph/osd/ceph-3/ --osd-uuid 0e2e5166-1b57-47bf-a39a-88f376db6317 --setuser ceph --setgroup ceph
--> ceph-volume lvm prepare successful for: ceph-78c23580-df79-4bad-b437-7e1d88b73f41/osd-block-5c140ed9-b126-4906-9db3-7ed9357e385f

Moving on to the creation of the `osd.3` service, the author ponders thoughts about just how much of a mess lvm is when things don't quite go the right way the first time with things like wrapper scripts.  After it is spun up and reweighted, the host figo now shows up in the crush map.

`root #``ceph osd tree`
ID CLASS WEIGHT   TYPE NAME       STATUS REWEIGHT PRI-AFF 
-1       16.00000 root default                            
-5        4.00000     host figo                           
 3   hdd  4.00000         osd.3       up  1.00000 1.00000 
-3       12.00000     host thufir                         
 0   hdd  4.00000         osd.0       up  1.00000 1.00000 
 1   hdd  4.00000         osd.1       up  1.00000 1.00000 
 2   hdd  4.00000         osd.2       up  1.00000 1.00000

With lvm started in the first place, the creation of `osd.4` using /dev/sde and `osd.5` using /dev/sdf just like they did for `osd.1` and `osd.2` on thufir.  The ceph status and osd tree now look like the following before we move on to the next two osd servers which will be on kroll7 (mike)

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 6 osds: 6 up, 6 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   6.03GiB used, 21.8TiB / 21.8TiB avail
    pgs:     
```
`root #``ceph osd tree`
ID CLASS WEIGHT   TYPE NAME       STATUS REWEIGHT PRI-AFF 
-1       24.00000 root default                            
-5       12.00000     host figo                           
 3   hdd  4.00000         osd.3       up  1.00000 1.00000 
 4   hdd  4.00000         osd.4       up  1.00000 1.00000 
 5   hdd  4.00000         osd.5       up  1.00000 1.00000 
-3       12.00000     host thufir                         
 0   hdd  4.00000         osd.0       up  1.00000 1.00000 
 1   hdd  4.00000         osd.1       up  1.00000 1.00000 
 2   hdd  4.00000         osd.2       up  1.00000 1.00000

#### Setting up osd.6 and osd.7 on kroll7 (mike)

We jump back on mater to rsync over /etc/ceph and /var/lib/ceph/bootstrap-osd just like we did with figo.

`root #``rsync -av --delete /etc/ceph/ kroll7:/etc/ceph/``root #``rsync -av /var/lib/ceph/bootstrap-osd/ kroll7:/var/lib/ceph/bootstrap-osd/`
Then we jump over to mike and spin up lvm before making any more mistakes with creating osds.

`root #``rc-update add lvm boot`
\* service lvm added to runlevel boot

`root #``rc-update add lvm-monitoring default``root #``/etc/init.d/lvm start`
\* /run/lvm: creating directory
 \* Starting lvmetad ...                                                                                                           \[ ok \]
 \* Setting up the Logical Volume Manager ...                                                                                      \[ ok \]

`root #``/etc/init.d/lvm-monitoring start`
\* Starting dmeventd ...                                                                                                          \[ ok \]
 \* Starting LVM monitoring for VGs : ...                                                                                          \[ ok \]

Mike has a pair of 4tb drives in a btrfs mirror set that was being used for VM storage that was no longer needed. So we unmounted the filesystem and pulled it out of the /etc/fstab.

`root #``btrfs fi show````
Label: 'ROOT'  uuid: df5549f4-b8f8-4576-83e9-bcad1290c344
        Total devices 1 FS bytes used 35.94GiB
        devid    1 size 292.72GiB used 61.01GiB path /dev/sda3
Label: 'HOME'  uuid: ac7487cb-d8cd-4c88-ab5c-9dffa3497a69
        Total devices 1 FS bytes used 165.84GiB
        devid    1 size 619.01GiB used 216.02GiB path /dev/sda6
Label: 'mikeraid'  uuid: b723413e-f575-45dd-8214-c103a65b3066
        Total devices 2 FS bytes used 31.57GiB
        devid    1 size 3.64TiB used 33.01GiB path /dev/sdb
        devid    2 size 3.64TiB used 33.01GiB path /dev/sdc
```
/dev/sdb and /dev/sdc will be recycled for use with `osd.6` and `osd.7` respectively.  With lvm already running, the creation of these two goes smoothly, and the cluster looks like the following before we jump over to tube to make the last osd.

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 8 osds: 8 up, 8 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   8.05GiB used, 29.1TiB / 29.1TiB avail
    pgs:     
```
`root #``ceph osd tree`
ID CLASS WEIGHT   TYPE NAME       STATUS REWEIGHT PRI-AFF 
-1       32.00000 root default                            
-5       12.00000     host figo                           
 3   hdd  4.00000         osd.3       up  1.00000 1.00000 
 4   hdd  4.00000         osd.4       up  1.00000 1.00000 
 5   hdd  4.00000         osd.5       up  1.00000 1.00000 
-7        8.00000     host mike                           
 6   hdd  4.00000         osd.6       up  1.00000 1.00000 
 7   hdd  4.00000         osd.7       up  1.00000 1.00000 
-3       12.00000     host thufir                         
 0   hdd  4.00000         osd.0       up  1.00000 1.00000 
 1   hdd  4.00000         osd.1       up  1.00000 1.00000 
 2   hdd  4.00000         osd.2       up  1.00000 1.00000



#### Setting up osd.8 on kroll4 (tube)

tube used to have a pair of 4tb drives used as the btrfs mirror set for the old `osd.3` server.  While we could press both into service as new BlueFS based osd servers, we only need one.  Ceph would use the crush map to spread PGs out evenly to all 10 of the resulting osd servers, but it would not take fullest use of their storage capacity unless the total number of drives (all of equal size in this case) is a multiple of the replica count (which we had set to three).  Thus we only need 9 osd servers for the moment and would only jump up to the next multiple at 12 if we had enough spare 4tb drives available on hosts (which we don't at the moment).

`root #``btrfs fi show````
Label: 'OCZROOT'  uuid: 64ed0927-023f-49df-9268-44b1b15fc1fd
        Total devices 1 FS bytes used 22.62GiB
        devid    1 size 238.22GiB used 29.02GiB path /dev/sda3
Label: 'cephosd3'  uuid: c4aac3db-16ea-489d-b636-2cb67769c43a
        Total devices 2 FS bytes used 1.77TiB
        devid    1 size 3.64TiB used 1.77TiB path /dev/sdc
        devid    2 size 3.64TiB used 1.77TiB path /dev/sdb
```

After going through all the motions of rsync from mater and getting lvm running we create `osd.8` to use /dev/sdb.  This time things go south because we had forgotten that tube had been rebuilt from scratch with a new install of Gentoo and thus /var/lib/ceph/osd wasn't created yet.  So this happened.

`root #``ceph-volume lvm prepare --data /dev/sdb`
Running command: /usr/bin/ceph-authtool --gen-print-key
Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new 96745093-a521-42be-920b-0d761144e048
Running command: vgcreate --force --yes ceph-d735846e-3346-4f26-b109-882c77795b2e /dev/sdb
 stdout: Wiping btrfs signature on /dev/sdb.
 stdout: Physical volume "/dev/sdb" successfully created.
 stdout: Volume group "ceph-d735846e-3346-4f26-b109-882c77795b2e" successfully created
Running command: lvcreate --yes -l 100%FREE -n osd-block-96745093-a521-42be-920b-0d761144e048 ceph-d735846e-3346-4f26-b109-882c77795b2e
 stdout: Logical volume "osd-block-96745093-a521-42be-920b-0d761144e048" created.
Running command: /usr/bin/ceph-authtool --gen-print-key
--> Was unable to complete a new OSD, will rollback changes
--> OSD will be fully purged from the cluster, because the ID was generated
Running command: ceph osd purge osd.8 --yes-i-really-mean-it
 stderr: purged osd.8
-->  OSError: \[Errno 2\] No such file or directory: '/var/lib/ceph/osd/ceph-8'

So we parked it out there and then reran the prepare with the hideously complicated lvm volume names for the block device instead of just /dev/sdb again.

`root #``mkdir /var/lib/ceph/osd``root #``chown ceph:ceph /var/lib/ceph/osd``root #``ceph-volume lvm prepare --data /dev/ceph-d735846e-3346-4f26-b109-882c77795b2e/osd-block-96745093-a521-42be-920b-0d761144e048`
Then after all that finally worked, we continued on with the `osd.8` startup and ended up as follows before moving back to mater to finally create an MDS server for ourselves and get some pools and PGs going.

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 9 osds: 9 up, 9 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   9.07GiB used, 32.7TiB / 32.7TiB avail
    pgs:     
```
`root #``ceph osd tree`
ID CLASS WEIGHT   TYPE NAME       STATUS REWEIGHT PRI-AFF 
-1       36.00000 root default                            
-5       12.00000     host figo                           
 3   hdd  4.00000         osd.3       up  1.00000 1.00000 
 4   hdd  4.00000         osd.4       up  1.00000 1.00000 
 5   hdd  4.00000         osd.5       up  1.00000 1.00000 
-7        8.00000     host mike                           
 6   hdd  4.00000         osd.6       up  1.00000 1.00000 
 7   hdd  4.00000         osd.7       up  1.00000 1.00000 
-3       12.00000     host thufir                         
 0   hdd  4.00000         osd.0       up  1.00000 1.00000 
 1   hdd  4.00000         osd.1       up  1.00000 1.00000 
 2   hdd  4.00000         osd.2       up  1.00000 1.00000 
-9        4.00000     host tube                           
 8   hdd  4.00000         osd.8       up  1.00000 1.00000



#### Setting up mds.a on kroll3

The first time we ever installed MDS in the Firefly cluster, we had to go to some blogger's website of a guy who worked with Inktank.  This time around, there is documentation in the [deployment guide](http://docs.ceph.com/en/latest/install/manual-deployment/) and then in the official Ceph documentation for the [creation of a CephFS filesystem](http://docs.ceph.com/en/latest/cephfs/createfs/).  We are only going to create `mds.a` for the time being on kroll3 (mater) but may add standbys on kroll5 (refurb) and/or kroll6 (topshelf) eventually if we do some work on mater.

Paying homage to the good old fashioned way off adding things to the cluster, we edit the /etc/ceph/ceph.conf file again to add a section for `mds.a` on kroll3.

**`/etc/ceph/ceph.conf`**

**edited for new \[mds.a\] section**

```
#
#  New version of ceph.conf for BlueFS rollout on Luminous
#j
[global]
        fsid = fb3226b4-c5ff-4bf3-92a8-b396980c4b71
        cluster = kroll
        public network = 192.168.2.0/24
        auth cluster required = cephx
        auth service required = cephx
        auth client required = cephx
        mon initial members = mon.a, mon.b, mon.c
        mon host = 192.168.2.1:6789, 192.168.2.5:6789, 192.168.2.6:6789
#
#  Ceph example conf file suggests 1gb journal but experience shows that it
#  should be more like 10gb.  The point is moot because BlueFS will not use
#  one.  Ceph site osd documentation suggests a 5gb journal setting still.
#
        osd journal size = 10240
#
#  Set replica count to 3 with a minimum of two PGs to be available from OSDs
#  to do an initial write
#
        osd default size = 3
        osd defualt min size = 2
#
#  PG is glossed over in the example conf file but looked at in more detail at
#  http://docs.ceph.com/en/latest/rados/operations/placement-groups/#choosing-number-of-placement-groups
#  The preselection rule of thumb section there suggests pg=128 for 5 osds or
#  less, pg=512 for 5-10 osds and pg=1024 for between 10-50 osds
#
#  Actually, looking at their example, the author suspects that someone hasn't
#  bothered to revisit the 333 number there since Inktank had a different idea
#  for settings in the earlier days.  Their 3 osd example should only have this
#  at 128.  kroll will initially have 8 osd servers on three hosts so will use
#  512
#
        osd pool default pg num = 512
        osd pool default pgp num = 512
#
#  It's not documented well on the ceph site, but this is the crushmap setting
#  where you can get over the having 3 replicas with only 1 to 2 hosts.
#  Set it to 0 if you have fewer nodes than replicas (ie only 1 osd
#  host)
#
#  An entry from a random blog author confirms this.  Also, this link at
#  serverfault shows the pulling of an operational crush map and having up to
#  ten different types available in it with anything greater than 2 having
#  meaning only for enormous clusters like CERN.
#
#  https://serverfault.com/questions/845927/is-ceph-replication-based-on-nodes-or-disks
#
        osd crush chooseleaf type = 1
[mds.a]
        host = kroll3
```
It isn't really necessary to push this version of the /etc/ceph/ceph.conf around to the other hosts from kroll3 (mater) because the new mds server will be running on it directly. We may do it later if we create the standy servers or have other tweaks that need to get passed around.

Like the mgr daemons, the mds just needs to have a keyring created for it in /var/lib/ceph/mds/ceph-a which then gets injected into the MON quorum with various access privileges that will be needed by `mds.a`.  We need to reset ownership of the tree to ceph or else the daemon will run into permission problems when it tries to start and read its /var/lib/ceph/mds/ceph-a/keyring file.

`root #``mkdir -p /var/lib/ceph/mds/ceph-a``root #``ceph-authtool --create-keyring /var/lib/ceph/mds/ceph-a/keyring --gen-key -n mds.a``root #``ceph auth add mds.a osd "allow rwx" mds "allow" mon "allow profile mds" -i /var/lib/ceph/mds/ceph-a/keyring``root #``chown -R ceph:ceph /var/lib/ceph/mds`
Notice we did `ceph auth` there instead of using the ceph-authtool to create a file.  The results ended up in the rocksdb databases on the MON quorum now that the cluster is fully functional.  We can pull the mds authorizations from the MON quorum as follows to verify it.

`root #``ceph auth get mds.a````
exported keyring for mds.a
[mds.a]
        key = (redacted key)
        caps mds = "allow"
        caps mon = "allow profile mds"
        caps osd = "allow rwx"
```
The MDS creation section in the [deployment guide](http://docs.ceph.com/en/latest/install/manual-deployment/) was a bit murky about how to run the new daemon and obsessed over keys.  This author had forgotten to reset ownership of the mds tree on the first attempt and had to looked at the daemon's stderr log out in /var/log/ceph to find the permissions issue.

`root #``cd /etc/init.d``root #``ln -s ceph ceph-mds.a``root #``rc-update add ceph-mds.a default``root #``./ceph-mds.a start`
We haven't set up any PG pools for the CephFS filesystem yet.  Until that happens, the `mds.a` will startup and go into standby mode.

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 9 osds: 9 up, 9 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   9.07GiB used, 32.7TiB / 32.7TiB avail
    pgs:     
```
`root #``ceph mds stat`
, 1 up:standby



#### Setting the noout flag

Up until this point, adding and removing osd servers was a very quick process since there weren't any pools filled with data yet. Once we create this filesystem and start adding data to it, the acts of changing osd servers or tweaking the crush map will cause a migration process to kick in which will transfer shards around from old PGs to newly created ones, and possibly move PGs from one osd server to another on a totally different host. Depending on how much junk you have in the cluster, that can take quite a while. The author recalls a number of times when this process would take a day or even longer on a fully loaded cluster.

One Pro tip that was learned involves the automatic marking of an osd server as out if it has been down long enough (some period of minutes longer than what would be expected on a normal reboot). When this happens, the PGs that are on that server start to go through a migration to the other osd servers in order to get the replica count that is readily available back up to 3 or whatever setting you have. If the osd comes back up again during the process, the migration will begin to reverse by deleting the newly relocated PGs on other hosts when the old PGs are found back online again.

For smallish clusters such as ours, this can be a bit disruptive to operations and performance, especially when we only have four hosts. The temporary shutdown of one may take out a significant portion of the osd servers in one shot and cause a mess when the used storage is approaching the cluster's total capacity.  In order to avoid these issues, we override the automatic outing process by setting the `noout` flag on the cluster.

{{RootCmd|ceph osd set noout|output=

noout is set

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_WARN
            noout flag(s) set
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 9 osds: 9 up, 9 in
         flags noout
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0B
    usage:   9.07GiB used, 32.7TiB / 32.7TiB avail
    pgs:     
```


#### Creating our first pools and the CephFS filesystem

We move on to the Ceph documentation for [creating a new filesystem](http://docs.ceph.com/en/latest/cephfs/createfs/).  In the earlier versions, this had been done for us automatically since there was initially only one filesystem at a time possible in the cluster.  Now we have to create data and metadata pools for ourselves for the new CephFS filesystem.

As you may have noted in our /etc/ceph/ceph.conf file, we have decided as a rule of thumb for the number of osd servers that we have to got with a total of 512 PGs total in the cluster. The vast majority of traffic will be for our CephFS filesystem with a smaller amount going to RBD and virtual machines eventually. John Spray, one of the authoritative experts at Inktank on the MDS server estimates as a rule of thumb that the CephFS metadata and data pool sizes probably should be in a 1:4 ratio. So we will divide up the 512 number into 8 segments, giving one 64 PG allocation to the metadata pool, 384 PGs to the data pool and the remaining 64 will be left for the RBD pool when we get around to creating it.

`root #``ceph osd pool create cephfs_data 384`
pool 'cephfs\_data' created

`root #``ceph osd pool create cephfs_metadata 64`
pool 'cephfs\_metadata' created

`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_WARN
            noout flag(s) set
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    osd: 9 osds: 9 up, 9 in
         flags noout
 
  data:
    pools:   2 pools, 448 pgs
    objects: 0 objects, 0B
    usage:   9.07GiB used, 32.7TiB / 32.7TiB avail
    pgs:     448 active+clean
```
`root #``ceph fs new cephfs cephfs_metadata cephfs_data`
new fs with metadata pool 2 and data pool 1

`root #``ceph fs ls`
name: cephfs, metadata pool: cephfs\_metadata, data pools: \[cephfs\_data \]

At this point, `mds.a` has gone from standby to active and we are actually starting to see some io out on the cluster.

`root #``ceph mds stat````
cephfs-1/1/1 up  {0=a=up:active}
```
`root #``ceph -s````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_WARN
            noout flag(s) set
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    mds: cephfs-1/1/1 up  {0=a=up:active}
    osd: 9 osds: 9 up, 9 in
         flags noout
 
  data:
    pools:   2 pools, 448 pgs
    objects: 21 objects, 2.19KiB
    usage:   9.08GiB used, 32.7TiB / 32.7TiB avail
    pgs:     448 active+clean
 
  io:
    client:   635B/s wr, 0op/s rd, 4op/s wr
```

At this point you may want to open a new tab in your `konsole` or other shell and start a `ceph -w` command running.  It will update whenever the MON quorum switches epochs from event activity.  When you start it, it will also show recent events such as here when our `mds.a` decided to go active.

`root #``ceph -w````
  cluster:
    id:     fb3226b4-c5ff-4bf3-92a8-b396980c4b71
    health: HEALTH_WARN
            noout flag(s) set
 
  services:
    mon: 3 daemons, quorum mon.a,mon.b,mon.c
    mgr: a(active), standbys: b, c
    mds: cephfs-1/1/1 up  {0=a=up:active}
    osd: 9 osds: 9 up, 9 in
         flags noout
 
  data:
    pools:   2 pools, 448 pgs
    objects: 21 objects, 2.19KiB
    usage:   9.08GiB used, 32.7TiB / 32.7TiB avail
    pgs:     448 active+clean
 
2019-03-28 13:18:03.914871 mon.mon.a [INF] daemon mds.a is now active in filesystem cephfs as rank 0
```


#### Mounting the CephFs filesystem

As usual, the documentation on [Mounting the filesystem](http://docs.ceph.com/en/latest/cephfs/kernel/) doesn't go into much detail about the credentials being used. The client.admin user (or just "admin" for our purposes here) is effectively the root user for ceph.  If you remember, the ceph.client.admin.keyring file you created back in the beginning of the install included allow for mds operations.  However the format of that keyring file will not work with mount.ceph.  We need to copy only the key value itself as the contents of an `/etc/ceph/admin.secret` file.

**`/etc/ceph.client.admin.keyring`**

**existing generated file**

```
[client.admin]
        key = (redacted key text)
        auid = 0
        caps mds = "allow"
        caps mgr = "allow *"
        caps mon = "allow *"
        caps osd = "allow *"
```
**`/etc/admin.secret`**

**hacked up copy of ceph.client.admin with just the key**

```
(redacted key text)
```
We could also have done a `ceph auth ls` to copy and paste the key from its output for the client.admin user.  In our old cluster, we had a dual mds setup on kroll5 and kroll6 that got mounted to /kroll.  When migrating off the data, we created a raid10 btrfs mirror that we manually mounted to an /oldceph mount point and then unmounted /kroll and changed the mountpoint to be a softlink to that so that other programs wouldn't notice that anything had happened.  For our new CephFS filesystem, we created a /newkroll mountpoint and then just copy and pasted from the old stanza to create the newkroll one.  Since we could have more than one CephFS filesystem defined now, we use the new mds\_namespace option with the one that we just created that we simply called cephfs.

**`/etc/fstab`**

**hacked up fstab for /newkroll**

```
# /etc/fstab: static file system information.
#
# noatime turns off atimes for increased performance (atimes normally aren't 
# needed); notail increases performance of ReiserFS (at the expense of storage 
# efficiency).  It's safe to drop the noatime options if you want and to 
# switch between notail / tail freely.
#
# The root filesystem should have a pass number of either 0 or 1.
# All other filesystems should have a pass number of 0 or greater than 1.
#
# See the manpage fstab(5) for more information.
#
# <fs>			<mountpoint>	<type>		<opts>		<dump/pass>
# NOTE: If your BOOT partition is ReiserFS, add the notail option to opts.
#
# NOTE: Even though we list ext4 as the type here, it will work with ext2/ext3
#       filesystems.  This just tells the kernel to use the ext4 driver.
#
# NOTE: You can use full paths to devices like /dev/sda3, but it is often
#       more reliable to use filesystem labels or UUIDs. See your filesystem
#       documentation for details on setting a label. To obtain the UUID, use
#       the blkid(8) command.
/dev/sda2		/boot		vfat		defaults,noatime		1 2
/dev/sda5		/		btrfs		defaults,ssd,noatime		0 0
#LABEL=swap		none		swap		sw		0 0
#/dev/cdrom		/mnt/cdrom	auto		noauto,ro	0 0
#kroll1,kroll5,kroll6:/	/kroll	ceph		name=admin,secretfile=/etc/ceph/admin.secret,defaults,noatime,noauto	0 0
kroll1,kroll5,kroll6:/	/newkroll	ceph		name=admin,secretfile=/etc/ceph/admin.secret,mds_namespace=cephfs,defaults,noatime,noauto	0 0
none		/var/tmp/portage	tmpfs		defaults,noatime,noauto  0 0
```
`root #``mount /newkroll``root #``df`
Filesystem                              1K-blocks       Used   Available Use% Mounted on
/dev/sda5                               222700708  128175664    92639664  59% /
devtmpfs                                    10240          0       10240   0% /dev
tmpfs                                     3292092       3768     3288324   1% /run
cgroup\_root                                 10240          0       10240   0% /sys/fs/cgroup
shm                                      16460460     270804    16189656   2% /dev/shm
/dev/sda2                                  523248      87084      436164  17% /boot
none                                     16460460          4    16460456   1% /run/user/0
/dev/sdb                               7814037168 5125596404  2687642300  66% /oldceph
/dev/loop0                                 891904     891904           0 100% /mnt/thumb
none                                     16460460         28    16460432   1% /run/user/501
192.168.2.1,192.168.2.5,192.168.2.6:/ 11131817984          0 11131817984   0% /newkroll

We can now kick off a huge rsync of /kroll to /newkroll on mater.  As that happens, the `ceph -s` command will show io statistics.  In the past, the `ceph -w` command used to update frequently with iops statistics, but that no longer happens.



#### Getting OSD servers to survive reboots with OpenRC

Adding a file to /etc/conf.d for each osd server on a host is necessary for persistence. In it, setting the `bluestore_osd_fsid` variable to the `osd fsid` associated with the osd.  The `ceph-volume lvm activate` in the script will then do the necessary tmpfs setup in /var/lib/ceph/osd to boot up the osd server again.  The `osd fsid` can be found using ceph-volume such as the following report on thufir for its three osds:

`root #``ceph-volume lvm list````
====== osd.1 =======
  [block]    /dev/ceph-1e733afa-e1b4-45a1-b5fe-031eb25ca379/osd-block-ecfc1e8d-f21d-46cb-93f8-ad1065d4542a
      type                      block
      osd id                    1
      cluster fsid              fb3226b4-c5ff-4bf3-92a8-b396980c4b71
      cluster name              ceph
      osd fsid                  ecfc1e8d-f21d-46cb-93f8-ad1065d4542a
      encrypted                 0
      cephx lockbox secret      
      block uuid                ROYrNM-39HC-jJMB-1NQq-7Qc5-uBiS-dFs3Hs
      block device              /dev/ceph-1e733afa-e1b4-45a1-b5fe-031eb25ca379/osd-block-ecfc1e8d-f21d-46cb-93f8-ad1065d4542a
      vdo                       0
      crush device class        None
      devices                   /dev/sdd
====== osd.0 =======
  [block]    /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735
      type                      block
      osd id                    0
      cluster fsid              fb3226b4-c5ff-4bf3-92a8-b396980c4b71
      cluster name              ceph
      osd fsid                  a23b9c03-bc6c-4c11-844d-f95eb8a5aeae
      encrypted                 0
      cephx lockbox secret      
      block uuid                WWMl2Q-DjHo-xg1k-taRs-e9Sk-C4e9-RXXFBK
      block device              /dev/ceph-a7287028-07de-4b3d-a814-f8b5ccd1305a/osd-block-932520d8-ee14-4d3d-9b60-4f1ea6c52735
      vdo                       0
      crush device class        None
      devices                   /dev/sdc
====== osd.2 =======
  [block]    /dev/ceph-24768615-7db1-4c1a-a278-65dfdd83d43c/osd-block-cacbb3da-0e23-4a99-944f-1ea8c41f7e2b
      type                      block
      osd id                    2
      cluster fsid              fb3226b4-c5ff-4bf3-92a8-b396980c4b71
      cluster name              ceph
      osd fsid                  cacbb3da-0e23-4a99-944f-1ea8c41f7e2b
      encrypted                 0
      cephx lockbox secret      
      block uuid                GVJgeE-roX2-W2ge-2Dhz-Xs9b-Q20g-OX1keH
      block device              /dev/ceph-24768615-7db1-4c1a-a278-65dfdd83d43c/osd-block-cacbb3da-0e23-4a99-944f-1ea8c41f7e2b
      vdo                       0
      crush device class        None
      devices                   /dev/sde
```

Based on the example, three osd config files in /etc/conf.d are needed:

**`/etc/conf.d/ceph-osd.0`**

**config file with fsid setting for osd 0**

```
bluestore_osd_fsid=a23b9c03-bc6c-4c11-844d-f95eb8a5aeae
```
**`/etc/conf.d/ceph-osd.1`**

**config file with fsid setting for osd 1**

```
bluestore_osd_fsid=ecfc1e8d-f21d-46cb-93f8-ad1065d4542a
```
**`/etc/conf.d/ceph-osd.2`**

**config file with fsid setting for osd 2**

```
bluestore_osd_fsid=cacbb3da-0e23-4a99-944f-1ea8c41f7e2b
```
#### Some Tunables to look at

Ceph relies on a network backbone, so we have learned a few things over time that need to be tweaked in the Kernel for network performance. The following section in /etc/sysctl.conf came from one or more bloggers doing tuning exercises, but the author can't recall the origins since it's been a few years. There may be some more eventually as we get some more experience with Luminous and bluestore. Also don't forget to tweak /etc/security/limits.conf to kick up the open file limits settings from the ridiculously small kernel defaults.

**`/etc/sysctl.conf`**

**TCP and UDP tweaks for Ceph**

```
#
#  tcp/udp perf tuning from ceph pov
#
#################################################
# Increase Linux autotuning TCP buffer limits
# Set max to 16MB for 1GE and 32M (33554432) or 54M (56623104) for 10GE
# Don't set tcp_mem itself! Let the kernel scale it based on RAM.
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.core.rmem_default = 16777216
net.core.wmem_default = 16777216
net.core.optmem_max = 40960
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216
# Make room for more TIME_WAIT sockets due to more clients,
# and allow them to be reused if we run out of sockets
# Also increase the max packet backlog
net.core.somaxconn = 1024
# Make room for more TIME_WAIT sockets due to more clients,
# and allow them to be reused if we run out of sockets
# Also increase the max packet backlog
net.core.netdev_max_backlog = 50000
net.ipv4.tcp_max_syn_backlog = 30000
net.ipv4.tcp_max_tw_buckets = 2000000
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 10
#
#  udp bumps
#
net.ipv4.udp_rmem_min = 8192
net.ipv4.udp_wmem_min = 8192
##################################################
```


**`/etc/security/limits.conf`**

**better open file descriptor limits**

```
*               soft    nofile          75000
*               hard    nofile          100000
```
