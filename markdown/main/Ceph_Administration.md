<!-- source: https://wiki.gentoo.org/wiki/Ceph/Administration | group: Gentoo Wiki (Main) | wiki-title: Ceph/Administration -->
---
title: Ceph/Administration
url: https://wiki.gentoo.org/wiki/Ceph/Administration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: abb8cb6605d585c0
license: CC BY-SA 4.0
---

# Ceph/Administration

[Ceph](https://wiki.gentoo.org/wiki/Ceph)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As the title suggests, this article focuses on the administration of various services inside a Ceph cluster.

## File system

A Ceph file system requires two pools to start with. One pool contains the data while another pool is meant for the metadata. During the installation these pools should already be created:

`root #``ceph osd lspools`
11 data,12 metadata,

If this is not the case, create the pools:

`root #``ceph osd pool create data 128``root #``ceph osd pool create metadata 128`
With these pools available, a file system can be created. First make sure that no file system already exists:

`root #``ceph fs ls`
name: cephfs, metadata pool: metadata, data pools: \[data \]

If it already exists, then no action needs to be undertaken anymore. Otherwise the file system can be created:

`root #``ceph fs new cephfs metadata data`
To remove a file system, it is necessary to first fail the MDS service:

`root #``ceph mds fail 0``root #``ceph fs rm cephfs`
When a file system exists, it can be mounted on the Linux clients that participate in the cluster. With the Cephx authentication it is necessary to pass on the client name (the `client.admin` one which is created when the cluster is created can be used, but a less privileged one can be used as well) with the key (usually by referring to the secret file created when creating the user key):

`root #``mount -t ceph -o name=admin,secretfile=/etc/ceph/ceph.client.admin.secret host1,host2:/ /srv/ceph`
## Pools

Pools can be created and manipulated immediately. However, when removing a pool, make sure that the pool is not used as all data is irrevocably removed from the cluster.

To list the current set of pools, use `ceph osd lspools`:

`root #``ceph osd lspools`
11 data,12 metadata,13 userdata,

To get some more information, such as the number of objects, size, etc. use the `rados` command:

`root #``rados df`
pool name       category                 KB      objects       clones     degraded      unfound           rd        rd KB           wr        wr KB
data            -                          0            0            0            0           0            0            0            0            0
metadata        -                          2           20            0            0           0            0            0           31            8
userdata        -                     109702            1            0            0           0            0            0           27       109702
  total used        14322004           21
  total avail       83099780
  total space      102683912

To create a pool:

`root #``ceph osd pool create s3data 256`
To remove it:

`root #``ceph osd pool delete s3data`
Administrators can take a snapshot of an existing pool (for instance to simplify backup operations so that the backup can take its time, being certain that the (snapshot) pool remains constant).

`root #``ceph osd pool mksnap s3data s3data-20150701`
To remove the snapshot again, use `rmsnap`.

## Placement groups

To get placement group information, use `ceph pg dump`:

`root #``ceph pg dump`
dumped all in format plain
version 570
stamp 2015-07-12 16:50:43.244414
last\_osdmap\_epoch 147
last\_pg\_scan 146
full\_ratio 0.95
nearfull\_ratio 0.85
pg\_stat objects mip     degr    misp    unf     bytes   log     disklog state   state\_stamp     v       reported        up      up\_primary      acting  acting\_primary  last\_scrub      scrub\_stamp     last\_deep\_scrub deep\_scrub\_stamp
13.72   0       0       0       0       0       0       0       0       active+clean    2015-07-12 16:42:29.120819      0'0     147:9   \[0,1\]   0       \[0,1\]   0       0'0     2015-07-12 16:42:21.359576      0'0     2015-07-12 16:42:21.359576
13.73   0       0       0       0       0       0       0       0       active+clean    2015-07-12 16:42:29.122274      0'0     147:9   \[0,1\]   0       \[0,1\]   0       0'0     2015-07-12 16:42:21.383445      0'0     2015-07-12 16:42:21.383445
...

To get the placement information for a particular placement group, use `ceph pg map`:

`root #``ceph pg map 13.17`
osdmap e147 pg 13.17 (13.17) -> up \[0,1\] acting \[0,1\]

## Authentication keys

Authentication keys are best managed through `ceph auth`. During the key generation, the capabilities are added so that the cluster knows which operations are supported through the key and which ones aren't.

For instance, to create a key for a user (say `client.chris`) which can manipulate the `s3data` pool:

`root #``ceph auth get-or-create client.chris mon 'allow r' osd 'allow rwx pool=s3data' > /home/chris/client.chris.keyring`
The user can use this keyring for the cluster operations. Note that the user still requires read access on the Ceph configuration file, but does not need access (and in fact shouldn't) to the secret files or keyrings inside /etc/ceph.

`root #``rados -n client.chris --keyring /home/chris/client.chris.keyring -p s3data put "objectname" /path/to/file`
To list the current authentication keys:

`root #``ceph auth list`
To remove all privileges from a key:

`root #``ceph auth del client.chris`
