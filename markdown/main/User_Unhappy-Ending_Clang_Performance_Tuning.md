<!-- source: https://wiki.gentoo.org/wiki/User/Unhappy-Ending/Clang/Performance_Tuning | group: Gentoo Wiki (Main) | wiki-title: User/Unhappy-Ending/Clang/Performance Tuning -->
---
title: User/Unhappy-Ending/Clang/Performance Tuning
url: https://wiki.gentoo.org/wiki/User/Unhappy-Ending/Clang/Performance_Tuning
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-03"
fingerprint: a621139e37267c30
license: CC BY-SA 4.0
---

# User/Unhappy-Ending/Clang/Performance Tuning

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Performance Tuning

Risky risky!! Be ware before treading here!!

### Devirtualization

Something about context switching and stuff like that I think and no real benchmarks to show

### Link-time Optimizations

LTO can get some gains "find some phoronix benchmark with some lto gains" but not always, fair warning blah blah blah, some extra time consuming because link phase takes longer

#### Traditional LTO backend

-flto or -flto=full

Called FullLTO, it offers more visibility during link time and provides better optimizations currently. Some major customers such as Sony use full LTO instead of ThinLTO.

#### ThinLTO backend

-flto=thin

#### Using LTO with the binutils BFD linker

When using LTO with Clang, don't bother with linker BFD. Although modern BFD supports linker plugins which is a hard requirement for Clang to use LTO with BFD, it's not recommended. When using LTO, use linker LLD. LLD has native support for Clang and LTO built in.

If choosing to use LTO with BFD, ensure LLVM has gold use flag. It's not really anything to do with gold, just supplies a linker plugin called gold that originally worked with gold since gold had plugins but not bfd.

Compile time does take a little extra but nothing at all like PGO. Threaded LTO is much faster than full, but has less area for optimization. Full gives whole program visibiilty than thin LTO can't.

#### Using LTO with the LLVM LLD linker

When using LLD linker for more aggressive LTO passes but takes longer: -Wl,--lto-O3

#### LTO only flags

-fvirtual-function-elimination risky risky risky! Requires -fwhole-program-vtables -fwhole-program-vtables

### Profile Guided Optimizations

Unlike GCC, Clang requires an external package to be able to utilize PGO. The package [sys-devel/clang-runtime](https://packages.gentoo.org/packages/sys-devel/clang-runtime) will pull in [sys-libs/compiler-rt-sanitizers](https://packages.gentoo.org/packages/sys-libs/compiler-rt-sanitizers) by default via the `sanitize` `USE` flags. A default Gentoo user will have no issue. Users who customize their `USE` flags and don't want the extra Clang sanitizers will need to ensure `profile` and `orc` are set locally in /etc/portage/package.use.

`root #``nano /etc/portage/package.use/compiler-rt-sanitizers.use`
**`/etc/portage/package.use/compiler-rt-sanitizers.use`**

*# required USE flags for pgo
sys-libs/compiler-rt-sanitizers profile orc*

Install the Clang sanitizers:

`root #``emerge --ask --changed-use sys-libs/compiler-rt-sanitizers`
**`/etc/portage/package.use/pgo.use`**

*# required USE flags for pgo
app-shells/bash pgo
dev-lang/python pgo
sys-devel/binutils pgo
sys-devel/gcc -pgo
www-client/firefox -pgo*

There can be real world gains from using PGO, such as with Python. Since Python is so intertwined in a Gentoo OS, it's worth it for Gentoo users to look into. Not everything gains from PGO, and unless there is real world data to provide proof sometimes there is no performance gain, so the extra compile time is a huge trade.
