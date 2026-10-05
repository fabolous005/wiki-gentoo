<!-- source: https://wiki.gentoo.org/wiki/Subuid_subgid | group: Gentoo Wiki (Main) | wiki-title: Subuid subgid -->
---
title: Subuid subgid
url: https://wiki.gentoo.org/wiki/Subuid_subgid
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-02"
fingerprint: "8d1c735abf3d8919"
license: CC BY-SA 4.0
---

# Subuid subgid

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SubUID/GIDs are a range subordinate user/group IDs that a user is allowed to use. These are commonly used by containerization software, such as [LXD](https://wiki.gentoo.org/wiki/LXD) and [Podman](https://wiki.gentoo.org/wiki/Podman), for creating privilege separated containers.

This article outlines a default configuration of subuid/subgid that should work for most user workloads.

## Overview of subuid/subgid

For setting up the various container software, proper configuration of subuid and subgid is vital. Keep in mind that after an initial configuration, it is not easily possible to change the subuid/gid mappings without starting from scratch and losing existing containers.

In most modern systems with [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc), UID/GIDs can be in the range `0-4,294,967,295 (2^32 - 1)`, which is the maximum `unsigned integer`. In the default LXD configuration (in the absence of /etc/subuid and /etc/subgid), it is assumed that the range `1,000,000-1,000,999,999` is available for LXD to use<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. It is best to ensure this configuration manually, even when not using LXD, so that it is easier to manage subuids/gids for use with other programs such as [podman](https://wiki.gentoo.org/wiki/Podman) and [docker](https://wiki.gentoo.org/wiki/Docker).

If using LXD, it is vital that the subuid/gid ranges for the users `lxd` and `root` are kept in sync. Additionally, for each user on the system, it is best to keep their available subuid/gid ranges distinct and non-overlapping. Creating such a configuration will also help podman in running rootless containers.

### Manual configuration

Available ranges for subuid/gid can be configured by editing the files /etc/subuid and /etc/subgid in a text editor.

**`/etc/subuid`**

**subuid ranges for LXD**

**`/etc/subgid`**

**subgid ranges for LXD**

### usermod

usermod (from [sys-apps/shadow](https://packages.gentoo.org/packages/sys-apps/shadow)) can also be used to programmatically configure ranges for users. For example, the above configuration can also be achieved by a series of commands:

`root #````
usermod --add-subuids 1000000-1000999999 root
```
`root #````
usermod --add-subgids 1000000-1000999999 root
```
`root #````
usermod --add-subuids 1001000000-1001999999 larry
```
`root #````
usermod --add-subgids 1001000000-1001999999 larry
```
`root #````
usermod --add-subuids 1002000000-1002999999 cow
```
`root #````
usermod --add-subgids 1002000000-1002999999 cow
```
`root #````
usermod --add-subuids 1003000000-1003999999 developer
```
`root #````
usermod --add-subgids 1003000000-1003999999 developer
```
## See also

- [subuid(5)](https://man.archlinux.org/man/subuid.5.en)- [subgid(5)](https://man.archlinux.org/man/subgid.5.en)- [LXC](https://wiki.gentoo.org/wiki/LXC) — a [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system that leverages Linux's [namespaces](https://wiki.gentoo.org/wiki/Namespaces) and [cgroups](https://wiki.gentoo.org/wiki/Cgroups) to create containers isolated from the host system
- [LXD](https://wiki.gentoo.org/wiki/LXD) — a system container manager
- [Podman](https://wiki.gentoo.org/wiki/Podman) — a daemonless container engine for developing, managing, and running [OCI Containers](https://opencontainers.org/), aiming to be a drop-in replacement for much of [Docker](https://wiki.gentoo.org/wiki/Docker)
- [Docker](https://wiki.gentoo.org/wiki/Docker) — a [container](<https://en.wikipedia.org/wiki/Container_(virtualization)>)-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system
