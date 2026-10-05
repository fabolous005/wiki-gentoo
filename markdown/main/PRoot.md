<!-- source: https://wiki.gentoo.org/wiki/PRoot | group: Gentoo Wiki (Main) | wiki-title: PRoot -->
---
title: PRoot
url: https://wiki.gentoo.org/wiki/PRoot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-14"
fingerprint: "8009361f5fce4a26"
license: CC BY-SA 4.0
---

# PRoot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

PRoot is a user-space implementation of chroot, mount --bind, and binfmt\_misc.

## Prerequisites

### Setting up the environment

When creating a new rootfs, the first thing needed is a directory for the rootfs to reside in. For example, a rootfs could be created in \~/tmp/gentoo:

`user $````
mkdir -p ~/tmp/gentoo
```
`user $````
cd ~/tmp/gentoo
```
If an installation has been previously created in a sub directory of the current root file system the above steps can be skipped.

### Unpacking system files and the Portage tree (new installations)

When building a new install, the next step is to download the stage3 tarball and unpack it to the rootfs location. For more information on this process please see [Downloading the stage tarball](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#Downloading_the_stage_tarball) and [Unpacking the stage tarball](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#Unpacking_the_stage_tarball) in the Gentoo [Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page).

`user $````
LATEST=$(cat latest-stage3-amd64-openrc.txt | tail -n1 | awk '{ print $1 }')
```
`user $````
curl -LO "http://distfiles.gentoo.org/releases/amd64/autobuilds/${LATEST}"
```
`user $``tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner`
## Installation

### Emerge

`root #``emerge --ask sys-apps/proot`
## Usage

`user $``proot -S ~/tmp/gentoo /bin/bash`
\~ #

## Troubleshooting

`user $``PROOT_NO_SECCOMP=1 proot echo "disable seccomp mode"`
