<!-- source: https://wiki.gentoo.org/wiki/Cramfs | group: Gentoo Wiki (Main) | wiki-title: Cramfs -->
---
title: Cramfs
url: https://wiki.gentoo.org/wiki/Cramfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-06-30"
fingerprint: "5e8f79aed4da3be8"
license: CC BY-SA 4.0
---

# Cramfs

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Cramfs is a memory and space sensitive filesystem that supports random reading. Ideal for use as ROM, it avoids the block device layer entirely and useful in embedded systems with very tight memory constraints. Cramfs is extremely limited in terms of features and performance.

The precursor to [SquashFS](https://wiki.gentoo.org/wiki/SquashFS), Cramfs was obsolete since late 2013<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> but has recently found new life from a kernel maintainer in Linux kernel 4.15.0.[\[2\]](https://wiki.gentoo.org#cite_note-2)

## Installation

### Kernel

Specifically the `CONFIG_CRAMFS` and `CONFIG_CRAMFS_BLOCKDEV` options.

KERNEL **Enable Cramfs support**

```
File systems  --->
  [*] Miscellaneous filesystems  --->
    <*>   Compressed ROM file system support (cramfs)
    [*]     Support CramFs image over a regular block device (NEW)
```
### Emerge

## See also

- [SquashFS](https://wiki.gentoo.org/wiki/SquashFS) — an open source, read only, extremely compressible filesystem.
