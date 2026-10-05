<!-- source: https://wiki.gentoo.org/wiki/Openstack | group: Gentoo Wiki (Main) | wiki-title: Openstack -->
---
title: Openstack
url: https://wiki.gentoo.org/wiki/Openstack
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-12-27"
fingerprint: d5953d49c5b769a8
license: CC BY-SA 4.0
---

# Openstack

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Openstack

The OpenStack project aims to create an open and free IaaS platform.

## Before Installation

#### Controller node

Let's begin from the Controller node. Before installing Openstack specific services the system needs to be configured with two physical NIC, one for the *Management Network* and one to expose the Openstack API to the internet on the *Public Network*, with the following IP configuration:

| Network Interface | IP Address | 
|---|---|
| Public Interface | 1.1.1.1 | 
| Management Interface | 192.168.0.1 | 

#### Network Node

#### Compute Node

## Installation

You will need three Gentoo hosts to fully exploit the power of the OpenStack platform.

### USE flags

*sys-cluster/openstack-meta*correct?

### Emerge

`root #``emerge --ask sys-cluster/openstack-meta`
