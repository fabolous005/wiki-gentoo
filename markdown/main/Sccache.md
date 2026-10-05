<!-- source: https://wiki.gentoo.org/wiki/Sccache | group: Gentoo Wiki (Main) | wiki-title: Sccache -->
---
title: sccache
url: https://wiki.gentoo.org/wiki/Sccache
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-13"
fingerprint: ca7a582ce3555352
license: CC BY-SA 4.0
---

# sccache

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**Article status**

- Explain how to use sccache as ccache replacement

sccache helps avoid repeated recompilation for the same C, C++, and [Rust](https://wiki.gentoo.org/wiki/Rust) object files by fetching result from a cache directory.

Compiler cache is typically useful for:

- Developers who rebuild the same/similar codebase multiple times and use [/etc/portage/patches](https://wiki.gentoo.org/wiki//etc/portage/patches) to test patches.
- Users who frequently play with USE-flag changes and end up rebuilding the same packages multiple times.
- Users who use live ebuilds extensively.
- Installing very big ebuilds, such as [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) or [mail-client/thunderbird](https://packages.gentoo.org/packages/mail-client/thunderbird), without fear of losing multiple hours of code compilation due to a failure.

## Installation

### Emerge

`root #``emerge --ask dev-util/sccache`
## Configuration

### Initial setup

Create the cache directory:

`root #````
mkdir -p /var/cache/sccache
```
`root #````
chown root:portage /var/cache/sccache
```
`root #````
chmod 2775 /var/cache/sccache
```
Allow write access to the cache directory when running under sandbox:

**`/etc/sandbox.d/20sccache`**

```
# Allow write access to sccache cache directory
SANDBOX_WRITE="/var/cache/sccache/"
```
Enable sccache support in make.conf:

**`/etc/portage/make.conf`**

```
RUSTC_WRAPPER=/usr/bin/sccache
SCCACHE_DIR=/var/cache/sccache
SCCACHE_MAX_FRAME_LENGTH=104857600
```
From now on all builds utilizing Rust compiler will try to reuse object files from the /var/cache/sccache cache.

## General notes

sccache can be enabled for the current user and re-use the same cache directory:

**`~/.bashrc`**

```
export RUSTC_WRAPPER="/usr/bin/sccache"
export SCCACHE_DIR="/var/cache/sccache"
export SCCACHE_MAX_FRAME_LENGTH="104857600"
```
## Useful variables and commands

Some variables:

- Variable `SCCACHE_DIR` points to cache root directory.

Some commands:

- Command sccache -s shows cache hit statistics:

`user $``SCCACHE_DIR=/var/cache/sccache sccache -s`
Compile requests                      0
Compile requests executed             0
Cache hits                            0
Cache misses                          0
Cache timeouts                        0
Cache read errors                     0
Forced recaches                       0
Cache write errors                    0
Compilation failures                  0
Cache errors                          0
Non-cacheable compilations            0
Non-cacheable calls                   0
Non-compilation calls                 0
Unsupported compiler calls            0
Average cache write               0.000 s
Average cache read miss           0.000 s
Average cache read hit            0.000 s
Failed distributed compilations       0
Cache location                  Local disk: "/var/cache/sccache/"
Cache size                            5 GiB
Max cache size                       10 GiB

## See also

- [Handbook:AMD64/Working/Features#Caching\_compilation\_objects](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Features#Caching_compilation_objects) — how to use [Ccache](https://wiki.gentoo.org/wiki/Ccache)
- [Ccache](https://wiki.gentoo.org/wiki/Ccache) — helps avoid repeated recompilation for the same C and C++ object files by fetching the result from a cache directory.
- [Sccache-dist](https://wiki.gentoo.org/wiki/Sccache-dist) — can be used to distribute compilation and compilation cache across network.
