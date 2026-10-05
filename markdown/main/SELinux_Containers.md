<!-- source: https://wiki.gentoo.org/wiki/SELinux/Containers | group: Gentoo Wiki (Main) | wiki-title: SELinux/Containers -->
---
title: SELinux/Containers
url: https://wiki.gentoo.org/wiki/SELinux/Containers
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-01"
fingerprint: "5fabc35d80d66948"
license: CC BY-SA 4.0
---

# SELinux/Containers

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Many container technologies such as [Docker](https://wiki.gentoo.org/wiki/Docker) or [Podman](https://wiki.gentoo.org/wiki/Podman) have various features which can integrate with SELinux at runtime. These features are primarily intended to provide additional isolation to containers. If enabled, SELinux ensures that containers remain isolated not only from the host, but also from each other.

## Introduction

SELinux policy support for containers is provided by the [sec-policy/selinux-container](https://packages.gentoo.org/packages/sec-policy/selinux-container) package as well as the corresponding policy packages for various container technologies. For example, [sec-policy/selinux-docker](https://packages.gentoo.org/packages/sec-policy/selinux-docker) provides policy support for [app-containers/docker](https://packages.gentoo.org/packages/app-containers/docker). The required policy packages will be pulled in automatically as long as the [selinux](https://packages.gentoo.org/useflags/selinux) [USE flag is set.](https://wiki.gentoo.org/wiki/USE_flag)

Generally speaking, most container runtimes (henceforth referred to as "engines" in this article) will take advantage of SELinux as soon as they are installed. However, there are a few cases where some extra configuration is required.

### Docker

### Podman

### CRI-O

## Differences from container-selinux

[container-selinux](https://github.com/containers/container-selinux) is the upstream SELinux policy package providing support for containers on Linux distributions utilizing [fedora-selinux](https://github.com/fedora-selinux/selinux-policy) as the foundation for their SELinux policies. This includes Fedora Linux, Red Hat Enterprise Linux, CentOS, etc.
