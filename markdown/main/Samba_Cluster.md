<!-- source: https://wiki.gentoo.org/wiki/Samba/Cluster | group: Gentoo Wiki (Main) | wiki-title: Samba/Cluster -->
---
title: Samba/Cluster
url: https://wiki.gentoo.org/wiki/Samba/Cluster
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: ee5cfd51bd83ac99
license: CC BY-SA 4.0
---

# Samba/Cluster

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

# Introduction

There are changes that you have a high samba share load and you need to have 2 or more samba server to serve the same group of users.

Samba 4 Cluster Member server with drbd and OCFS[\[1\]](https://wiki.gentoo.org#cite_note-samba4-cluster-for-ad-drbd-ocfs2-ctdb-1)

We can do that via CTDB and some cluster files system.

Our guide are focus on version 2.5.4 and above, version 1 is not covered on this guide

## Topology

Below are the minimal requirement for ctdb:

1. 2 node samba DC member/ files server
2. an Extra NIC for ctdb services which have no address (Samba ctdb will take over this nic and assign ip)
3. Each node had one 1 Private IP address for ctdb services (Not use for samba services)
4. A Shared cluster drive OCFS2, GFS or other
5. Not working as Samba DC.

# Getting CTDB

## From Poly-C Eselect Repository

Let hope when this guide is ready the new ctdb-2.5.4 already in portage. [bug #500332](https://bugs.gentoo.org/show_bug.cgi?id=500332)
Else you will need to get from eselect repository on poly-c

`root #``eselect repository enable poly-c`


## Unmaks CTDB

Before this let's unmask ctdb 2.5.4

**`/etc/portage/package.unmask`**

**add this line in**

```
=dev/db-ctdb-2.5.4
```
## Emerge CTDB

`root #``emerge --ask --newuse --verbose dev/db-ctdb`
# Configure CTDB

A Basical running CTDB are simple with the latest configuration

Assumption:

Node 1 eth1 Private IP: 192.168.100.11 (CTDB node IP) 
Node 1 eth2 Public IP: 192.168.10.11 (Servicing IP)
Node 2 eth1 Private IP: 192.168.100.12 (CTDB node IP)
Node 2 eth2 Public IP: 192.168.10.12 (Servicing IP)
Both running on eth2
Running OCFS2

eth2 will need to start without any ip.

**`/etc/conf.d/net`**

**Make some change like below**

```
config_eth2="null"
```
## CTDB Configuration Change

We will need to make a few changes.

**`/etc/ctdb/ctdbd.conf`**

**Make some change like below**

```
CTDB_PUBLIC_ADDRESSES=/etc/ctdb/public_addresses
```
**`/etc/ctdb/nodes`**

**Make some change like below**

```
192.168.100.11
192.168.100.12
```
**`/etc/ctdb/public_addresses`**

**Make some change like below**

```
192.168.10.11/24        eth2
192.168.10.12/24        eth2
```


## Samba Configuration Change

Please change this on both samba server

**`/etc/samba/smb.conf`**

**Add the following line**

```
[global]
        #CTDB Cluster
        netbios name = SambaServerName
        clustering = yes
        ctdbd socket = /var/lib/ctdb/ctdb.socket
        #ocfs2 ctdb setting
        fileid:mapping = fsid
```
We are done on configuration

# Start and Stop CTDB

## Start

`root #``ctdbd_wrapper /var/run/ctdb/ctdbd.pid start`
## Stop

`root #``ctdbd_wrapper /var/run/ctdb/ctdbd.pid stop`
Done
