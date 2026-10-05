<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:All_available_memory_is_being_used | group: Gentoo Knowledge | wiki-title: Knowledge_Base:All_available_memory_is_being_used -->
---
title: Knowledge Base:All available memory is being used
url: https://wiki.gentoo.org/wiki/Knowledge_Base:All_available_memory_is_being_used
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-03-13"
fingerprint: bc799d0974b2a4e1
license: CC BY-SA 4.0
---

# Knowledge Base:All available memory is being used

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When calling the free command to see the available memory, users notice that all memory is in use and therefore not available for applications anymore:

`user $``free -m`
total       used       free     shared    buff/cache   available
Mem:          7974        5721        61          0          2192        2154
Swap:         8205         20       8184

A more human readable output is available with the `-h` option:

`user $``free -mh`
total       used       free     shared    buff/cache   available
Mem:         7.8Gi      5.6Gi      6.1Mi        0Ki         2.1Gi       2.1Gi
Swap:        8.0Gi       20Mi      8.0Gi

## Environment

This article is applicable to all Linux systems.

## Analysis

The output of the free command is correct, but often misinterpreted.

- The *free* column in the *Mem:* row tells the user how much memory is not used for anything, including buffers or cache (which can be removed at any time when memory is needed).
- The *buff/cache* column in the *Mem:* row show how much memory is currently used by buffers and cache (mostly used for file system caching), which is memory that can be immediately reclaimed for applications (and as such should be seen as *free* for applications)

## Resolution

To know how much memory is available for applications (if they need it), one should read the *available* column on the *Mem:* line, as that includes free memory + memory used by buffers and cache.

In the above example, more than 2 GiB of memory is available for applications.

## External resources

- [Linux ate my ram!](https://www.linuxatemyram.com) — An entire site dedicated to the misinterpretation of available RAM in Linux.
