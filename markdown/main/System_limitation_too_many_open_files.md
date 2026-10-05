<!-- source: https://wiki.gentoo.org/wiki/System_limitation_too_many_open_files | group: Gentoo Wiki (Main) | wiki-title: System limitation too many open files -->
---
title: System limitation too many open files
url: https://wiki.gentoo.org/wiki/System_limitation_too_many_open_files
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-09"
fingerprint: "557abe6a63c78601"
license: CC BY-SA 4.0
---

# System limitation too many open files

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Problem Description

Some packages open many files at the same time during build time. The Gentoo default setting on 2024-02-09 is 1024.

`user $``ulimit -n`
### Tracking problematic packages

Reports about packages which exceeded the limit of max open files are tracked in [bug #924186](https://bugs.gentoo.org/show_bug.cgi?id=924186)

### Solutions

- As user one can increase the max number of open files

`root #``ulimit -n 4096`
