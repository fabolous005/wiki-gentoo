<!-- source: https://wiki.gentoo.org/wiki/AMD64/FAQ | group: Gentoo Wiki (Main) | wiki-title: AMD64/FAQ -->
---
title: AMD64/FAQ
url: https://wiki.gentoo.org/wiki/AMD64/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-09"
fingerprint: bfb33a480d232d9e
license: CC BY-SA 4.0
---

# AMD64/FAQ

[AMD64](https://wiki.gentoo.org/wiki/Special:MyLanguage/AMD64)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The Gentoo AMD64 Frequently Asked Questions article.

**Article status**

- Add instructions for [Cool'n'Quiet/PowerNow/SpeedStep](https://wiki.gentoo.org#How_do_I_use_Cool.27n.27Quiet.2FPowerNow.2FSpeedStep_features.3F) for systemd users



## Introduction

This document contains the most commonly asked questions about using Gentoo Linux on **amd64** systems. Many of these questions come from the [forums](https://forums.gentoo.org), IRC, and various [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/all-lists.html). The [AMD64 Project](https://wiki.gentoo.org/wiki/Project:AMD64) provides support for Gentoo Linux on this architecture. For suggestions or questions about this FAQ, please leave a comment on the associated [talk page](https://wiki.gentoo.org/wiki/Talk:AMD64/FAQ).

## Installation

### What's the difference between AMD64 and EM64T?

Both refer to 64-bit processors, one from AMD (AMD64) and the other from Intel (EM64T). AMD's 64-bit processors include the Athlon 64, Opteron, Turion, Neo, Phenom, FX, A*X*/E*X*/C*X*, Ryzen and EPYC families. Intel's 64-bit processors include the Core 2, Core i*X*, Celeron, Xeon, and Pentium (all based on the Core 2 architecture and later). The [last few versions](https://en.wikipedia.org/wiki/X86-64#Intel_64) of the old Pentium 4 and Xeon chips also support EM64T.

The AMD line of 64-bit processors were released well ahead of Intel's offering. Therefore, for historical reasons, the arch keyword for all x86-64 compatible architectures in Gentoo is **amd64**. As such, *AMD64* is a generic reference to 64-bit chips, whether AMD or Intel.

### What CFLAGS should be used?

Simple `CFLAGS` should be used, to avoid future problems. Supported `CFLAGS` values are `-O2 -march=native -pipe` for both AMD and Intel (EM64T) systems.

### What kernel to use?

Almost all kernel sources available in Portage can be used. [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) is recommended because it is a special kernel designed for Gentoo Linux. It contains lots of AMD64-specific patches and therefore is the only kernel officially supported by Gentoo/AMD64.

Make sure the `IA32_EMULATION` option is activated in the kernel.

`root #````
cd /usr/src/linux
```
`root #``make menuconfig`
**Enabling IA32 emulation**

```
Executable file formats / Emulations  --->
  [*] IA32 Emulation
```
### Will using a 64-bit system instead of 32-bit improve performance?

That is a difficult question to answer. In some cases, there may be a performance improvement, in other cases not. For everyday use, there is almost no difference between 64-bit and 32-bit. 64-bit is generally faster when it comes to floating point computing, which is normally the case in multimedia applications and 3D rendering. Linux.com's articles [64-bit performance in Gentoo Linux](https://www.linux.com/news/64-bit-performance-gentoo-linux/) and [64-bit Linux and BSD are maturing steadily](https://www.linux.com/news/64-bit-linux-and-bsd-are-maturing-steadily/) may be of interest. There's also a nice thread, [Article: 64-bit performance in Gentoo Linux](https://forums.gentoo.org/viewtopic-t-349691.html) in the forums.

Most systems will now be 64-bit unless there is a specific requirement otherwise.

### What is multilib and how to use it?

Every AMD64 processor is able to run 32-bit code as well as 64-bit code. However, a 32-bit application is unable to mix it with 64-bit libraries or vice versa. However, natively (without emulation) 32-bit applications can run if all shared libraries they need are available as 32-bit objects. Choose [multilib](https://wiki.gentoo.org/wiki/Multilib) support or not by selecting the according profile. The default is a multilib-enabled profile.

### How to create a 32-bit chroot?

Please read the [32-bit Chroot Guide](https://wiki.gentoo.org/wiki/Project:AMD64/32-bit_Chroot_Guide).

### Is it possible to upgrade from an x86 system to AMD64 by doing emerge -e @world?

Due to several differences between **x86** (32-bit) and **amd64** (64-bit) installations, it is impossible to upgrade this way. Please perform a fresh install using the [AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64).

Note that binary files created on an x86 system most likely cannot be read by packages on an AMD64 system. In particular, some databases, such as OpenLDAP, cannot simply be copied as-is. A database would need to be dumped into an architecture-independent format, such as a text file, before attempting to restore it on an AMD64 system. Database files for MySQL, Berkley DB and SQLite are architecture-independent and can be moved freely after their corresponding daemons, if any, are cleanly shut down.

## Hardware

### Will binary NVIDIA/ATI drivers work?

Yes, NVIDIA and ATI provide drivers that work on AMD64. Either [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) or [x11-drivers/xf86-video-ati](https://packages.gentoo.org/packages/x11-drivers/xf86-video-ati) can be installed. See also the [nvidia-drivers guide](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) or the [ATI FAQ](https://wiki.gentoo.org/wiki/ATI_FAQ).

### How to use Cool'n'Quiet/PowerNow/SpeedStep features?

Custom kernel users must compile the kernel with support for these features. Enable the features below:

**Kernel options for Cool'n'Quiet/PowerNow/SpeedStep**

```
Power management and ACPI options --->
  [*] Device power management core functionality
  [*] ACPI (Advanced Configuration and Power Interface) Support --->
    <*>   Processor
    <*>     Thermal Zone
  CPU Frequency scaling --->
  [*] CPU Frequency scaling
         Default CPUFreq governor (ondemand) --->
  -*-   'performance' governor
  <*>   'userspace' governor for userspace frequency scaling
  -*-   'ondemand' cpufreq policy governor
  <*>   ACPI Processor P-States driver
  CPU Idle  --->
    -*- CPU idle PM support
```
Then install [sys-power/cpupower](https://packages.gentoo.org/packages/sys-power/cpupower) and edit /etc/conf.d/cpupower with a preferred text editor.

**`/etc/conf.d/cpupower`**

**Example configuration**

```
# Options when starting cpufreq (given to the `cpupower` program)
START_OPTS="--governor ondemand"
  
# Options when stopping cpufreq (given to the `cpupower` program)
STOP_OPTS="--governor performance"
```
Now run the following command to make sure the cpupower daemon is started every time the system boots:

`root #``rc-update add cpupower boot`
## Applications

### Upon installing package foo, emerge says: (masked by: \~amd64 keyword)

This means that the package is still in the 'testing' tree. Please read Portage's manual page (man portage), the [Mixing Software Branches](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches) chapter of the [AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64), or [accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package) for more information.

### Upon installing package foo, emerge says: (masked by: missing keyword)

This means that the package has not been tested yet. It does *not necessarily* mean that the package does not work. It can be unmasked by accepting another architecture (or set of architectures) for the package. The following example shows how to tell Portage to accept the installation of the given package, under any keyword:

`root #````
echo "category/packagename **" >> /etc/portage/package.accept_keywords
```
`root #``emerge packagename`
If the package works well, please file a bug on [Gentoo's Bugzilla](https://bugs.gentoo.org/), so that the AMD64 team can keyword the package and other users can benefit.
