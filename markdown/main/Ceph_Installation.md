<!-- source: https://wiki.gentoo.org/wiki/Ceph/Installation | group: Gentoo Wiki (Main) | wiki-title: Ceph/Installation -->
---
title: Ceph/Installation
url: https://wiki.gentoo.org/wiki/Ceph/Installation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: ee98d87edda5b9c0
license: CC BY-SA 4.0
---

# Ceph/Installation

[Ceph](https://wiki.gentoo.org/wiki/Ceph)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

All necessary Ceph software is available through the [sys-cluster/ceph](https://packages.gentoo.org/packages/sys-cluster/ceph) package. It contains all services as well as basic administration utilities for managing a Ceph cluster.

## Design

Before embarking on a Ceph deployment scenario, take the time to make a basic Ceph cluster design.

What is the purpose of the Ceph cluster? Is it to play around and experiment with Ceph? Is it to host all critical data in form of [rbd devices](https://wiki.gentoo.org/wiki/Ceph/Rados_Block_Device)? Is it to create a highly available file server?

What features are needed on the Ceph cluster? How many monitors are likely to be needed? How much storage will be used, and how will this storage be represented (as in, how many OSDs will be available and where will they run)? Will the cluster provide S3- or Swift-like APIs to the outside world?

What are the IP addresses that will be used by the cluster? Ceph requires a static IP environment, so making a well designed network infrastructure is important for Ceph to function properly.

How will the servers be distributed across the environment? Ceph has a number of *buckets* that it can use to differentiate servers and make well-thought-through distribution and replication decisions. The default is an *OSD* on a *host* in a *rack* in a *row* in a *room* inside a *data center*.

There are a number of best practices to account for through:

- Most clusters require 3 monitor servers, perhaps 5. Clusters generally do not need more than 5 monitor servers to function in even the harshest environments.
- Distribute the monitor servers across the environment. If the cluster is over a couple of racks, make sure that the monitor servers are distributed across the racks as well.
- There is usually no need for RAID on the file system that an OSD uses. Instead, rely on the Ceph availability and distribution.
- OSD services do not need a lot of CPU or RAM. A metadata server however does benefit from high-speed CPU and lots of memory.

## Hardware layout

The hardware specification of this example consist of three machines: host1, host2, host3, each has three harddisk, first driver (/dev/sda) for OS installation, second, third (/dev/sdb, /dev/sdb) for OSD service, Ceph Monitor will be deployed at each machine, while Metadata serive will be deployed only at host1

## System configuration

The first configuration to decide on is which Ceph version to deploy. At the time of writing, Ceph version 0.87 ("Giant") is available in the tree in \~arch while version 0.80 ("Firefly") is available as stable release. To use the \~arch version, add [sys-cluster/ceph](https://packages.gentoo.org/packages/sys-cluster/ceph) to package.accept\_keywords:

**`/etc/portage/package.accept_keywords/ceph`**

```
sys-cluster/ceph
```
Next, validate that the Linux kernel is configured to support Ceph.

**Linux kernel configuration for Ceph**

```
Device Drivers --->
  [*] Block devices --->
    <*> Rados block device (RBD)
 
File systems --->
  [*] Network File Systems --->
    <*> Ceph distributed file system
```
## Installation

With the system configuration done, install the Ceph software.

The following USE flags are available for fine-tuning the installation.


### USE flags for
            [sys-cluster/ceph](https://packages.gentoo.org/packages/sys-cluster/ceph)
            
            Ceph distributed filesystem

| [+cephfs](https://packages.gentoo.org/useflags/+cephfs) | Build support for cephfs, a POSIX compatible filesystem built on top of ceph | 
| [+mgr](https://packages.gentoo.org/useflags/+mgr) | Build the ceph-mgr daemon | 
| [+parquet](https://packages.gentoo.org/useflags/+parquet) | Support for s3 select on parquet objects | 
| [+radosgw](https://packages.gentoo.org/useflags/+radosgw) | Add radosgw support | 
| [+sqlite](https://packages.gentoo.org/useflags/+sqlite) | Add support for sqlite - embedded sql database | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [+system-boost](https://packages.gentoo.org/useflags/+system-boost) | Use system dev-libs/boost instead of the bundled one | 
| [+tcmalloc](https://packages.gentoo.org/useflags/+tcmalloc) | Use the dev-util/google-perftools libraries to replace the malloc() implementation with a possibly faster one | 
| [+uring](https://packages.gentoo.org/useflags/+uring) | Build with support for sys-libs/liburing | 
| [babeltrace](https://packages.gentoo.org/useflags/babeltrace) | Add support for LTTng babeltrace | 
| [custom-cflags](https://packages.gentoo.org/useflags/custom-cflags) | Build with user-specified CFLAGS (unsupported) | 
| [diskprediction](https://packages.gentoo.org/useflags/diskprediction) | Enable local diskprediction module to predict disk failures | 
| [dpdk](https://packages.gentoo.org/useflags/dpdk) | Enable DPDK messaging | 
| [fuse](https://packages.gentoo.org/useflags/fuse) | Build fuse client | 
| [grafana](https://packages.gentoo.org/useflags/grafana) | Install grafana dashboards | 
| [jaeger](https://packages.gentoo.org/useflags/jaeger) | Enable jaegertracing and it's dependent libraries | 
| [kafka](https://packages.gentoo.org/useflags/kafka) | Rados Gateway's pubsub support for Kafka push endpoint | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [lttng](https://packages.gentoo.org/useflags/lttng) | Add support for LTTng | 
| [nvmeof](https://packages.gentoo.org/useflags/nvmeof) | Enable support for NVMEoF | 
| [pmdk](https://packages.gentoo.org/useflags/pmdk) | Enable PMDK libraries | 
| [rabbitmq](https://packages.gentoo.org/useflags/rabbitmq) | Use rabbitmq-c to build rgw amqp push endpoint | 
| [rbd-rwl](https://packages.gentoo.org/useflags/rbd-rwl) | Enable librbd persistent write back cache | 
| [rbd-ssd](https://packages.gentoo.org/useflags/rbd-ssd) | Enable librbd persistent write back cache for SSDs | 
| [rdma](https://packages.gentoo.org/useflags/rdma) | Enable RDMA support via sys-cluster/rdma-core | 
| [rgw-lua](https://packages.gentoo.org/useflags/rgw-lua) | Rados Gateway's support for dynamically adding lua packagess | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [spdk](https://packages.gentoo.org/useflags/spdk) | Enable SPDK user-mode storage driver toolkit | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [xfs](https://packages.gentoo.org/useflags/xfs) | Add xfs support | 
| [zbd](https://packages.gentoo.org/useflags/zbd) | Enable sys-block/libzbd bluestore backend | 

With the USE flags defined, install the software:

`root #``emerge --ask sys-cluster/ceph`
## Cluster creation

Use `uuidgen` to generate a cluster id.

`user $``uuidgen`
a0ffc974-222e-449a-a078-121bdfcb110b

Create the basic skeleton for the ceph.conf file, and use the generated id for the `fsid` parameter.

**`/etc/ceph/ceph.conf`**

**Global part in ceph.conf**

```
[global]
  fsid = a0ffc974-222e-449a-a078-121bdfcb110b
  cluster = ceph
  public network = 192.168.100.0/24
  # Enable cephx authentication (which uses shared keys for almost everything)
  auth cluster required = cephx
  auth service required = cephx
  auth client required = cephx
  # Replication
  osd pool default size = 2
  osd pool default min size = 1
```
In this example, a cluster is used with a replication factor of 2 (which means it is replicated once - there are two instances of each block) and a minimum of 1 (i.e. as long as one copy of the data is available, continue).

Next create the administrative key. The default administrative key is called `client.admin`:

`root #````
ceph-authtool --create-keyring /etc/ceph/ceph.client.admin.keyring --gen-key -n client.admin --cap mon 'allow *' --cap osd 'allow *' --cap mds 'allow'
```
`root #``awk '$1~"key" { print $3 }' /etc/ceph/ceph.client.admin.keyring > /etc/ceph/ceph.client.admin.secret``root #``chmod 600 /etc/ceph/ceph.client.admin.secret`
## Monitors

To create the monitors, first add in the information to the ceph.conf file:

**`/etc/ceph/ceph.conf`**

**Snippet for monitors**

```
[mon]
  # Global settings for monitors
  mon host = host1, host2, host3
  mon addr = 192.168.100.10:6789, 192.168.100.11:6789, 192.168.100.12:6789
  mon initial members = 0, 1, 2
[mon.0]
  host = host1
  mon addr = 192.168.100.10:6789
[mon.1]
  host = host2
  mon addr = 192.168.100.11:6789
[mon.2]
  host = host3
  mon addr = 192.168.100.12:6789
```
Next create the keyring for the monitor (so that the Ceph monitors can integrate and interact with the Ceph cluster) and add the administrative keyring to it:

`root #````
ceph-authtool --create-keyring /etc/ceph/ceph.mon.keyring --gen-key -n mon. --cap mon 'allow *' --import-keyring /etc/ceph/ceph.client.admin.keyring
```
Now create the initial monitor map (which is a binary file that the Ceph monitors use to find the default, initial monitor list).

`root #``monmaptool --create --fsid $(uuidgen) --add 0 192.168.100.10 --add 1 192.168.100.11 --add 2 192.168.100.12 /etc/ceph/ceph.initial-monmap`
Create the file system that the monitors will use to keep their information in.

`root #``mkdir -p /var/log/ceph && chown ceph.  /var/log/ceph``root #``mkdir -p /var/lib/ceph/mon && chown ceph. /var/lib/ceph/mon``root #``ceph-mon --mkfs -i 0 --monmap /etc/ceph/ceph.initial-monmap --keyring /etc/ceph/ceph.mon.keyring`
Repeat this step on each system for the right id (`-i 0` becomes `-i 1` etc.)

Finally, create the init script to launch the monitor at boot:

`root #``ln -s /etc/init.d/ceph /etc/init.d/ceph-mon.0``root #``rc-update add ceph-mon.0 default``root #``rc-service ceph-mon.0 start`
Also repeat this on each system for the right id.

## Object store devices

### Adding manually

Get a UUID for intended osd:

`root #````
osd_uuid=$(uuidgen)
```
`root #``echo $osd_uuid`
e33dcfb0-31d5-4953-896d-007c7c295410

Create a new OSD in the cluster:

`root #````
id=$(ceph osd create $osd_uuid)
```
`root #``echo $id`
0

Create the mountpoint on which the data of the OSD will be stored:

`root #``mkdir -p /var/lib/ceph/osd/ceph-$id` Make the filesystem for storing data and mount it (assuming you plan to store data on /dev/{partition}):

`root #````
mkfs.xfs /dev/{partition}
```
`root #``mount -o rw,inode64 /dev/{partition} /var/lib/ceph/osd/ceph-$id`
Also, consider adding that filesystem to fstab like so:

`root #````
 fs_uuid=$(blkid -o value /dev/{partition} | cut -f1 -d$'\n') 
```
`root #````
 echo "UUID=$fs_uuid	/var/lib/ceph/osd/ceph-$id	xfs	rw,inode64	0	0" >> /etc/fstab
```
Then, create the OSD files on it:

`root #``ceph-osd -i $id --mkfs --mkkey --no-mon-config --osd-uuid $osd_uuid`
Change owner and group of that files to `ceph`, otherwise osd would not be able to write anything:

`root #``chown ceph: /var/lib/ceph/osd/ceph-$id -R`
Add the OSD keyring to the clusters' authentication database:

`root #``ceph auth add osd.$id osd 'allow *' mon 'allow profile osd' -i /var/lib/ceph/osd/ceph-$id/keyring`
### Adding via ceph-volume

[Ceph-volume](https://docs.ceph.com/docs/master/ceph-volume/) is a tool for OSD deployment, at the moment recommended by the upstream way to deploy OSDs is through [LVM](https://wiki.gentoo.org/wiki/LVM), enable it before proceeding.
To create new OSD:

`root #``ceph-volume lvm prepare --bluestore --data /dev/{partition} --no-systemd` Mind the id and uuid of the OSD that was created (look for `-i` and `--osd-uuid` keys in invocation of `ceph-osd`). At the moment OSD files are created in the tmpfs, that is mounted over `/var/lib/ceph/osd/ceph-$id/` directory. At the boot these files should be recreated from LVM metadata, to help boot scripts do so:

`root #``echo "bluestore_osd_fsid=$osd_uuid" > /etc/conf.d/ceph-osd.$id` ### Finalizing

Add the current host to the CRUSH map if it is the first OSD of this host that participates in the cluster:

`root #````
ceph osd crush add-bucket $(hostname) host
```
`root #``ceph osd crush move $(hostname) root=default`
Add each OSD to the map with a default weight value:

`root #``ceph osd crush add osd.$id 1.0 host=$(hostname)`
Create the init script for the OSD and have it start at boot:

`root #``ln -s /etc/init.d/ceph /etc/init.d/ceph-osd.$id``root #``rc-update add ceph-osd.$id default``root #``rc-service ceph-osd.$id start`
## Metadata server

Update the ceph.conf information for the MDS:

**`/etc/ceph/ceph.conf`**

**MDS snippet**

```
[mds.0]
  host = host1
```
Create two pools - one for data and one for metadata. The number `128` in the example below is the number of placement groups to assign inside the pool. Tune this correctly depending on the size of the cluster (see [Ceph's placement groups information](http://ceph.com/docs/master/rados/operations/placement-groups/)).

`root #````
ceph osd pool create data 128
```
`root #``ceph osd pool create metadata 128`
Now create a file system that uses these pools. The name of the file system can be chosen freely - the example uses `cephfs`:

`root #``ceph fs new cephfs metadata data`
Create the keyring for the MDS service:

`root #``mkdir -p /var/lib/ceph/mds/ceph-0``root #``ceph auth get-or-create mds.0 mds 'allow' osd 'allow *' mon 'allow rwx' > /var/lib/ceph/mds/ceph-0/keyring`
Create the init script and have it start at boot:

`root #``ln -s /etc/init.d/ceph /etc/init.d/ceph-mds.0``root #``rc-update add ceph-mds.0 default``root #``rc-service ceph-mds.0 start`
