<!-- source: https://wiki.gentoo.org/wiki/DRBD | group: Gentoo Wiki (Main) | wiki-title: DRBD -->
---
title: DRBD
url: https://wiki.gentoo.org/wiki/DRBD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-10-12"
fingerprint: "579a50dec8f4ab03"
license: CC BY-SA 4.0
---

# DRBD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


**DRBD** (or **Distributed Replicated Block Device**) is a network block device that provides reliability when storing data across multiple network nodes.

From the kernel documentation:

*DRBD is a shared-nothing, synchronously replicated block device. It is designed to serve as a building block for high availability clusters and in this context, is a "drop-in" replacement for shared storage. Simplistically, you could see it as a network RAID 1.*


## Installation

### Kernel

**Enable CONFIG\_BLK\_DEV\_DRBD in the kernel**

```
Device Drivers --->  Block devices --->
<*>   DRBD Distributed Replicated Block Device support
```
### Emerge

Install [sys-cluster/drbd-utils](https://packages.gentoo.org/packages/sys-cluster/drbd-utils):

`root #``emerge --ask sys-cluster/drbd-utils`
This package installs the userland utilities to interact with, and control DRBD. Also known as drbdsetup and drbdadm.

## Troubleshooting

### Errors

"ERROR: unknown cs for drbd0 : BrokenPipe, Update/DUnknown"

This error means connection state has a problem, link needs fixing, or drbd version updating. Upstream states "we had some issues with discarding the first successful connection and getting in a connect/brokenpipe loop."

Run this command to extract useful information:

`root #``cat /proc/drbd`
