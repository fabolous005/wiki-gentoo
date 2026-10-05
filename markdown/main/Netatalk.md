<!-- source: https://wiki.gentoo.org/wiki/Netatalk | group: Gentoo Wiki (Main) | wiki-title: Netatalk -->
---
title: netatalk
url: https://wiki.gentoo.org/wiki/Netatalk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-02"
fingerprint: bc665d1f3f04c9ef
license: CC BY-SA 4.0
---

# netatalk

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Netatalk** is a free, open-source implementation of the [Apple Filing Protocol](https://en.wikipedia.org/wiki/Apple_Filing_Protocol) (AFP). It allows Unix-like operating systems to serve as file, print and time servers for [Macintosh](https://en.wikipedia.org/wiki/Macintosh) computers.

## Installation

### Emerge

Install [net-fs/netatalk](https://packages.gentoo.org/packages/net-fs/netatalk):

`root #``emerge --ask net-fs/netatalk`
## Configuration

In this example, the icon model that appears on clients is a Mac Pro and all private IPv4 hosts are allowed access to a Time Machine volume.

Create the /mnt/storage/TimeMachine folder:

`root #``mkdir /mnt/storage/TimeMachine`
Allow all users read+write access. In practice a more secure configuration should be used with uam list = "uams\_dhx.so uams\_dhx2.so"

`root #``chmod 777 /mnt/storage/TimeMachine`
Edit /etc/afp.conf:

**`/etc/afp.conf`**

## Usage

### Services

To start netatalk:

`root #``/etc/init.d/netatalk start`
To start netatalk at boot:

`root #``rc-update add netatalk default`
