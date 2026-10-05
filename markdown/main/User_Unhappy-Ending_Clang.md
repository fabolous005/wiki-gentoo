<!-- source: https://wiki.gentoo.org/wiki/User/Unhappy-Ending/Clang | group: Gentoo Wiki (Main) | wiki-title: User/Unhappy-Ending/Clang -->
---
title: User/Unhappy-Ending/Clang
url: https://wiki.gentoo.org/wiki/User/Unhappy-Ending/Clang
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-11"
fingerprint: b661d10b24263fa0
license: CC BY-SA 4.0
---

# User/Unhappy-Ending/Clang

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Clang** is a C, Objective-C, C++, Objective-C++, CUDA, and RenderScript language front-end for the LLVM project with a focus on standards, code correctness, and useful diagnostic messages to ease compile time troubleshooting. Language support is detailed on the [Clang compatibility website.](https://clang.llvm.org/compatibility.html)

## Foreword

One of the goals of the Clang project is to be compatible with code written with GCC as the target compiler. Thus, it's possible to use Clang as an alternative compiler on a Gentoo system. Decide beforehand if usage should be:

- Simple. Using Clang on a per package or as needed basis.

- Intermediate. Using Clang system wide with the default GNU supplied toolchain and libraries.

- Advanced. Using Clang system wide with the LLVM toolchain and libraries. This is beneficial since Clang natively integrates with LLVM.


On most Linux systems, [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) provides the system's C++ library, unwinder, openmp, compiler runtime libraries, and sanitizers. These are part of the fundamental building blocks of a Linux system. It's possible to use Clang with LLVM's alternatives in place of the GCC provided libraries for a fully self contained LLVM toolchain.

### Important differences compared to GCC

The following differences are important to understand when using Clang as a compiler. They will cause issues when compiling packages expecting default GCC behavior.

- GCC supports nested functions. Minor difference, major problem for being able to switch fully to the LLVM/Clang toolchain. [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) is commonly the fundamental building block of most Linux systems world wide and contains nested functions. Nested functions are a GCC specific extension and are non-standard in C. This is the main reason Clang is not able to compile glibc although there's progress to make glibc alternate toolchain friendly.

- GCC defaults to `-ftrapping-math`, Clang defaults to `-fno-trapping-math`. [x11-libs/pixman](https://packages.gentoo.org/packages/x11-libs/pixman) will fail to build with Clang because of this.

- GCC doesn't need a separate package installed to execute profile guided optimizations. Clang requires [llvm-runtimes/compiler-rt-sanitizers](https://packages.gentoo.org/packages/llvm-runtimes/compiler-rt-sanitizers) with `USE="profile orc"` before you can enable [pgo](https://packages.gentoo.org/useflags/pgo)

- GCC defaults to `-fsemantic-interposition` to interpose code. Clang does interprocedural optimizations by default, similar to passing the `-fno-semantic-interposition` flag. Explicitly passing `-fno-semantic-interposition` allows further interprocedural optimizations if the code allows it.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>

### Negligible differences compared to GCC

Mostly negligible differences that shouldn't affect a system, but worth taking into considering if matching compiler behaviors is desired.

- GCC defaults to `-ffp-contract=fast`, Clang defaults to `-ffp-contract=on`. Unless wanting to match GCC's slightly riskier behavior there should be no issue with Clang's safer default.

- Prior to version 12, GCC didn't run vector optimizations at the default level of `-O2` and lower. Clang runs vector optimizations at all levels greater than `-O1` except level `-Oz`, which only runs the SLP vectorizer.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> Though unlikely to cause issue, it's currently relevant since GCC:11 is still in the Portage tree.



## Create a fallback environment

This is especially relevant when using Clang system wide. If Clang fails, the fallback environment `compiler-gcc` should be invoked instead. Configuration files with a set of environment variables to override current Portage settings can be created in the [/etc/portage/env](https://wiki.gentoo.org/wiki//etc/portage/env) directory. If the directory doesn't exist, create it:

`root #``mkdir /etc/portage/env`
The following examples will use [app-editors/nano](https://packages.gentoo.org/packages/app-editors/nano)

`root #``nano /etc/portage/env/compiler-gcc`
**`/etc/portage/env/compiler-gcc`**

**Environment named*compiler-gcc***

```
# set the compiler(s) to be used
CC=gcc
CPP=cpp
CXX=g++
# reset flags in case compiler specific flags have been defined
COMMON_FLAGS="-O2 -march=native"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
LDFLAGS="-Wl,--as-needed -Wl,-O1"
```

In order to set `compiler-gcc` as the environment to be used, create a file in /etc/portage/ directory named [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env).

`root #``nano /etc/portage/package.env`
**`/etc/portage/package.env`**

**Falling back to*GCC* for *sys-libs/glibc***

```
# compile phase fails with clang
sys-libs/glibc compiler-gcc
```
## Installation

Installation can be extremely simple or advanced. To simply install Clang and be done with it, double check the USE flags:

### Clang USE flags


| [+debug](https://packages.gentoo.org/useflags/+debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [+extra](https://packages.gentoo.org/useflags/+extra) | Build extra tools (clangd, clang-tidy and a few more) | 
| [+pie](https://packages.gentoo.org/useflags/+pie) | Build programs as Position Independent Executables (a security hardening technique) | 
| [+static-analyzer](https://packages.gentoo.org/useflags/+static-analyzer) | Install the Clang static analyzer | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [ieee-long-double](https://packages.gentoo.org/useflags/ieee-long-double) | Use accelerated 128-bit IEEE long double ABI (ppc64le only) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xml](https://packages.gentoo.org/useflags/xml) | Add support for XML files | 

Then run:

`root #``emerge --ask --update --deep --changed-use llvm-core/clang`
On a default glibc profile, this will pull in [llvm-core/llvm](https://packages.gentoo.org/packages/llvm-core/llvm) and a couple of other dependencies.

#### Intermediate Clang runtime behavior

More advanced installations can be done by choosing appropriate USE flags for the [llvm-core/clang-common](https://packages.gentoo.org/packages/llvm-core/clang-common) and [llvm-runtimes/clang-runtime](https://packages.gentoo.org/packages/llvm-runtimes/clang-runtime) packages. The USE flags set for clang-common will alter how Clang runs, such as calling LLD at link time instead of BFD. that will pull in more runtime dependencies.

## Configuration

In the foreword it was noted about simple, intermediate, or advanced usage. Choose which.

### Simple

#### Using Clang on the command line

The most basic usage of Clang on a Gentoo system is invoking it through the `CC` variable. It can be defined on the command line:

`root #``CC=clang emerge --oneshot app-foo/bar`
If a package requires C++ compiled code, also define the `CXX` variable:

`root #``CC=clang CXX=clang++ emerge --oneshot app-foo/bar`
It's a good idea to include `CCP` variable since some packages like [xorg-base/xorg-server](https://packages.gentoo.org/packages/xorg-base/xorg-server) will fail otherwise:

`root #``CC=clang CPP=clang-cpp CXX=clang++ emerge --oneshot x11-base/xorg-server`
#### Configure Clang for local use

If GCC is the default system compiler but blah blah

`root #``nano /etc/portage/env/compiler-clang`
**`/etc/portage/env/compiler-clang`**

**Environment named*compiler-clang***

```
# set the compiler(s) to be used
CC=clang
CPP=clang-cpp
CXX=clang++
# reset flags in case compiler specific flags have been defined
COMMON_FLAGS="-O2 -march=native"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
LDFLAGS="-Wl,--as-needed -Wl,-O1"
```
Blah blah

`root #``nano /etc/portage/package.env`
**`/etc/portage/package.env`**

**Use GCC for sys-libs/glibc**

```
# override system compiler with clang
app-foo/bar compiler-clang
```
blah blah blah

#### Configure Clang for global use

`root #``nano /etc/portage/make.conf`
**`/etc/portage/make.conf`**

**Environment named*compiler-clang***

```
# set the compiler(s) to be used
CC=clang
CPP=clang-cpp
CXX=clang++
# safe default flags for global use
COMMON_FLAGS="-O2 -march=native"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
LDFLAGS="-Wl,--as-needed -Wl,-O1"
```
### Intermediate

Use Clang system wide with its own runtimes and linker.

### Advanced

Use Clang and the entire LLVM toolchain system wide.

##### eselect profile

Or simply use the gentoo/clang/\* profiles.

`root #``eselect profile list`
Available profile symlink targets:
  \[1\]   default/linux/amd64/17.1 (stable)
  \[2\]   default/linux/amd64/17.1/selinux (stable)
  \[3\]   default/linux/amd64/17.1/hardened (stable)
  \[4\]   default/linux/amd64/17.1/hardened/selinux (stable)
  \[5\]   default/linux/amd64/17.1/desktop (stable)
  \[6\]   default/linux/amd64/17.1/desktop/gnome (stable)
  \[7\]   default/linux/amd64/17.1/desktop/gnome/systemd (stable)
  \[8\]   default/linux/amd64/17.1/desktop/gnome/systemd/merged-usr (stable)
  \[9\]   default/linux/amd64/17.1/desktop/plasma (stable)
  \[10\]  default/linux/amd64/17.1/desktop/plasma/systemd (stable)
  \[11\]  default/linux/amd64/17.1/desktop/plasma/systemd/merged-usr (stable)
  \[12\]  default/linux/amd64/17.1/desktop/systemd (stable)
  \[13\]  default/linux/amd64/17.1/desktop/systemd/merged-usr (stable)
  \[14\]  default/linux/amd64/17.1/developer (exp)
  \[15\]  default/linux/amd64/17.1/no-multilib (stable)
  \[16\]  default/linux/amd64/17.1/no-multilib/hardened (stable)
  \[17\]  default/linux/amd64/17.1/no-multilib/hardened/selinux (stable)
  \[18\]  default/linux/amd64/17.1/no-multilib/systemd (dev)
  \[19\]  default/linux/amd64/17.1/no-multilib/systemd/merged-usr (dev)
  \[20\]  default/linux/amd64/17.1/no-multilib/systemd/selinux (exp)
  \[21\]  default/linux/amd64/17.1/no-multilib/systemd/selinux/merged-usr (exp)
  \[22\]  default/linux/amd64/17.1/systemd (stable)
  \[23\]  default/linux/amd64/17.1/systemd/merged-usr (stable)
  \[24\]  default/linux/amd64/17.1/systemd/selinux (exp)
  \[25\]  default/linux/amd64/17.1/systemd/selinux/merged-usr (exp)
  \[26\]  default/linux/amd64/17.1/clang (exp)
  \[27\]  default/linux/amd64/17.1/systemd/clang (exp)
  \[28\]  default/linux/amd64/17.1/systemd/clang/merged-usr (exp) \*
  \[29\]  default/linux/amd64/17.0/x32 (dev)
  \[30\]  default/linux/amd64/17.0/x32/systemd (exp)
  \[31\]  default/linux/amd64/17.0/x32/systemd/merged-usr (exp)
  \[32\]  default/linux/amd64/17.0/musl (dev)
  \[33\]  default/linux/amd64/17.0/musl/clang (exp)
  \[34\]  default/linux/amd64/17.0/musl/hardened (exp)
  \[35\]  default/linux/amd64/17.0/musl/hardened/selinux (exp)

#### Clang environment

Gentoo maintains a [bug tracker](https://bugs.gentoo.org/408963) for packages that fail to build with Clang. Configuring Gentoo to use Clang system wide is simple. Change the `CC` and `CXX` variables in /etc/portage/make.conf to reference the Clang equivalents. No further configuration is necessary.

## Usage

This covers more advanced usage than described above for configuration.

### Bootstrapping the Clang toolchain

For a "pure" Clang toolchain, one can build the whole LLVM stack using itself.

This is detailed in a subpage: [Clang/Bootstrapping](https://wiki.gentoo.org/wiki/Clang/Bootstrapping).

### Link-time optimizations with Clang

The link-time optimization feature defers optimizing the resulting executables to linking phase. This can result in better optimization of packages but isn't standard behavior in Gentoo yet. Clang uses the lld linker for LTO.

#### Environment

Clang supports two types of link time optimization:

- -flto or -flto=full. This is the traditional method.
- -flto=thin. The link unit is scanned and split up into multiple parts.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> With ThinLTO, the final compilation units only contain the code that are relevant to the current scope, thus speeding up compilation, lowering footprint and allowing for more parallelism at (mostly) no cost. ThinLTO is the recommended LTO mode when using Clang.

For full LTO, replace `-flto=thin` with `-flto` in the following examples. There should be no compatibility differences between full LTO and thin LTO. Additionally, if Clang was not built with the `default-lld` USE flag, add the `-fuse-ld=lld` value to the following `LDFLAGS`.

**`/etc/portage/env/compiler-clang-lto`**

**Environment named*compiler-clang-lto***

```
CFLAGS="${CFLAGS} -flto=thin"
CXXFLAGS="${CXXFLAGS} -flto=thin"
# -O2 in LDFLAGS refers to binary size optimization during linking, it is NOT related to the -O levels of the compiler
LDFLAGS="${LDFLAGS} -Wl,-O2 -Wl,--as-needed"
CC="clang"
CXX="clang++"
```
As an alternative, LLVM provides its own `ar`, `nm`, and `ranlib` values. Feel free to use them though mileage may vary over using the standard `ar`, `nm`, and `ranlib`, since they're intended to handle LLVM bitcode which Clang produces when using the `-flto` flag.

**`/etc/portage/env/compiler-clang-lto`**

**Environment named*compiler-clang-lto***

```
CFLAGS="${CFLAGS} -flto=thin"
CXXFLAGS="${CXXFLAGS} -flto=thin"
# -O2 in LDFLAGS refers to binary size optimization during linking, it is NOT related to the -O levels of the compiler
LDFLAGS="${LDFLAGS} -Wl,-O2 -Wl,--as-needed"
CC="clang"
CXX="clang++"
AR="llvm-ar"
NM="llvm-nm"
RANLIB="llvm-ranlib"
```
Now set /etc/portage/package.env overrides using Clang with LTO enabled:

**`/etc/portage/package.env`**

**Enabling*LTO* for *app-foo/bar* and *app-bar/baz***

```
 compiler-clang-lto
app-bar/baz compiler-clang-lto
```
#### Global configuration

Again, it is possible to set the `AR`, `NM`, and `RANLIB` to the LLVM implementations. Since earlier in the article compiler environments were set up using Clang without LTO, GCC without LTO, and GCC with LTO, it is now possible to pick and choose which is best on a per package basis. Since the goal is to compile packages system wide with Clang using LTO and not every package will successfully compile using it, fall back to Clang with LTO disabled or GCC. The /etc/portage/package.env may look like the following:

**`/etc/portage/package.env`**

**Example*package.env* setup**

```
# Compiled using Clang with no link-time optimization since package bar fails using flto
app-foo/bar compiler-clang
# Compiled using GCC with no link-time optimization since package baz fails using flto
app-bar/baz compiler-gcc
# Compiled using GCC with link-time optimization since package foo compiles using flto
app-baz/foo compiler-gcc-lto
```
### distcc

In order to use Clang on a distcc client, additional symlinks must to be created in /usr/lib\*/distcc/bin:

`root #````
ln -s /usr/bin/distcc /usr/lib/distcc/bin/clang
```
`root #````
ln -s /usr/bin/distcc /usr/lib/distcc/bin/clang++
```
### Kernel

The Linux kernel can be compiled with Clang and the LLVM toolchain by defining a kernel environment variable.

`root #``LLVM=1`
To configure Clang specific kernel options such as link-time optimizations or control flow integrity, run the following command:

`root #``LLVM=1 make menuconfig`
The above example demonstrates using `menuconfig`. Other options are `nconfig` and `xconfig`. Next, compile your kernel as normal.

`root #``LLVM=1 make -j$N`
In the past, it was necessary to pass `LLVM_IAS=1` to use the Clang internal assembler for a complete LLVM toolchain built kernel. This is no longer required since `LLVM=1` now defaults to include the Clang internal assembler. Use `LLVM_IAS=0` to disable the internal assembler if desired, otherwise stick to the default behavior.

#### Genkernel

When using genkernel, edit the /etc/genkernel.conf by substituting the following "Low Level Compile Settings" and adding the additional MAKEOPTS:

**`/etc/genkernel.conf`**

**Sample Clang/llvm*genkernel.conf***

```
# =========Low Level Compile Settings=========
#
# Additional make options
MAKEOPTS="LLVM=1 ${MAKEOPTS}"
# Assembler to use for the kernel.  See also the --kernel-as command line
# option.
## FIXME: llvm-as may not be a compatible tool
## KERNEL_AS="llvm-as"
# Archiver to use for the kernel.  See also the --kernel-ar command line
# option.
KERNEL_AR="llvm-ar"
# Compiler to use for the kernel (e.g. distcc).  See also the --kernel-cc
# command line option.
KERNEL_CC="clang"
# Linker to use for the kernel.  See also the --kernel-ld command line option.
KERNEL_LD="ld.lld"
# NM utility to use for the kernel.  See also the --kernel-nm command line option.
KERNEL_NM="llvm-nm"
# GNU Make to use for kernel.  See also the --kernel-make command line option.
#KERNEL_MAKE="make"
# not exposed in default config
KERNEL_OBJCOPY="llvm-objcopy"
KERNEL_OBJDUMP="llvm-objdump"
KERNEL_READELF="llvm-readelf"
KERNEL_STRIP="llvm-strip"
KERNEL_RANLIB="llvm-ranlib"
# Assembler to use for the utilities.  See also the --utils-as command line
# option.
## FIXME: llvm-as may not be a compatible tool
# it broke building util-linux for me for certain users
##UTILS_AS="llvm-as"
# Archiver to use for the utilities.  See also the --utils-ar command line
# option.
UTILS_AR="llvm-ar"
# C Compiler to use for the utilities (e.g. distcc).  See also the --utils-cc
# command line option.
UTILS_CC="clang"
# C++ Compiler to use for the utilities (e.g. distcc).  See also the --utils-cxx
# command line option.
UTILS_CXX="clang++"
# Linker to use for the utilities.  See also the --utils-ld command line
# option.
UTILS_LD="ld.lld"
# NM utility to use for the utilities.  See also the --utils-nm command line option.
UTILS_NM="llvm-nm"
# GNU Make to use for the utilities.  See also the --utils-make command line
# option.
#UTILS_MAKE="make"
# not exposed in default config
UTILS_OBJCOPY="llvm-objcopy"
UTILS_OBJDUMP="llvm-objdump"
UTILS_READELF="llvm-readelf"
UTILS_STRIP="llvm-strip"
UTILS_RANLIB="llvm-ranlib"
# Target triple (i.e. aarch64-linux-gnu) to build for. If you do not
# cross-compile, leave blank for auto detection.
#CROSS_COMPILE=""
# Override default make target (bzImage). See also the --kernel-target
# command line option. Useful to build a uImage on arm.
#KERNEL_MAKE_DIRECTIVE_OVERRIDE="fooImage"
# Override default kernel binary path. See also the --kernel-binary
# command line option. Useful to install a uImage on arm.
#KERNEL_BINARY_OVERRIDE="arch/foo/boot/bar"
```
After that use genkernel as usual:

`root #``genkernel all`
Additionally, the same options will have to be provided for any kernel modules:

**`/etc/portage/package.env`**

**Use clang for any kernel module packages**

```
# Compiled using clang like kernel itself
app-foo/bar compiler-clang
```
**`/etc/portage/env/compiler-clang`**

**Environment named*compiler-clang***

```
# This is added to make options by linux-mod.eclass
BUILD_FIXES="LLVM=1 LLVM_IAS=1"
# CC/CCX and other tools must match genkernel config
```
Further, once clang becomes the default compiler, it might be possible to use portageq envvar and make things DRY.

#### Distribution Kernel

You can compile the [distribution kernels](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) (for clarity's sake, not including the binary kernel) with LLVM using the following configs:

**`/etc/portage/env/llvm-kernel`**

and

**`/etc/portage/package.env/gentoo-kernel`**



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

Unlike GCC, Clang requires an external package to be able to utilize PGO. The package [llvm-runtimes/clang-runtime](https://packages.gentoo.org/packages/llvm-runtimes/clang-runtime) will pull in [llvm-runtimes/compiler-rt-sanitizers](https://packages.gentoo.org/packages/llvm-runtimes/compiler-rt-sanitizers) by default via the `sanitize` `USE` flags. A default Gentoo user will have no issue. Users who customize their `USE` flags and don't want the extra Clang sanitizers will need to ensure `profile` and `orc` are set locally in /etc/portage/package.use.

`root #``nano /etc/portage/package.use/compiler-rt-sanitizers.use`
**`/etc/portage/package.use/compiler-rt-sanitizers.use`**

*# required USE flags for pgo
llvm-runtimes/compiler-rt-sanitizers profile orc*

Install the Clang sanitizers:

`root #``emerge --ask --changed-use llvm-runtimes/compiler-rt-sanitizers`
**`/etc/portage/package.use/pgo.use`**

*# required USE flags for pgo
app-shells/bash pgo
dev-lang/python pgo
sys-devel/binutils pgo
sys-devel/gcc -pgo
www-client/firefox -pgo*

There can be real world gains from using PGO, such as with Python. Since Python is so intertwined in a Gentoo OS, it's worth it for Gentoo users to look into. Not everything gains from PGO, and unless there is real world data to provide proof sometimes there is no performance gain, so the extra compile time is a huge trade.

## Troubleshooting

The main place for looking up known failures with Clang is the tracker [bug #408963](https://bugs.gentoo.org/show_bug.cgi?id=408963). If hitting an issue not reported on Gentoo's Bugzilla already, please open a new bug report and make it block the linked tracker.

### Compile errors when using Clang with -flto

If the packages being installed are failing, check the logs. Often, packages with errors like the following will need to disable LTO by invoking the compiler-clang environment.

**`/var/log/portage/sys-apps:less-483-r1:20160712-034715.log`**

The following error may be seen in every LTO failure case:

**`/var/log/portage/sys-apps:less-483-r1:20160712-034715.log`**

Simply add the failing package to /etc/portage/package.env. In this case, it's the [sys-apps/less](https://packages.gentoo.org/packages/sys-apps/less) package, so to apply the proper override.

**`/etc/portage/package.env`**

**Example*package.env* setup**

```
# Compiled using Clang with no link-time optimization since the package 'less' fails using lto
sys-apps/less compiler-clang
```
Sometimes a package will fail to compile even when disabling LTO because it requires another package which was compiled using -flto and works incorrectly. Something like the following error may be seen:

**`/var/log/portage/dev-libs:boehm-gc-7.4.2:20160713-085706.log`**

In this case libatomic\_ops is causing boehm-gc to fail compiling. Recompile the program causing the failure using the non-LTO environment and then recompile the new program. In this case, boehm-gc fails when using LTO, so add both of them to the /etc/portage/package.env file to build them without LTO:

**`/etc/portage/package.env`**

**Example*package.env* setup**

```
 compiler-clang
dev-libs/libatomic_ops compiler-clang
```
### Use of GNU extensions without proper -std=

Some packages tend to use GNU extensions in their code without specifying `-std=` appropriately. GCC allows that usage, yet Clang disables some of more specific GNU extensions by default.

If a particular package relies on such extensions being available, then append the correct `-std=` flag to it:

- `-std=gnu89` for C89/C90 with GNU extensions,
- `-std=gnu99` for C99 with GNU extensions,
- `-std=gnu++98` for C++:1998 with GNU extensions.

A common symptom of this problem are multiple definitions of inline functions like this:

**`/var/log/portage/example.log`**

**Example package error in example log**

This is because Clang uses C99 inline rules by default which do not work with gnu89 code. To work around it, it is likely necessary to pass `-std=gnu89` or set one of the environmental overrides to use GCC to compile the failing package if passing the right `-std=` flag doesn't work.

Since both current (2020) GCC and Clang default to `-std=gnu17` with C99 inline rules, chances are the problems have already been spotted by a GCC user.

### sudo: clang: command not found

Clang is not added to /usr/bin and instead lives in a separate path that is added to the PATH variable. Sudo has a whitelisted PATH variable that is baked in at compile time. So when a new version of clang is installed,it will not be added to sudo's PATH until sudo is re-emerged.

### troubleshooting compiling with gcc at clang profile

/usr/src/debug/sys-libs/glibc-2.37-r3/glibc-2.37/csu/../sysdeps/x86\_64/start.S:103: undefined reference to \`main'

Use bfd linker. Add `-fuse-ld=bfd` to `CFLAGS`, `CXXFLAGS` and `LDFLAGS`  at your `/etc/portage/env/compiler-gcc-lto` or `/etc/portage/env/compiler-gcc` configuration files .
