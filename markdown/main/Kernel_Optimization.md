<!-- source: https://wiki.gentoo.org/wiki/Kernel/Optimization | group: Gentoo Wiki (Main) | wiki-title: Kernel/Optimization -->
---
title: Kernel/Optimization
url: https://wiki.gentoo.org/wiki/Kernel/Optimization
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-12"
fingerprint: "601111f64aa3f20"
license: CC BY-SA 4.0
---

# Kernel/Optimization

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

- Fix GCC PGO

- Add other ways to apply patches due to new kernel package like sys-kernel/gentoo-kernel



This article describes various optimizations for the [Linux kernel](https://wiki.gentoo.org/wiki/Kernel), including speed and hardening.

## Prerequisites

The article assumes the user is using `sys-kernel/gentoo-sources` and that /usr/src/linux is the [symbolic link](https://wiki.gentoo.org/wiki/Kernel/Configuration#Set_symlink) to the current kernel. **C**hange **d**irectory to /usr/src/linux before continuing:

`user $``cd /usr/src/linux`
One way to optimize the kernel is to remove what users don't need. For example, if not using [KVM](https://wikipedia.org/wiki/Kernel-based_Virtual_Machine), then remove `CONFIG_KVM`:

**Disable KVM (`CONFIG_KVM`) support**

### Kbuild

The [**K**ernel **build**](https://www.kernel.org/doc/html/latest/kbuild/index.html) system can be used to change how Kernel builds in a more advanced way than make \*config, similar to [GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html). Kbuild also support [Environment Variables](https://www.kernel.org/doc/html/latest/kbuild/kbuild.html) like `LLVM=1`. For example, the kernel will be build with LLVM and with aggressive optimization flags:

`root #``make LLVM=1 KCFLAGS="-O3 -march=native -pipe"`
### Experimental USE flag

The user may turn on [experimental](https://packages.gentoo.org/useflags/experimental) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) to be able to use more features, like **-march=native**:

**`/etc/portage/package.use/gentoo-sources`**

```
sys-kernel/gentoo-sources experimental
```
Starting with kernel version 6.16, this is no longer necessary, as there is now a separate option for this:

**Kernels 6.16+**

```
Processor type and features  --->
  [*] Build and optimize for local/native CPU 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_X86_NATIVE_CPU</code> to find this item.
### Clang/LLVM

Make sure the LLVM toolchain is installed before proceeding:

`root #``emerge --pretend --noreplace llvm-core/clang llvm-core/llvm llvm-runtimes/compiler-rt llvm-runtimes/libunwind llvm-core/lld`
By default, the kernel is build under [GNU binutils](https://www.gnu.org/software/binutils). The following environment variables are used: CC, LD, AR, NM, STRIP, OBJCOPY, OBJDUMP, READELF, HOSTCC, HOSTCXX, HOSTAR, and HOSTLD. Alternatively, the kernel may be build using [LLVM binutils](https://llvm.org/docs/CommandGuide/#gnu-binutils-replacements):

`root #``make LLVM=1`
### \*FLAGS

By default, [most of the kernel](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Makefile#n801) is build with C's `-O2` (some code, like Random Number Generation, does not work with optimizations and sometimes checked with the C macro `__OPTIMIZE__`). This can be changed via [Kbuild](https://wiki.gentoo.org/wiki/Kernel/Optimization#Kbuild). Before making any KCFLAGS and similar flags, please check the kernel's Makefiles before it gets any changes. For example, `-fallow-store-data-races` is disabled on this [Makefile](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Makefile#n818).

#### -O3

The command to add this flag is:

`root #``make KCFLAGS="-O3"`
There was a [official attempt](https://lore.kernel.org/lkml/20220621133526.29662-1-mikoxyzzz@gmail.com/) to add `-O3` to the kernel but [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds) [reject](https://lore.kernel.org/lkml/CA+55aFz2sNBbZyg-_i8_Ldr2e8o9dfvdSfHHuRzVtP2VMAUWPg@mail.gmail.com/) it due to `-O3` historically outputting worse code than `-O2`. Phoronix ran a [`-O3` kernel benchmark](https://www.phoronix.com/review/linux-kernel-o3) and found nearly all tested programs to have no measurable benefit.

## Performance

Performance means how fast the kernel runs.

### Link Time Optimization

Enabling [**L**ink **T**ime **O**ptimization](https://en.wikipedia.org/wiki/Interprocedural_optimization) is not simple as make KCFLAGS="-flto". Except Clang's ThinLTO, the whole kernel will be recompiled if at least one CONFIG option change. See [Clang LTO](https://wiki.gentoo.org/wiki/Kernel/Optimization#Clang_LTO) and [GCC LTO](https://wiki.gentoo.org/wiki/Kernel/Optimization#GCC_LTO) for more information.

#### GCC LTO

Andi Kleen and others have made [experimental patches](https://lore.kernel.org/lkml/20221114114344.18650-1-jirislaby@kernel.org) for this and will be used to apply GCC [LTO](https://gcc.gnu.org/onlinedocs/gccint/LTO.html). For more information, see [LWN article](https://lwn.net/Articles/744507).

First, download the following 2 patches from [CachyOS's kernel patches](https://github.com/CachyOS/kernel-patches/):

Then, apply the patch using [git](https://wiki.gentoo.org/wiki/Git):

`root #``mv gcc-lto.patch``root #``git apply gcc-lto-no-pie.patch`
Alternatively, when using [distribution kernels](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) users can add these patches in [/etc/portage/patches](https://wiki.gentoo.org/wiki//etc/portage/patches)/sys-kernel/\*-kernel/\*.patch.

Afterwards, [enable](https://wiki.gentoo.org/wiki/Kernel/Upgrade#Update_the_.config_file) GCC LTO on the kernel and enjoy:

`root #``make oldconfig`
Link Time Optimization (LTO)
> 1. None (LTO\_NONE)
  2. gcc LTO (LTO\_GCC) (NEW)
choice\[1-2?\]: 2
Allow aggressive cloning for function specialization (LTO\_CP\_CLONE) \[N/y/?\] (NEW) n

To remove the patch:

`root #``git apply gcc-lto.patch --reverse``root #``git apply gcc-lto-no-pie.patch --reverse``root #``rm gcc-lto.patch gcc-lto-no-pie.patch`
#### Clang LTO

[Clang's **L**ink **T**ime **O**ptimization](https://www.llvm.org/docs/LinkTimeOptimization.html) can be either FullLTO or [ThinLTO](https://clang.llvm.org/docs/ThinLTO.html) for 5.12+ Linux kernel:

**Enable Clang's LTO (`CONFIG_LTO_CLANG_FULL` and `CONFIG_LTO_CLANG_THIN`) support**

The difference between these two are that ThinLTO compiles faster due to parallelization and less memory usage. ThinLTO may [sometimes](https://llvm.org/devmtg/2016-11/Slides/Amini-Johnson-ThinLTO.pdf) improve or decrease performance.

### Profile Guided Optimization

The Clang's package [llvm-core/clang-runtime](https://packages.gentoo.org/packages/llvm-core/clang-runtime) will pull in [sys-libs/compiler-rt-sanitizers](https://packages.gentoo.org/packages/sys-libs/compiler-rt-sanitizers) by default via the `sanitize` `USE` flags to be able to use **P**rofile **G**uided **O**ptimization. Users who customize their `USE` flags and don't want the extra Clang sanitizers will need to ensure `profile` and `orc` are set locally in /etc/portage/package.use:

`root #``nano /etc/portage/package.use/compiler-rt-sanitizers.use`
**`/etc/portage/package.use/compiler-rt-sanitizers.use`**

*# required USE flags for pgo
sys-libs/compiler-rt-sanitizers profile orc*

Install the Clang sanitizers:

`root #``emerge --ask --changed-use sys-libs/compiler-rt-sanitizers`
#### GCC PGO

To use [**P**rofile-**G**uided **O**ptimization](https://en.wikipedia.org/wiki/Profile-guided_optimization), activate [debugfs](https://www.kernel.org/doc/html/latest/filesystems/debugfs.html) and [gcov](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html) support (See [this](https://www.kernel.org/doc/html/latest/dev-tools/gcov.html) for modern info):

**Enable debugfs (`CONFIG_DEBUG_FS`) and gcov (`CONFIG_GCOV_KERNEL` and `CONFIG_GCOV_PROFILE_ALL`) support**

The environment variable `CFLAGS_GCOV`, used when `CONFIG_GCOV_KERNEL` is on, defaults to `-fprofile-arcs -ftest-coverage`, but can be changed to `-fprofile-generate -ftest-coverage` or similar in [Instrumentation Options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html):

`root #``make CFLAGS_GCOV="-fprofile-generate -ftest-coverage"`
Then build as [usual](https://wiki.gentoo.org/wiki/Kernel/Configuration), [setup](https://wiki.gentoo.org/wiki/Kernel/Configuration#Setup) the kernel and reboot the system using the command:

`root #``reboot`
After booted back to system, run the system with many programs: [play sound](https://wiki.gentoo.org/wiki/ALSA), game, run [Firefox](https://wiki.gentoo.org/wiki/Firefox) and so on. The longer the system is run and with more different programs, the higher instrumented data gets. When satisfied with the instrumented data, copy /sys/kernel/debug/gcov/usr/src/linux/\*gcda files to /usr/src/linux:

`root #``cd /sys/kernel/debug/gcov/usr/src/linux``root #``find . -name '*.gcda' -exec cp {} /usr/src/linux/{} \;`
Then disable `CONFIG_GCOV_KERNEL` and `CONFIG_GCOV_PROFILE_ALL` and edit the `KCFLAGS`:

**Disable gcov (`CONFIG_GCOV_KERNEL` and `CONFIG_GCOV_PROFILE_ALL`) support**

`root #``make KCFLAGS="-fprofile-use -fprofile-correction -Wno-error=missing-profile -Wno-error=coverage-mismatch"`
Like before, setup the kernel and finally reboot. To remove /usr/src/linux/\*gcda files, run the command:

`root #``cd /usr/src/linux``root #``find . -name '*.gcda' -exec rm {} \;`
#### Clang PGO

Download the patch and apply the patch:

`root #``git apply clang-pgo.patch`
Configure the kernel with [LLVM](https://wiki.gentoo.org/wiki/Kernel/Optimization#Clang.2FLLVM):

`root #``make menuconfig LLVM=1`
Configure the kernel as follows:

**Disable[Clang's LTO](https://wiki.gentoo.org/wiki/Kernel/Optimization#Clang_LTO) (`CONFIG_LTO_CLANG_FULL` and `CONFIG_LTO_CLANG_THIN`) support. Then enable Clang's PGO (`CONFIG_PGO_CLANG`)**

Then build as [usual](https://wiki.gentoo.org/wiki/Kernel/Configuration), [setup](https://wiki.gentoo.org/wiki/Kernel/Configuration#Setup) the kernel and reboot the system using the command:

`root #``reboot`
After booted back to system, clear any PGO data:

`root #``echo 1 | tee /proc/pgo/reset`
Run the system with many programs: [play sound](https://wiki.gentoo.org/wiki/ALSA), game, run [Firefox](https://wiki.gentoo.org/wiki/Firefox) and so on. The longer the system is run and with more different programs, the higher instrumented data gets. When satisfied with the instrumented data, collect the raw profile data:

`root #``cp -a /proc/pgo/vmlinux.profraw /tmp/vmlinux.profraw`
Then process the raw profile data using [**llvm-profdata**](https://llvm.org/docs/CommandGuide/llvm-profdata.html):

`root #``cd /usr/src/linux``root #``llvm-profdata merge --output=vmlinux.profdata /tmp/vmlinux.profraw`
Disable Clang's PGO and optionally enable Clang's LTO:

`root #``make menuconfig LLVM=1`
**Optionally enable[Clang's LTO](https://wiki.gentoo.org/wiki/Kernel/Optimization#Clang_LTO) (`CONFIG_LTO_CLANG_FULL` and `CONFIG_LTO_CLANG_THIN`) support. Then disable Clang's PGO (`CONFIG_PGO_CLANG`)**

Then compile the kernel with Clang's PGO and enjoy the faster kernel:

`root #``make KCFLAGS=-fprofile-use=/usr/src/linux`
The user may now remove the patch:

`root #``git apply clang-pgo.patch --reverse``root #``rm clang-pgo.patch`
#### Clang AutoFDO

Using AutoFDO requires 6.13+ and can be combined with Propeller. Follow these [instructions](https://www.kernel.org/doc/html/next/dev-tools/autofdo.html).

#### Clang Propeller

Using Propeller requires 6.13+ and can be combined with AutoFDO. Follow these [instructions](https://www.kernel.org/doc/html/next/dev-tools/propeller.html). Propeller has much [lower memory usage than BOLT and achieve similar performance](https://dl.acm.org/doi/pdf/10.1145/3575693.3575727).

#### BOLT

Follow this [link instructions](https://github.com/llvm/llvm-project/blob/main/bolt/docs/OptimizingLinux.md).

## Hardened

[Hardening](<https://en.wikipedia.org/wiki/Hardening_(computing)>) refers to reducing the potential for malware to damage the system.

**Enable hardening**

[Pietinger](https://wiki.gentoo.org/wiki/User:Pietinger/Tutorials/Kernel_Hardening_with_KSPP), [Kicksecure](https://www.kicksecure.com/wiki/Hardened-kernel), and [Clip OS](https://docs.clip-os.org/clipos/kernel.html) has more hardened config options to the kernel.

## Size

This section describes reducing kernel memory usage (useful for embedded systems).

5.4+ kernel officially support `-Os` flag:

**Enable`-Os` (`CONFIG_CC_OPTIMIZE_FOR_SIZE`)**

`-Oz` may also be instead use to more aggressively reduce size than `-Os`:

`root #``make KCFLAGS="-Oz"`
