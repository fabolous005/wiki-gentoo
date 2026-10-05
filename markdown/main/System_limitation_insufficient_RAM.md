<!-- source: https://wiki.gentoo.org/wiki/System_limitation_insufficient_RAM | group: Gentoo Wiki (Main) | wiki-title: System limitation insufficient RAM -->
---
title: System limitation insufficient RAM
url: https://wiki.gentoo.org/wiki/System_limitation_insufficient_RAM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-11"
fingerprint: "70fe662683fce72b"
license: CC BY-SA 4.0
---

# System limitation insufficient RAM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Problem Description

When compiling with **n** jobs parallel the requirement of RAM grows approximately by a factor **n**. Unfortunately we can not predict the exact amount of RAM needed nor we can predict how much RAM will be consumed by other programs.

### Tracking problematic packages

Packages with reports about insufficient RAM are tracked in [bug #924185](https://bugs.gentoo.org/show_bug.cgi?id=924185)

### Solutions

- As package maintainer inherit the eclass [check-reqs.eclass](https://devmanual.gentoo.org/eclass-reference/check-reqs.eclass) and set `CHECKREQS_MEMORY`.
- As user one can [limit the number of parallel jobs](https://wiki.gentoo.org/wiki/Troubleshooting_parallel_builds) to safe RAM.
