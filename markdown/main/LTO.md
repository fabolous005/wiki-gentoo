<!-- source: https://wiki.gentoo.org/wiki/LTO | group: Gentoo Wiki (Main) | wiki-title: LTO -->
---
title: LTO
url: https://wiki.gentoo.org/wiki/LTO
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-23"
fingerprint: af241a5f08638708
license: CC BY-SA 4.0
---

# LTO

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Link Time Optimization (**LTO**) is the term for a class of optimizations done during linking. The linker is aware of all the [translation units](<https://en.wikipedia.org/wiki/Translation_unit_(programming)>) (TUs) and can optimize more compared to what conventional optimization passes by a compiler individually can do. The most important optimizations done are inlining and code locality improvements. Inlining is the driver of optimizations in general and facilitates other compiler passes. This can improve system performance at the cost of longer compile time.

This article will show the user how to enable LTO without the need for third party overlays and using recommended tested `[CFLAGS](https://wiki.gentoo.org/wiki/CFLAGS)` for the best performance and system stability (with either [GCC](https://wiki.gentoo.org/wiki/GCC) or [LLVM](https://wiki.gentoo.org/wiki/LLVM)-based toolchains).

## Before Beginning

LTO has a risk-to-reward ratio to work out before a user decides to switch. If the system has a 4 core CPU with 4GB of RAM as an example, then the user will very likely spend much more time waiting for a package to compile than they could ever hope to recover in optimization runtime speed. ([obligatory XKCD](https://xkcd.com/1691/))

## Benefits of LTO

- LTO can give double-digit performance boosts for many programs.
- Binaries can become much smaller.
- Startup time may be improved, especially for large C++ applications.
- Better diagnostics (warnings) for problematic code (GCC).
- Can lower RAM usage per program making it very useful for limited memory systems.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>

## Downsides of LTO

- Can increase compile time by a factor of 2 to 3.
- Uses more RAM during compiling.
- Not all programs become faster or smaller.
- There is a chance of finding build-time or runtime bugs while using it.
- Must be prepared to try without it if something is acting odd.
- Reporting problems like compiler crashes become much harder than without LTO, because the compiler no longer works on one object file at-a-time.

## Terminology

There are some terms which are overloaded because GCC, LLVM, and Rust have [distinct meanings](https://forums.gentoo.org/viewtopic-p-8863445.html#8863445) for them. The terminology is highly overloaded and confusing.

GCC has the following modes:

- fat LTO (i.e. `-flto -ffat-lto-objects`, not default) where objects are both GIMPLE (IR) and object code, so at link-time, you can choose if you want to use LTO or not
- non-fat LTO (i.e. "thin LTO", -flto, default) where objects are only GIMPLE (IR)

GCC always partitions (unless you pass `-flto=partition=none`) in some form but its scheme is controllable by `-flto-partition=scheme`.


LLVM has the following modes:

- fat LTO (i.e. `-flto -ffat-lto-objects`) where objects are both IR and object code, so at link-time, you can choose if you want to use LTO or not
- non-fat LTO (i.e. "non-fat, non-thin", "full", `-flto`) where objects are "thin" (they don't contain object code, only IR) (no partitioning)
- thin LTO (i.e "LTO with partitioning", `-flto=thin`) where objects are "thin" (they don't contain object code, only IR), but with partitioning of source files that makes it quicker and parallelisable
- fat thin LTO (i.e. `-flto=thin -ffat-lto-objects`) where objects are both IR and object code, but partitioned differently


In both GCC and LLVM, "fat" means "both IR and object code", but "thin" means something different:

- In LLVM, it's better to refer to "-flto" (not "`-flto=thin`") as "non-thin LTO" instead, not "fat".
- It's unfortunate that LLVM chose to use the term "thinLTO" because the term "thin" was already being used in GCC to discuss changing away from `-ffat-lto-objects` being the default (many years ago).
- In Rust, "fat" means something completely different.


Rust has the [following](https://doc.rust-lang.org/cargo/reference/profiles.html#lto) modes:

- no LTO (`-C lto=off`) where no LTO is performed at all (yes, this is different from *false*)
- "thin local LTO" (`-C lto=false`) where LTO is only within a crate
- "fat" LTO (`-C lto=fat`, `-C lto=true`) where it's across all crates
- "thin" LTO (`-C lto=thin`) where it's across all crates but with better partitioning

See also the Rust [performance book](https://nnethercote.github.io/perf-book/build-configuration.html#link-time-optimization).

As a final remark, GCC 15 has incremental LTO (`-flto-incremental=/path/to/cache`). In Clang, the equivalent is "thinLTO cache".

## Enabling LTO per package

For some systems it can be more helpful to only enable LTO on packages that have a known large benefit to using, such as [Firefox](https://wiki.gentoo.org/wiki/Firefox)

**`/etc/portage/env/lto.conf`**

**lto.conf**

```
CFLAGS="${CFLAGS} -flto"
CXXFLAGS="${CXXFLAGS} -flto"
```
Then make the package ([www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) in this example) use the newly created env file with:

**`/etc/portage/package.env/firefox`**

**Firefox lto config example**

Now every time [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) is compiled, portage will enable `-flto` for the package. Further packages can be added underneath in same format.

For more information see the [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env) article.

## Enabling LTO System-wide

### GCC Systems

#### Enable LTO

To enable LTO package building on GCC systems, edit [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) then add `LTO` to the `USE` line and `-flto` to the `COMMON_FLAGS`.

**`/etc/portage/make.conf`**

**make.conf example**

```
# These warnings indicate likely runtime problems with LTO, so promote them
# to errors. If a package fails to build with these, LTO should not be used there.
WARNING_FLAGS="-Werror=odr -Werror=lto-type-mismatch -Werror=strict-aliasing"
COMMON_FLAGS="-O2 -pipe -march=native -flto ${WARNING_FLAGS}"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
LDFLAGS="${COMMON_FLAGS} ${LDFLAGS}"
USE="lto"
```
With this, LTO is now enabled on the system for all future emerges.

With GCC you can also pass the number of processes to use for linking to the `-flto` parameter using `-flto=NUMBER-OF-PROCESSES` which will speed it up. Using `-flto` on its own is normally all that is needed though as it automatically picks the best number of processes to use for the system. The default single-threaded link optimization was changed to multi-threaded in 2019.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> Regardless of what `-flto=NUMBER-OF-PROCESSES` option is used, GCC will use the GNU Make jobserver to determine the number of processes, if found to be available.[\[3\]](https://wiki.gentoo.org#cite_note-3)

### LLVM Systems

#### Enable LTO

All that is needed is to `-flto=thin` to `COMMON_FLAGS` and `lto` to `USE`.

**`/etc/portage/make.conf`**

**make.conf example**

```
# These warnings indicate likely runtime problems with LTO, so promote them
# to errors. If a package fails to build with these, LTO should not be used there.
#
# NOTE: As of 2025-03-10, clang does not actually implement diagnostics for any
#       of this. If you want to check for issues, use GCC. We list these flags
#       so that in the future, if clang starts respecting them, they are automatically
#       used by existing make.conf files.
#
# As of 2024-11-11, Clang has a pull request for -Wstrict-aliasing as a real
# warning (noop for now):
# https://github.com/llvm/llvm-project/pull/74155
#
# As of 2024-11-11, Clang lacks -Wlto-type-mismatch entirely:
# https://github.com/llvm/llvm-project/issues/56487
#
# As of 2025-03-10, clang has -Wodr but doesn't use it for much:
# https://github.com/llvm/llvm-project/issues/34914
WARNING_FLAGS="-Werror=odr -Werror=strict-aliasing"
COMMON_FLAGS="-O2 -pipe -march=native -flto=thin ${WARNING_FLAGS}"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
LDFLAGS="${COMMON_FLAGS} ${LDFLAGS}"
USE="lto"
```
Clang added the multithreaded `-flto=thin` flag in 2016.[\[4\]](https://wiki.gentoo.org#cite_note-4)

### Rust

Enabling LTO for Rust project ebuilds (assuming Cargo as build tool) is conceptually quite easy. As mentioned in [the "Environment variables" section](https://wiki.gentoo.org/wiki/Rust#Environment_variables) of the "Rust" page, the simplest way is to specify an environment variable that enables LTO for the desired profile (here *release*):

**`/etc/portage/env/rust-lto.conf`**

```
CARGO_PROFILE_RELEASE_LTO=yes
```
This can be done per-package or in the ebuild as an exported environment variable before invoking Cargo, and should work for any generic Cargo project. Ebuilds not using Cargo are on their own and *might* succeed by using `RUSTFLAGS` - see below.

Unfortunately this is not where the story ends. Many Rust projects depend on crates which involve bits of C or C++ and this complicates LTO across language barriers. Enabling LTO via `CARGO_PROFILE_RELEASE_LTO=yes` will almost always work *for the Rust parts* (which can be a huge win already) but not inline functions to/from other native code, which requires careful matching of Rust's internal LLVM version with the external used compiler and linker. Due to the many possible permutations of LLVM, GCC and linkers on a user's system, this is difficult to support in ebuilds; for a long story see [bug #963128](https://bugs.gentoo.org/show_bug.cgi?id=963128).

### Rust on LLVM systems

To compile Rust programs with LTO then, enable it in both `COMMON_FLAGS` and `[RUSTFLAGS](https://wiki.gentoo.org/wiki/Rust#Environment_variables)`.

**`/etc/portage/make.conf`**

**make.conf example**

```
RUSTFLAGS="${RUSTFLAGS} -C linker-plugin-lto"
```
### Rust on GCC systems

As of 2024-11-09, [Rust docs](https://doc.rust-lang.org/rustc/linker-plugin-lto.html) say:

The -C linker-plugin-lto flag allows for deferring the LTO optimization to the actual linking step, which in turn allows for performing interprocedural optimizations across programming language boundaries if all the object files being linked were created by LLVM based toolchains. The prime example here would be linking Rust code together with Clang-compiled C/C++ code.

\[...\]

C/C++ code as a dependency in Rust

RUSTFLAGS="-Clinker-plugin-lto -Clinker=clang -Clink-arg=-fuse-ld=lld"


Therefore, to have LTO for Rust code on GCC systems, one must build packages selectively with Clang (for now). Create /etc/portage/env/llvm-lto.conf with these contents:

**`/etc/portage/env/llvm-lto.conf`**

**llvm-lto.conf example**

```
# These warnings indicate likely runtime problems with LTO, so promote them
# to errors. If a package fails to build with these, LTO should not be used there.
#
# As of 2024-11-11, Clang has a pull request for -Wstrict-aliasing as a real
# warning (noop for now):
# https://github.com/llvm/llvm-project/pull/74155
#
# As of 2024-11-11, Clang lacks -Wlto-type-mismatch:
# https://github.com/llvm/llvm-project/issues/56487
WARNING_FLAGS="-Werror=odr -Werror=strict-aliasing"
COMMON_FLAGS="-march=native -O2 -flto=thin -pipe ${WARNING_FLAGS}"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
RUSTFLAGS="-C target-cpu=native -C strip=debuginfo -C opt-level=3 \
-C linker=clang -C linker-plugin-lto -C link-arg=-fuse-ld=lld"
LDFLAGS="${COMMON_FLAGS} ${LDFLAGS} -fuse-ld=lld"
CC="clang"
CXX="clang++"
CPP="clang-cpp"
AR="llvm-ar"
NM="llvm-nm"
RANLIB="llvm-ranlib"
USE="lto"
```
Then, in /etc/portage/package.env, specify llvm-lto.conf after each package where Rust LTO is desired.

### USE Flags

On both GCC and LLVM systems, some packages require special fixes to work with LTO and this is enabled by setting globally the [lto](https://packages.gentoo.org/useflags/lto) [USE flag.](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/make.conf`**

**USE flag example**

```
USE="lto"
```
### Exceptions (USE="lto")

There are some complicated packages, such as Mozilla Firefox and its derivatives (Thunderbird, SpiderMonkey) that require [lto](https://packages.gentoo.org/useflags/lto) [to manage a working dependency chain so the project can be built. But the goal is to get rid of the](https://wiki.gentoo.org/wiki/USE_flag) [lto](https://packages.gentoo.org/useflags/lto) [use flag where possible, as much as possible.](https://wiki.gentoo.org/wiki/USE_flag)

### With a custom LTO profile

It is possible to use a custom [profile](https://wiki.gentoo.org/wiki/Portage/Profiles) to setup system-wide LTO. See [LTO profile](https://wiki.gentoo.org/wiki/LTO/LTO_profile).

## Troubleshooting, Fixing and Reporting LTO Issues

While LTO works without issue for most users thanks to the hard work of everyone in the community in Gentoo and beyond, there are still some packages that a user may run across that have been missed.

This section will show how to fix the issue for the their system and also report the issue (if needed) so it can be fixed for everyone to help make Gentoo better for everyone in the future.

### Stable Systems

As of 2023-05-15, LTO fixes have mostly trickled down from testing to stable (e.g. \~amd64 to amd64). However, there may be exceptions, so before reporting, try the latest version of the package in the testing branch to see if it's already fixed and save yourself and the developers time and effort.

`root #``emerge --ask app-editors/emacs`
In this example, on a stable amd64 system, Portage would compile version **28.2-r6**. On the first sign of a build issues, check [packages.gentoo.org](https://packages.gentoo.org) if there is a version in testing. In this case, one would find that there is a version **28.3\_rc1-r2** available.

In this case, the next step would be to temporarily accept the keyword for just that version so Portage only uses the testing version until the stable package version catches up to the fix.

To only accept the keyword for one version, then an `=` is required before the package type and then the version number after:

**`/etc/portage/package.accept_keywords/emacs`**

**Keywording Example**

Then try to emerge the updated version:

`root #``emerge --ask app-editors/emacs`
If the package compiles fine now, then no reporting is necessary. If, however, it still fails, then continue to the next step.

### Latest Version Issues

#### Existing Reports Procedure

First check if a bug already exists for the problem at [bugs.gentoo.org](https://bugs.gentoo.org). Using the search tool, looking up "emacs lto" would show an already reported bug as [bug #854360](https://bugs.gentoo.org/show_bug.cgi?id=854360).

Next, compare the reported version to the version tested on the system. If the versions are the same, then no action is needed, however, if the tested version is newer, then adding a comment that the problem still exists in the testing version should be added to help save time for the person working on the bug.

#### New Bugs Procedure

In the case of no bug being open after searching, opening a new report is one of the most helpful things a Gentoo user can do to help highlight issues.

Do remember there is no shame if a mistake is made so there is no need for a user to feel nervous as you will be guided on what extra information is needed if required.

An imperfect bug report is always preferred over a perfect report that never materializes.

##### Information Required

A good title helps, for example "app-editors/emacs-28.3\_rc1-r2: fails to compile with LTO"

A short description of the issue and what has been tried so far.

Steps to reproduce, preferably from a fresh stage3 install:

1. Install stage3 \<tarball name used>
2. Enable lto in /etc/portage/make.conf via flags, per above instructions
3. Add `app-editors/emacs-28.3_rc1-r2` to package.accept\_keywords
4. emerge -va app-editors/emacs

Attach the build.log to the bug report from the location portage gives.

Add the output of emerge --info to a section comment.

This may sound confusing the first time, however it's very easy after the first time, and can be fun to watch a reported bug get fixed in real time.

### Disable LTO per Package

After reporting, it's still possible to workaround the issue until a fix is committed by disabling LTO using [package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env).

#### Create nolto.conf

package.env configuration is a Portage feature that allows a user to set `CFLAGS` and other options on a per-package basis.

Create /etc/portage/env/nolto.conf with the following:

**`/etc/portage/env/nolto.conf`**

**nolto.conf example**

```
# Env setup to disable LTO and related warnings for problematic builds
DISABLE_LTO="-Wno-error=odr -Wno-error=lto-type-mismatch -Wno-error=strict-aliasing -fno-lto"
CFLAGS="${CFLAGS} ${DISABLE_LTO}"
CXXFLAGS="${CXXFLAGS} ${DISABLE_LTO}"
FCFLAGS="${FCFLAGS} ${DISABLE_LTO}"
FFLAGS="${FFLAGS} ${DISABLE_LTO}"
LDFLAGS="${LDFLAGS} ${DISABLE_LTO}"
```
This is the configuration that Portage will use for the packages defined in the next step.

#### Create package.env/noltobuild

Suppose further that emacs is a problematic package, then /etc/portage/package.env/noltobuild would look like:

**`/etc/portage/package.env/noltobuild`**

**noltobuild config example**

Now compiling `app-editors/emacs` on the user's system would no longer build with LTO and the compilation issue found at the start would be fixed as a workaround until a permanent fix is found in Gentoo.

## See Also

- [LLVM/Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) — a C/C++/Objective-C/C++, CUDA, and RenderScript language front-end for the LLVM project
- [LTO optimization of GCC](https://wiki.gentoo.org/wiki/GCC_optimization#Link_Time_Optimization_.28LTO.29)
- [Project:Toolchain/LTO](https://wiki.gentoo.org/wiki/Project:Toolchain/LTO) — details the Toolchain project's position on Link Time Optimization.

1. [↑](https://wiki.gentoo.org#cite_ref-1) Teresa Johnson, Mehdi Amini, Xinliang David Li. [ThinLTO - Building C++ Applications with Scalable Whole Program Optimization](https://github.com/CppCon/CppCon2017/blob/master/Presentations/ThinLTO%20-%20Building%20C%2B%2B%20Applications%20with%20Scalable%20Whole%20Program%20Optimization/ThinLTO%20-%20Building%20C%2B%2B%20Applications%20with%20Scalable%20Whole%20Program%20Optimization%20-%20Teresa%20Johnson%20-%20CppCon%202017.pdf), September 27, 2017. Retrieved on May 5, 2024
2. [↑](https://wiki.gentoo.org#cite_ref-2) Liska, Martin. [Deduce automatically number of cores for -flto option.](https://gcc.gnu.org/git/?p=gcc.git;a=commit;h=f12fbeb535f192f742025cc4f9b69a48136730f1), July 30, 2019. Retrieved on November 10, 2024.
3. [↑](https://wiki.gentoo.org#cite_ref-3) [https://github.com/gcc-mirror/gcc/blob/releases/gcc-15.1.0/gcc/lto-wrapper.cc#L1701](https://github.com/gcc-mirror/gcc/blob/releases/gcc-15.1.0/gcc/lto-wrapper.cc#L1701)
4. [↑](https://wiki.gentoo.org#cite_ref-4) Wennborg, Hans. [LLVM 3.9 Release](https://lists.llvm.org/pipermail/llvm-announce/2016-September/000070.html), September 2, 2016. Retrieved on November 10, 2024.
