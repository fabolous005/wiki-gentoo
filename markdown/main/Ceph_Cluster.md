<!-- source: https://wiki.gentoo.org/wiki/Ceph/Cluster | group: Gentoo Wiki (Main) | wiki-title: Ceph/Cluster -->
---
title: Ceph/Cluster
url: https://wiki.gentoo.org/wiki/Ceph/Cluster
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: a4f9fb557cb00399
license: CC BY-SA 4.0
---

# Ceph/Cluster

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A Ceph cluster is the whole of all Ceph components, working together to provide a high-available and reliable shared object storage and file system. All components interact with each other through the network as Ceph provides a "shared-nothing" storage environment - there is no need for a central, shared storage platform such as a SAN.

## Structure

![](https://wiki.gentoo.org/images/thumb/d/da/Ceph_cluster.png/300px-Ceph_cluster.png)

A Ceph cluster needs a number of components to function properly.

- A number of [monitors](https://wiki.gentoo.org/wiki/Ceph/Monitor) (MON) which provide the quorum handling capabilities (operations against the cluster must have a majority of monitors accepting the operation, otherwise the operation is unsuccessful)
- A number of [object store devices](https://wiki.gentoo.org/wiki/Ceph/Object_Store_Device) (OSD) which represent storage areas (and are usually mapped one-to-one on mounted file systems) that Ceph uses to store the objects in
- An (optional) [metadata server](https://wiki.gentoo.org/wiki/Ceph/Metadata_Server) (MDS) that allows for mounting a Ceph POSIX-compliant file system

It is also possible to have gateway services that provide a compatible layer between Ceph and popular cloud services / APIs such as Amazon S3 or OpenStack Swift. This allows users to host their own S3-compatible or Swift-compatible storage.

## Configuration

The configuration file for a Ceph cluster is hosted in /etc/ceph and is named after the cluster name. Most users will call their Ceph cluster `ceph` so the resulting configuration file is called /etc/ceph/ceph.conf. It is possible to manage multiple Ceph clusters from the same host if the administrator takes care to use either different IP addresses (multi-homed systems or interface aliases) or different ports for the various Ceph services.

A configuration file contains `[global]` settings (which are applicable to the entire cluster), component-specific settings such as `[mon]` (which are applicable to all instances of that particular component) and service individual settings such as `[mon.0]` (which are applicable to that particular instance).

**`/etc/ceph/ceph.conf`**

**Sample cluster configuration file**

```
[global]
  fsid = f7cfbe1c-064f-4fb1-b6f6-93fa4764879b
  cluster = ceph
  public network = 192.168.100.0/24
  # Enable cephx authentication (which uses shared keys for almost everything)
  auth cluster required = cephx
  auth service required = cephx
  auth client required = cephx
  # Replication
  osd pool default size = 2
  osd pool default min size = 1
 
[osd]
  osd journal size = 1024
  # Uncomment when mounting with ext4
  filestore xattr use omap = true
 
[mon.0]
  host = test
  mon addr = 192.168.100.153:6789
 
[mon.1]
  host = test2
  mon addr = 192.168.100.161:6789
 
[osd.0]
  host = test
 
[osd.1]
  host = test2
```
Alongside the cluster configuration file, most clusters will also have the authentication keyrings and/or secrets stored in this location. These keyrings are necessary for services to be able to interact with a Cephx-enabled cluster (cephx is the authentication and authorization implementation of Ceph).

Every host that participates in the Ceph cluster should have the same /etc/ceph content. The keyrings *can* be limited to just those hosts that access them, but it is in general more simple to just keep the entire directory synchronized across hosts.

## Querying

To query the state of the cluster, use `ceph -s`:

`root #``ceph -s````
    cluster 8b8a14c3-9535-4309-a8d8-54aa46d59ea6
     health HEALTH_OK
     monmap e1: 3 mons at {0=192.168.100.153:6789/0,1=192.168.100.161:6789/0,2=192.168.100.168:6789/0}, election epoch 2013, quorum 0,1 1,2 0,2
     mdsmap e536: 1/1/1 up {0=0=up:active}
     osdmap e1449: 203 osds: 203 up, 203 in
      pgmap v4164: 13302 pgs, 18 pools, 248354484235962 bytes data, 15473 objects
            248311 GB used, 845532 GB / 1093843 GB avail
                 13302 active+clean
```
This shows that 3 monitor services are up and running, one metadata server, 203 object store devices.
