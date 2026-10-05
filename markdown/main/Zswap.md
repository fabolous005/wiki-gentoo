<!-- source: https://wiki.gentoo.org/wiki/Zswap | group: Gentoo Wiki (Main) | wiki-title: Zswap -->
---
title: Zswap
url: https://wiki.gentoo.org/wiki/Zswap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-26"
fingerprint: ae0f1f493ee239e8
license: CC BY-SA 4.0
---

# Zswap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Zswap** is a lightweight compressed cache for swap pages.

Zswap is a kernel feature that provides a compressed RAM cache for swap memory pages. Pages which would otherwise typically be swapped out to hard disk are instead compressed and stored into a memory pool in RAM. Once this memory pool is full or the available RAM is exhausted, the least recently used ([LRU](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_.28LRU.29)) page is decompressed and written to swap on hard disk, as if it had not been intercepted by zswap. After the page has been moved to swap, the compressed version in the memory pool can be freed and used again.

## Zswap and Gentoo

Zswap is particularly interesting for Gentoo, because it can help make the most of limited RAM resources when emerging (compiling) large packages. It uses a part of the available RAM as compressed swap space.

Imagine a system with 8 GB of RAM with zswap using up to 25% of it, and zswap reaching a compression ratio of a factor two. The system will then have 25% of 8GB times two, what is 4GB of swap space in memory which only costs 2 GB of RAM. Not only is this a very effective use of RAM memory, but it can be faster than swap files on older mechanical hard drives.

For systems with swap files on SSD, zswap might help to relieve wear on the SSDs.

## Differences between zswap and zram based swap

A short overview:

- Zswap works in conjunction with regular [swap](https://wiki.gentoo.org/wiki/Swap) while a [zram](https://wiki.gentoo.org/wiki/Zram) based swap device does not require a backing swap device and may work standalone (if no swap on hard disk is required, i.e. on SSD or kind of flash memory).
- Zswap is a compressed swap cache in RAM and works as a type of proxy for regular swap (in this context also called backing swap device). Zswap gets filled up first and evicts pages from compressed cache on an [LRU](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_.28LRU.29) basis to the backing swap device when the compressed pool reaches its size limit. This not only speeds up swap usage but also reduces hits on backing swap device (i.e. SSD).
- A zram based swap on the other hand works like regular swap (but compressed in RAM) without the opportunity to evict pages. So it gets filled up gradually until it’s full. After that, the next (but probably slower) swap in order (i.e. on hard disk) fills up. This way, it is possible to have stored less frequently used memory pages within the faster zram based swap, while newer frequently used memory pages get swapped to slower hard disk.

## Kernel configuration

The kernel needs to have swap, frontswap, options for zswap and compression algorithms enabled:

**Enable zswap**

## Interactive configuration

The zswap parameters can be examined as follows:

`root #````
cd /sys/module/zswap/parameters
```
`root #``grep "" *`
compressor:lzo
enabled:N
max\_pool\_percent:20
same\_filled\_pages\_enabled:Y
zpool:zbud

Enabling zswap can be done by writing "1" to the enabled file:

`root #``echo 1 > /sys/module/zswap/parameters/enabled` or:

`root #``echo Y > /sys/module/zswap/parameters/enabled` LZ4 is a popular choice for the compression algorithm:

`root #``echo lz4 > /sys/module/zswap/parameters/compressor`
## Making the configuration permanent

### Using the kernel commandline

Zswap can be configured permanently using the kernel commandline, e.g when using [GRUB](https://wiki.gentoo.org/wiki/GRUB):

**`/etc/default/grub`**

```
GRUB_CMDLINE_LINUX="zswap.enabled=1 zswap.compressor=lz4"
```
Do not forget to regenerate the GRUB configuration.

### Alternative: using local.d

Create a file in /etc/local.d:

**`/etc/local.d/50-zswap.start`**

```
# configure zswap
echo lz4 > /sys/module/zswap/parameters/compressor
echo 1 > /sys/module/zswap/parameters/enabled
```
Make the file executable:

`root #``chmod +x /etc/local.d/50-zswap.start`
## See also

- [Swap](https://wiki.gentoo.org/wiki/Swap) — refers to both the act of moving memory pages between memory and a secondary storage.
- [Zram](https://wiki.gentoo.org/wiki/Zram) — a [Linux kernel](https://wiki.gentoo.org/wiki/Kernel) feature and set of userspace tools for creating compressible RAM-based block devices.
