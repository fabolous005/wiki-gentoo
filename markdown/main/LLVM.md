<!-- source: https://wiki.gentoo.org/wiki/LLVM | group: Gentoo Wiki (Main) | wiki-title: LLVM -->
---
title: LLVM
url: https://wiki.gentoo.org/wiki/LLVM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-16"
fingerprint: "1c81713a65673fa0"
license: CC BY-SA 4.0
---

# LLVM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The **LLVM** Project is a collection of modular and reusable compiler and toolchain technologies.

"LLVM" is an orphan initialism; originally an acronym standing for *Low Level Virtual Machine*, LLVM today has little to do with [virtual machines](https://wiki.gentoo.org/wiki/Virtualization) under the contemporary understanding of the term.

## Installation

### USE flags


| [+binutils-plugin](https://packages.gentoo.org/useflags/+binutils-plugin) | Build the binutils plugin | 
| [+debug](https://packages.gentoo.org/useflags/+debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [+libffi](https://packages.gentoo.org/useflags/+libffi) | Enable support for Foreign Function Interface library | 
| [debuginfod](https://packages.gentoo.org/useflags/debuginfod) | Install llvm-debuginfod (requires net-misc/curl and dev-cpp/cpp-httplib) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Build and install the HTML documentation and regenerate the man pages | 
| [exegesis](https://packages.gentoo.org/useflags/exegesis) | Enable performance counter support for llvm-exegesis tool that can be used to measure host machine instruction characteristics | 
| [libedit](https://packages.gentoo.org/useflags/libedit) | Use the libedit library (replacement for readline) | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Support querying terminal properties using ncurses' terminfo | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xar](https://packages.gentoo.org/useflags/xar) | Support dumping LLVM bitcode sections in Mach-O files (uses app-arch/xar) | 
| [xml](https://packages.gentoo.org/useflags/xml) | Add support for XML files | 
| [z3](https://packages.gentoo.org/useflags/z3) | Enable support for sci-mathematics/z3 constraint solver | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

### LLVM\_TARGETS

In this example, LLVM\_TARGET\_AArch64 will be removed from `LLVM_TARGETS`. In [/etc/portage/profile/package.use.mask](https://wiki.gentoo.org/wiki//etc/portage/profile/package.use.mask), create a file with the following contents:

**`/etc/portage/profile/package.use.mask/llvm_targets`**

Finally, rebuild LLVM and Clang:

`root #``emerge --ask --oneshot llvm-core/llvm llvm-core/clang`
### Emerge

`root #``emerge --ask llvm-core/llvm`
## LLVM components

LLVM is comprised of the following components:

## Advanced Usage

### LLVM profiles

The LLVM profiles in Gentoo are experimental and intended for playing around with pure-LLVM systems (no GCC).

**Most people do not want these** even if choosing to use Clang to build most packages.

They come with **no guarantees** of support or stability and are *not* simply the same as setting `CC` and `CXX` for Clang; the LLVM profiles use libcxx which means they're ABI-incompatible with the regular profiles using libstdc++.

See also the following bugs:

- LLVM profiles: rename libcxx-using profiles to include libcxx in the name - [bug #944478](https://bugs.gentoo.org/show_bug.cgi?id=944478)
- LLVM profiles: add separate libstdc++ profiles - [bug #944482](https://bugs.gentoo.org/show_bug.cgi?id=944482)
- LLVM profile links should have a warning above them - [bug #944483](https://bugs.gentoo.org/show_bug.cgi?id=944483).
- Btop crashes when compiled with libc++ : [GitHub issue](https://github.com/aristocratos/btop/issues/619)

#### Desktop LLVM profiles

Desktop profiles for LLVM can be created by following [Combining multiple profiles from the Gentoo ebuild repository](<https://wiki.gentoo.org/wiki/Profile_(Portage)#Example_1:_Combining_multiple_profiles_from_the_Gentoo_ebuild_repository>) at one's own risk.

### Using libcxx

Using libcxx / libc++ breaks ABI. Doing so means that GCC cannot be used as a fallback. See the LLVM profile section for more.

### Kernel

The Linux kernel can be compiled with Clang and the LLVM toolchain by defining a kernel environment variable.

`root #``LLVM=1`
To configure Clang specific kernel options such as link-time optimizations or control flow integrity, run the following command:

`root #``LLVM=1 make menuconfig`
The above example demonstrates using `menuconfig`. Other options are `nconfig` and `xconfig`. Next, compile the kernel as normal.

`root #``LLVM=1 make -j$N`
In the past, it was necessary to pass `LLVM_IAS=1` to use the Clang internal assembler for a complete LLVM toolchain built kernel. This is no longer required since `LLVM=1` now defaults to include the Clang internal assembler. Use `LLVM_IAS=0` to disable the internal assembler if desired, otherwise stick to the default behavior.

#### Distribution Kernel

Compile the [distribution kernels](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) (for clarity's sake, not including the binary kernel) with LLVM using the following configs **in addition**  to the values in package.env or make.conf as described in [LLVM/Clang](https://wiki.gentoo.org/wiki/LLVM/Clang#Configuration):

**`/etc/portage/env/llvm-kernel`**

and

**`/etc/portage/package.env/gentoo-kernel`**

### Bootstrapping the LLVM toolchain

For a "pure" Clang toolchain, one can build the whole LLVM stack using itself; this is unnecessary, but users may do so for fun.

Advanced users can choose to bootstrap [LLVM/Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) by building it using the full LLVM toolchain, thus fully dogfooding with clang.

Be warned that due to LLVM's internal structure and style of packaging, this is liable to break across upgrades of major versions.

This is *required* only if looking to move a system to be entirely [GCC](https://wiki.gentoo.org/wiki/GCC)-free (which is currently not possible on [Glibc](https://wiki.gentoo.org/wiki/Glibc) systems).
**This is not yet supported and is dangerous**.

Mixing Clang and GCC should be fine, unless using [default-libcxx](https://packages.gentoo.org/useflags/default-libcxx)[; otherwise, the two should always produce output with the same ABI.](https://wiki.gentoo.org/wiki/USE_flag)

#### Preparing the environment

Prepare the environment for the Clang toolchain:

**`/etc/portage/env/compiler-clang`**

```
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
CC="clang"
CXX="clang++"
LDFLAGS="-fuse-ld=lld -rtlib=compiler-rt -unwindlib=libunwind -Wl,--as-needed"
```
This example replaces not only the compiler but also the GNU linker ld.bfd with the LLVM linker lld which is a drop-in replacement that is significantly faster than the bfd linker.

Set USE flags `default-compiler-rt default-lld llvm-libunwind` for Clang via /etc/portage/package.use:

**`/etc/portage/package.use/clang`**

```
 default-compiler-rt default-lld llvm-libunwind
```
Then install Clang, LLVM, compiler-rt, llvm-runtimes/libunwind, and lld with the default GCC environment:

`root #``emerge llvm-core/clang llvm-core/llvm llvm-runtimes/compiler-rt llvm-runtimes/libunwind llvm-core/lld`
It is also possible to add the `default-libcxx` USE flag to use LLVM's C++ STL with clang, however this is **heavily** discouraged because libstdc++ and libc++ are not ABI compatible (i.e., a program built against libstdc++ will likely break when using a library built against libc++, and vice versa).

Note that [llvm-runtimes/libunwind](https://packages.gentoo.org/packages/llvm-runtimes/libunwind) deals with linking issues that [sys-libs/libunwind](https://packages.gentoo.org/packages/sys-libs/libunwind) has, so it is preferred to use and replace the non-llvm libunwind package if installed (it builds with `-lgcc_s` to resolve issues with `__register_frame` / `__deregister_frame` undefined symbols).

#### Finalizing

Enable the Clang environment for these packages now:

**`/etc/portage/package.env`**

```
 compiler-clang
llvm-core/llvm compiler-clang
llvm-runtimes/libcxx compiler-clang
llvm-runtimes/libcxxabi compiler-clang
llvm-runtimes/compiler-rt compiler-clang
llvm-runtimes/compiler-rt-sanitizers compiler-clang
llvm-runtimes/libunwind compiler-clang
llvm-core/lld compiler-clang
```
Repeat the emerge step with the new environment - the toolchain will be rebuilt using itself instead of GCC:

`root #``emerge llvm-core/clang llvm-core/llvm llvm-runtimes/libcxx llvm-runtimes/libcxxabi llvm-runtimes/compiler-rt llvm-runtimes/compiler-rt-sanitizers llvm-runtimes/libunwind llvm-core/lld`
Clang may now be used with other packages!

### Bootstrapping Rust

On LLVM-based systems using musl, packages may break if dev-lang/rust-bin is the active Rust installation, and is therefore masked ([bug #912154](https://bugs.gentoo.org/show_bug.cgi?id=912154)). Emerge dev-lang/rust requires a Rust installation, which leads to a circular dependency that cannot be resolved.

In this case Rust can be built in two ways:

- [Bootstrapping Rust via (non-LLVM) stage file](https://wiki.gentoo.org/wiki/Bootstrapping_Rust_via_stage_file) on the same host
- [Bootstrapping Rust via cross compilation](https://wiki.gentoo.org/wiki/Bootstrapping_Rust_via_cross_compilation) from another host via cross compilation



## Troubleshooting

### ld.lld: error: undefined symbol: ... std::\_\_1::basic\_string

This means that libc++ has been enabled instead of libstdc++ as the default C++ standard library for Clang by either switching to the [LLVM profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>), installing [llvm-core/clang-common](https://packages.gentoo.org/packages/llvm-core/clang-common) with `USE=default-libcxx`, or by adding `--stdlib=libc++` directly in `CXXFLAGS`. Switching to libc++ breaks ABI compatibility for libraries with a C++ public interface (for example, libLLVM), because libc++ uses the `std::__1` namespace; to use libc++, such libraries must be recompiled with `emerge -av1 llvm-core/llvm && emerge @preserved-rebuild` before installing other software.

### Compiling with GCC on LLVM profile

/usr/src/debug/sys-libs/glibc-2.37-r3/glibc-2.37/csu/../sysdeps/x86\_64/start.S:103: undefined reference to \`main'

Use bfd linker. Add `-fuse-ld=bfd` to `CFLAGS`, `CXXFLAGS`, and `LDFLAGS` at the `/etc/portage/env/compiler-gcc-lto` or `/etc/portage/env/compiler-gcc` configuration files.

### error: cannot open crtbeginS.o: No such file or directory

`musl-ld: error: cannot open crtbeginS.o: No such file or directory`

musl-ld: error: unable to find library -lgcc

musl-ld: error: unable to find library -lgcc\_s

musl-ld: error: unable to find library -lgcc

musl-ld: error: unable to find library -lgcc\_s

[Bug 951445](https://bugs.gentoo.org/951445): `emerge --oneshot --nodeps clang-runtime` after merging compiler-rt: you should then be able to upgrade libcxxabi without trouble.
