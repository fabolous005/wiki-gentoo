<!-- source: https://wiki.gentoo.org/wiki/LLVM/Clang | group: Gentoo Wiki (Main) | wiki-title: LLVM/Clang -->
---
title: LLVM/Clang
url: https://wiki.gentoo.org/wiki/LLVM/Clang
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-28"
fingerprint: be61f30b28363680
license: CC BY-SA 4.0
---

# LLVM/Clang

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Clang** is a C/C++/Objective-C/C++, CUDA, and RenderScript language front-end for the LLVM project with a focus on standards, code correctness, and useful diagnostic messages to ease compile time troubleshooting.

Clang can be used as an alternative to [GCC](https://wiki.gentoo.org/wiki/GCC). Simple usage is calling Clang instead of GCC, despite the latter providing more than a simple compiler front-end. The GCC toolchain also provides the system's C++ library, unwinder, OpenMP, and runtime libraries and sanitizers. It's possible to use Clang with LLVM's alternatives in place of the GCC provided libraries. These are part of the fundamental building blocks of a Linux system and toolchain.

Furthermore, it's possible to use a fully self contained LLVM toolchain by calling the LLVM alternatives to GNU [binutils](https://wiki.gentoo.org/wiki/Binutils), thus bypassing nearly all the GNU toolchain with the exception of the C library which is usually provided by GNU [glibc](https://wiki.gentoo.org/wiki/Glibc).

C, Objective-C, C++, and Objective-C++ compatibility can be found here, as detailed on the [Clang compatibility website.](https://clang.llvm.org/compatibility.html)

## Prerequisites

One of the goals of the Clang project is to be compatible with code written with GCC as the target compiler. Using Clang system wide is experimental and comes with associated risks, such as packages requiring GCC specific functions, failures to build correctly, or successful compilations but having issues when executed. These can be reported in the system wide Clang [bugtracker](https://bugs.gentoo.org/show_bug.cgi?id=408963). In these events, it's necessary to use GCC as a fallback.

### Important differences when compared to GCC

- Clang doesn't support some GCC extensions like nested functions. This is the main reason Clang is not able to compile [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) although currently a lot of work is happening to make glibc alternate toolchain friendly.

- Second, GCC defaults to `-ftrapping-math`, Clang defaults to `-fno-trapping-math`.

- GCC doesn't need a separate library installed for PGO and is self sufficient. Clang requires [llvm-runtimes/compiler-rt-santizers\[profile orc\]](https://packages.gentoo.org/packages/llvm-runtimes/compiler-rt-sanitizers) before enabling the [pgo](https://packages.gentoo.org/useflags/pgo)[USE flag](https://wiki.gentoo.org/wiki/USE_flag) for packages.

- GCC interposes code via `-fsemantic-interposition` by default. Clang does interprocedural optimizations by default, but `-fno-semantic-interposition` allows further interprocedural optimizations if the code allows it.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>

### Minor differences to GCC

- GCC defaults to `-ffp-contract=fast` while Clang defaults to `-ffp-contract=on`. Unless wanting to match GCC's slightly riskier behavior there should be no issue with Clang's safer default.

- Until version 12, GCC didn't run vector optimizations at `-O2` or lower. Clang runs vector optimizations at all levels greater than `-O1` except level `-Oz`, which only runs the SLP vectorizer.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> Though unlikely to cause issue, it's currently relevant since [sys-devel/gcc:11](https://packages.gentoo.org/packages/sys-devel/gcc) is still in ::gentoo.

- Both compilers LTO phases function *drastically* different and it's outside the scope of this article to detail it. What works with GCC may or may not work for Clang, and vice versa.

## Installation

### USE flags


| [+debug](https://packages.gentoo.org/useflags/+debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [+extra](https://packages.gentoo.org/useflags/+extra) | Build extra tools (clangd, clang-tidy and a few more) | 
| [+pie](https://packages.gentoo.org/useflags/+pie) | Build programs as Position Independent Executables (a security hardening technique) | 
| [+static-analyzer](https://packages.gentoo.org/useflags/+static-analyzer) | Install the Clang static analyzer | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [ieee-long-double](https://packages.gentoo.org/useflags/ieee-long-double) | Use accelerated 128-bit IEEE long double ABI (ppc64le only) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xml](https://packages.gentoo.org/useflags/xml) | Add support for XML files | 

### LLVM\_TARGETS

For information on setting `LLVM_TARGETS` for clang, refer to [LLVM#LLVM\_TARGETS](https://wiki.gentoo.org/wiki/LLVM#LLVM_TARGETS).

### Emerge

`root #``emerge --ask llvm-core/clang`
## Configuration

There are many possible configurations. This section assumes the user wants to choose to use Clang to compile some packages. It is unrelated to packages using LLVM or Clang for their libraries.

Users may wish to default to Clang and selectively use GCC or vice-versa.

There are two ways to do this:

1. System wide using /etc/portage/make.conf or,
2. via environment variables like the one(s) created for the GCC fallback.

### make.conf

Users will still need GCC to build some packages like glibc or wine. Gentoo maintains a tracker bug ([bug #408963](https://bugs.gentoo.org/show_bug.cgi?id=408963)) for packages that fail to build with Clang.

Configuring Gentoo to use Clang system wide is simple. Change the `CC` and `CXX` variables in /etc/portage/make.conf to reference the Clang equivalents. No further configuration is necessary.

**`/etc/portage/make.conf`**

```
# Normal settings here
COMMON_FLAGS="-O2 -march=native"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
 
CC="clang"
CPP="clang-cpp" # necessary for xorg-server and possibly other packages
CXX="clang++"
AR="llvm-ar"
NM="llvm-nm"
RANLIB="llvm-ranlib"
```
Alternatively, the same contents could be put in e.g. /etc/portage/env/compiler-clang. This would allow using Clang on a per package basis by invoking the compiler-clang environment file if desired:

**`/etc/portage/package.env`**

**Using the*Clang* compiler for *app-foo/bar* and *app-bar/baz***

```
 compiler-clang
app-bar/baz compiler-clang
```
### GCC fallback environment

Create a configuration file with a set of environment variables using Portage's built in [/etc/portage/env](https://wiki.gentoo.org/wiki//etc/portage/env) directory. This will override any defaults for any packages that fail to compile with clang.

The name used below is just an example, so feel free to choose whatever name is desired for the fallback environment. Be sure to substitute the chosen name with the examples used in this article.

The most basic example is:

**`/etc/portage/env/compiler-gcc`**

**Environment named*compiler-gcc***

```
COMMON_FLAGS="-O2 -march=native"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
LDFLAGS="-Wl,--as-needed"
 
CC="gcc"
CXX="g++"
CPP="gcc -E"
AR="ar"
NM="nm"
RANLIB="ranlib"
```
In the event GCC should be used as the fallback environment, set the appropriate flags in the /etc/portage/package.env file:

**`/etc/portage/package.env`**

**Falling back to*GCC* for *app-foo/bar* and *app-bar/baz***

```
# Compiled using GCC with no link-time optimization since package baz fails using lto
app-bar/baz compiler-gcc
# Compiled using GCC with link-time optimization since package bar compiles using lto
app-foo/bar compiler-gcc-lto
```
## Advanced usage

### LTO

The link-time optimization feature defers optimizing the resulting executables to linking phase. This can result in better optimization of packages but isn't standard behavior in Gentoo yet. Clang uses the linker, [llvm-core/lld](https://packages.gentoo.org/packages/llvm-core/lld), for LTO.

#### Environment

Clang supports two types of link time optimization:

- Full LTO, which is the traditional approach also used by GCC where the whole link unit is analyzed at once. Using it is no longer recommended.
- ThinLTO, where the link unit is scanned and split up into multiple parts.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> With ThinLTO, the final compilation units only contain the code that are relevant to the current scope, thus speeding up compilation, lowering footprint and allowing for more parallelism at (mostly) no cost. ThinLTO is the recommended LTO mode when using Clang.

For full LTO, replace `-flto=thin` with `-flto` in the following examples. There should be no compatibility differences between full LTO and thin LTO.

**`/etc/portage/env/compiler-clang-lto`**

**Environment named*compiler-clang-lto***

```
CFLAGS="${CFLAGS} -flto=thin"
CXXFLAGS="${CXXFLAGS} -flto=thin"        
LDFLAGS="${LDFLAGS}"
 
CC="clang"                         
CXX="clang++"
CPP="clang-cpp" # necessary for xorg-server and possibly other packages
```
As an alternative, LLVM provides its own `AR`, `NM`, and `RANLIB` values. Feel free to use them though mileage may vary over using the standard `AR`, `NM`, and `RANLIB`, since they're intended to handle LLVM bitcode which Clang produces when using the `-flto` flag.

**`/etc/portage/env/compiler-clang-lto`**

**Environment named*compiler-clang-lto***

```
CFLAGS="${CFLAGS} -flto=thin"
CXXFLAGS="${CXXFLAGS} -flto=thin"
 
CC="clang"
CXX="clang++"
CPP="clang-cpp" # necessary for xorg-server and possibly other packages
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

Similar to what was covered earlier in the article, a system wide Clang with LTO enabled can be done by changing the /etc/portage/make.conf file:

**`/etc/portage/make.conf`**

**Setting the system compiler to*Clang***

```
CFLAGS="${CFLAGS} -flto=thin"
CXXFLAGS="${CXXFLAGS} -flto=thin"
 
CC="clang"
CXX="clang++"
CPP="clang-cpp" # necessary for xorg-server and possibly other packages
AR="llvm-ar"
NM="llvm-nm"
RANLIB="llvm-ranlib"
```
Again, it is possible to set `AR`, `NM`, and `RANLIB` to the LLVM implementations. Since earlier in the article compiler environments were set up using Clang without LTO, GCC without LTO, and GCC with LTO, it is now possible to pick and choose which is best on a per package basis. Since the goal is to compile packages system wide with Clang using LTO and not every package will successfully compile using it, fall back to Clang with LTO disabled or GCC. The /etc/portage/package.env may look like the following:

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
### PGO

Emerge [llvm-runtimes/clang-runtime\[sanitize\]](https://packages.gentoo.org/packages/llvm-runtimes/clang-runtime) and [llvm-runtimes/compiler-rt-sanitizers\[profile orc\]](https://packages.gentoo.org/packages/llvm-runtimes/compiler-rt-sanitizers)

`root #``emerge --ask llvm-runtimes/clang-runtime llvm-runtimes/compiler-rt-sanitizers`
And finally, add `USE="pgo"` globally or locally, using [package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) and update:

`root #``emerge --ask --verbose --update --deep --changed-use @world`
### ccache

[ccache](https://wiki.gentoo.org/wiki/Ccache) support is automatic once Clang is emerged.

### Propeller

Propeller is a PGO meant to address BOLT's problems like how much memory it uses. Unlike BOLT, Propeller only works for Clang.

Get [app-arch/zstd](https://packages.gentoo.org/packages/app-arch/zstd) with USE flag [static-libs](https://packages.gentoo.org/useflags/static-libs) [first.](https://wiki.gentoo.org/wiki/USE_flag)

Get Propeller:

Build Propeller from source:

`user $``cmake -G Ninja -B build``user $``ninja -C build generate_propeller_profiles`
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
Sometimes a package will fail to compile even when disabling LTO because it requires another package which was compiled using `-flto` and works incorrectly. This is becoming an issue of the past as more packages installing static libraries adopt dot-a.eclass. Something like the following error may be seen:

**`/var/log/portage/dev-libs:boehm-gc-7.4.2:20160713-085706.log`**

In this case libatomic\_ops is causing boehm-gc to fail compiling. Recompile the program causing the failure using the non-LTO environment and then recompile the new program. In this case, boehm-gc fails when using LTO, so add both of them to the /etc/portage/package.env file to build them without LTO:

**`/etc/portage/package.env`**

**Example*package.env* setup**

```
 compiler-clang
dev-libs/libatomic_ops compiler-clang
```
### Use of GNU extensions without proper -std=

Some packages use GNU extensions in their code without specifying `-std=` appropriately; GCC permits that usage, but Clang disables some of the more specific GNU extensions by default.

If a particular package relies on such extensions being available, then append the correct `-std=` flag to it:

- `-std=gnu89` for C89/C90 with GNU extensions,
- `-std=gnu99` for C99 with GNU extensions,
- `-std=gnu++98` for C++:1998 with GNU extensions.

A common symptom of this problem is multiple definitions of inline functions like this:

**`/var/log/portage/example.log`**

**Example package error in example log**

This is because Clang uses C99 inline rules by default, which are incompatible with gnu89 code; as a workaround, it is likely necessary to pass `-std=gnu89` or set package.env to use GCC for a failing package if passing the proper `-std=` flag doesn't work.

Since both current (2020) GCC and Clang default to `-std=gnu17` with C99 inline rules, these problems have likely been spotted by GCC developers.

### sudo: clang: command not found

Clang is not added to /usr/bin/ and instead lives in a separate path that is added to the `PATH` variable.
Sudo has a whitelisted `PATH` variable that is baked in at compile time. So when a new version of clang is installed, it will not be added to sudo's `PATH` until sudo is re-emerged.

## See also

- [Clang/Desktop profile](https://wiki.gentoo.org/wiki/Clang/Desktop_profile)
- [GCC](https://wiki.gentoo.org/wiki/GCC) — among the most widely used compiler toolchains in the world with official support for: [C](https://wiki.gentoo.org/wiki/C), [C++](https://wiki.gentoo.org/wiki/C%2B%2B), [Objective-C](https://en.wikipedia.org/wiki/Objective-C), [Objective-C++](https://en.wikipedia.org/wiki/Objective-C%2B%2B), [Modula-2](https://en.wikipedia.org/wiki/Modula-2), [Fortran](https://wiki.gentoo.org/wiki/Fortran), [Ada](https://wiki.gentoo.org/wiki/Ada), [Go](https://wiki.gentoo.org/wiki/Go), [COBOL](https://en.wikipedia.org/wiki/COBOL), and [D](<https://en.wikipedia.org/wiki/D_(programming_language)>)

## External resources

- [Playing with LLVM, gold linker and CLANG](https://forums.gentoo.org/viewtopic-t-1084292-start-0.html) Gentoo Forums post
- [Nearly full clang+lto'd system configuration, stable amd64](https://dev.gentoo.org/~juippis/tmp/clangthinltodsystem.txt) July 2022
- [https://blogs.gentoo.org/mgorny/2022/10/07/clang-in-gentoo-now-sets-default-runtimes-via-config-file/](https://blogs.gentoo.org/mgorny/2022/10/07/clang-in-gentoo-now-sets-default-runtimes-via-config-file/)
