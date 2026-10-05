<!-- source: https://wiki.gentoo.org/wiki/Musl_porting_notes/1.2.4 | group: Gentoo Wiki (Main) | wiki-title: Musl porting notes/1.2.4 -->
---
title: Musl porting notes/1.2.4
url: https://wiki.gentoo.org/wiki/Musl_porting_notes/1.2.4
hostname: gentoo.org
sitename: Musl porting notes/1.2.4
date: "2026-01-06"
fingerprint: "1ef4f95b0c4899d8"
license: CC BY-SA 4.0
---

# Musl porting notes/1.2.4

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Set of notes for **musl 1.2.4 porting**

See [bug #903611](https://bugs.gentoo.org/show_bug.cgi?id=903611) and [bug #920477](https://bugs.gentoo.org/show_bug.cgi?id=920477) for Musl 1.2.4 trackers.

## What changed?

1. LFS64 hacks removed

See also: [musl release notes, see 1.2.4](https://musl.libc.org/releases.html)

## Fixes

### error: LFS64 interfaces (\*64 undeclared here, ex. pread64)

The Gentoo tracker bug for these issues is [bug #903611](https://bugs.gentoo.org/show_bug.cgi?id=903611).

The legacy "LFS64" ("large file support") interfaces, which were provided by macros remapping them to their standard names (`#define stat64 stat` and similar) have been deprecated and are no longer provided under the `_GNU_SOURCE` feature profile, only under explicit `_LARGEFILE64_SOURCE`. The latter will also be removed in a future version.

The correct fix is to adjust the code to use standard `off_t` types and then to cater for glibc by passing `-D_FILE_OFFSET_BITS=64` to avoid regressing 32-bit glibc systems. In autoconf, this can be done with the `AC_SYS_LARGEFILE` macro (if using this, config.h **must** be consistently included before all standard headers everywhere, or corruption may occur). Meson does this by default. CMake has no built-in macro for this and it must be done manually.

As a temporary workaround, `-D_LARGEFILE64_SOURCE` can be appended to `CPPFLAGS` by doing

**`borked.ebuild`**

```
inherit flag-o-matic
...
src_compile() {
    # Temporary workaround for musl-1.2.4 (upstream bug #123456, gentoo bug #123456)
    # XXX: This will stop working with future musl releases!
    append-cppflags "-D_LARGEFILE64_SOURCE"
    ...
}
```
## FAQ

### Why is the 64-bit supplementary interface not needed?

musl by default uses 64-bit types for the "LFS lot" (e.g. `off_t`, `ino_t`). It also does the same for `time_t` which is related but a separate topic.

In contrast, glibc on 32-bit systems needs `-D_FILE_OFFSET_BITS=64` to be defined to widen its types. This however affects ABI and interfaces in public headers must be checked to see if they contain any of the changed types.

The dual-ABI approach was proposed years ago as a compromise to allow libraries to support both 32-bit `off_t` (and friends) and 64-bit LFS-capable `off_t`. The idea was that libraries would provide two versions of relevant functions, structures, etc rather than unilaterally changing ABI on a flag day. glibc did its part with providing e.g. `off64_t` guarded by `-D_LARGEFILE_SOURCE` (there's also `-D_LARGEFILE64_SOURCE`). Adoption of this was extremely poor and nowadays, use of these types is usually based on a misunderstanding of how to obtain wider types on glibc systems.

### How do I reproduce these bugs?

If musl 1.2.4 is not already installed on the host, using a 32-bit chroot with a musl stage3 is the best way to reproduce.
