<!-- source: https://wiki.gentoo.org/wiki/Lmctfy | group: Gentoo Wiki (Main) | wiki-title: Lmctfy -->
---
title: Lmctfy
url: https://wiki.gentoo.org/wiki/Lmctfy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-29"
fingerprint: ab120dfbdddd39a7
license: CC BY-SA 4.0
---

# Lmctfy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This article explains how to install and configure lmctfy, Google's open-source container support. This page is very much a WIP.

## Installation

### Kernel

A kernel that supports cgroups is required in order to make lmctfy work. The following setup enables lmctfy on linux-3.10.7-gentoo-r1.

### Userspace

The following code exists in the "palmer" overlay, available from [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository):

`root #``emerge --ask lmctfy`
## Configuration

The following commands setup a simple lmctfy container limeted to 100MB of memory and then proceeds to start a process that attempts to allocate 200MB of memory, which should be killed.

`root #````
lmctfy init ""
```
`root #````
lmctfy create memory_only "memory:{limit:100000000}"
```
`root #````
lmctfy run memory_only "memtester 200M"
```
