<!-- source: https://wiki.gentoo.org/wiki/Alpha/FAQ | group: Gentoo Wiki (Main) | wiki-title: Alpha/FAQ -->
---
title: Alpha/FAQ
url: https://wiki.gentoo.org/wiki/Alpha/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-17"
fingerprint: "8747545641272b47"
license: CC BY-SA 4.0
---

# Alpha/FAQ

[Alpha](https://wiki.gentoo.org/wiki/Special:MyLanguage/Alpha)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This FAQ is intended to answer some of the most frequently asked questions relating to Alpha on Gentoo and Alpha on Linux in general.

### Introduction

This document contains answers to the most commonly asked questions about Gentoo/Alpha and Alpha/Linux. It's aimed at both new users and experienced users alike.

## About the Alpha Project

### What is Alpha?

The Alpha project is responsible for keeping the Gentoo distribution updated and current for the Alpha platform. This primarily includes keywording packages, porting software, and making new releases. The Alpha project aims to provide a usable environment for both desktops and servers.

## The Alpha Architecture

### Isn't the Alpha architecture dead?

Yes. HP stopped taking orders for new Alpha systems on April 27, 2007.

### Can I run 32-bit applications?

There is no such thing as a 32-bit Alpha, the architecture was 64-bit only from its inception. You cannot even force gcc to output 32 bit code using the `-m32` compiler flag.

### Is Alpha big endian or little endian?

Alpha systems are little endian. A large Cray system using Alphas was big endian, but Linux will never support it.

### Can I run Linux/x86 applications on Alpha?

No, not without a x86 emulator like [QEMU](https://wiki.gentoo.org/wiki/QEMU). In the past DEC/Compaq/HP maintained a program called `em86` which allowed Linux/x86 applications to be run on Linux/Alpha systems. However, the program is closed source and no longer maintained upstream. There are no `em86` binaries available that are compatible with gcc-3's ABI.

### Why install Gentoo on Alpha?

Admittedly, you can find faster 64-bit systems out there, like amd64. However, that doesn't mean that Alpha systems are useless. The following are a few good reasons why you'd want to install Linux on this sort of hardware:

- You can turn an old computer into something useful. Older Alphas make great routers and servers. Their desktop performance isn't that bad either.
- You will learn about unusual computer hardware.
- It's cool. How many people can say they've got Linux running on an Alpha?

### Where can I buy Alpha hardware?

Alpha hardware generally isn't available at your local computer store. [eBay](https://www.ebay.com/) is usually a good option, assuming that the shipping doesn't cost you too much. There are several used equipment dealers that can be found via Google, however most of them charge too much.

## Hardware

### I opened up the case on an AlphaServer and now it won't power on, what do I do?

AlphaServers have interlock switches to prevent the system from running while the chassis is open. When the chassis is open, so are the switches, and power is cut to the system. Some AlphaServers, like the ES40, have multiple access panels in the chassis; on these systems, only certain panels being open may cause the computer to shut off, whereas others may just cause it to complain on the control panel. Some AlphaServers can have a jumper set on the mainboard that causes them to remain powered on even if the chassis is open. Some AlphaServers, like the 2100, are finicky, and their interlock switches might still be open even if the chassis is closed - try tightening the panel screws or jiggling the chassis panels a little.

If it becomes truly necessary to power a system up with the chassis open, try locating the interlock switch(es) and jamming a nonconductive spudger in it to force it closed. Interlock switches can be spotted by examining the edges of the chassis's panels for protrusions with a matching slit in the chassis proper, or by carefully reading the computer's manual.

## Alpha Specific Compiler Flags

### Is the use of Alpha specific compiler flags recommended?

As with all compiler optimizations, don't over do it. Use these flags with care and they will give you a stable, speedy system; use them too aggressively and you could seriously break your system. When in doubt, leave it out. If you need further clarification, check the gcc [man page](https://wiki.gentoo.org/wiki/Man_page).

### How do I optimize for a specific instruction set and scheduling model?

You can optimize for a specific instruction set and scheduling model with `-mcpu=XXX`, where XXX is your cpu type. Valid cpu types are `ev4`, `ev45`, `ev5`, `ev56`, `pca56`, `ev6` and `ev67`. If you do not use the `-mcpu=XXX` flag, then `gcc` will default to the cpu type of the machine it was compiled on. `-mcpu=` is equivalent to the `-march=` flag on x86.

### How do I specify the scheduling model without changing the instruction set?

You can specify the scheduling model without changing the instruction set with `-mtune=XXX`, where XXX is your cpu type. Valid cpu types are `ev4`, `ev45`, `ev5`, `ev56`, `pca56`, `ev6` and `ev67`. `-mtune=` is implied by `-mcpu=`, so you don't need to specify this with if you set `-mcpu=`. `-mtune=` is equivalent to the `-mcpu=` flag on x86.

### How do I enable software assisted floating point numbers?

You can enable software assisted floating point numbers with `-mieee`. This will enable software assistance to correctly support denormalized numbers and exceptional IEEE values such as not-a-number and plus/minus infinity.

This should be in your global `CFLAGS` unless you really know what you are doing.

### How do I specify the memory latency of my system?

You can specify the memory latency of your system with `-mmemory-latency=XXX`, where XXX is the number of clock cycles. This sets the latency time that the scheduler should assume for memory references. `gcc` has some predefined "typical" values that are also accepted by `-mmemory-latency=XXX`. Those values are `L1` for Dcache, `L2` for Scache, `L3` for Bcache, and `main` for main memory.

This flag should probably not be use in your global `CFLAGS` as the value you choose is highly dependent on the memory access pattern of the application you are compiling.

### How do I specify that static data should be placed in the small data area?

You can specify that static data should be placed in the small data area with `-msmall-data`. This will cause `gcc` to take advantage of some features of the Alpha architecture by storing heavily used pieces of data in specific sections called the small memory sections. The sections are limited to 64KiB which means accessing anything stored in them will only take one instruction.

`-fPIC` will override`-msmall-data`, so you should use the`-fpic` flag instead if you do not want`-msmall-data` to be overridden.

This flag should probably not be used in your global `CFLAGS` as some objects require more than 64KiB to be stored, and this will cause a gp\_overflow ld error.

### How do I specify that object code should be placed in the small text area?

You can specify that object code should be placed in the small text area with `-msmall-text`. This will cause `gcc` to assume that the entire program (or library) fits in 4MB, and is thus reachable with a branch instruction. This reduces the number of instructions required for a function call from 4 to 1.

This flag should probably not be used in your global `CFLAGS` as some programs (or libraries) are larger than 4MB, and this will cause an ld error.

### What do I do if any of these compiler flags cause a compile, linker, or runtime error?

As [CFLAGS](https://wiki.gentoo.org/wiki/Alpha/FAQ#Alpha_Specific_Compiler_Flags), please use these `CFLAGS` cautiously. They may break your system. If you experience problems, try using less aggressive `CFLAGS` before [bugs](https://wiki.gentoo.org/wiki/Alpha/FAQ#How_do_I_report_bugs.3F).

## Alpha Booting and Bootloaders

### What is SRM?

SRM is based on the Alpha Console Subsystem specification, which provides an operating environment for OpenVMS, Tru64 UNIX, and Linux operating systems. See the [SRM Howto](https://cdn.rawgit.com/alphalinux/web/master/faq/SRM-HOWTO/index.html) for more information about SRM.

### What is ARC?

ARC is based on the Advanced RISC Computing (ARC) specification, which provides an operating environment for Windows NT.

### How can I tell if which firmware my Alpha has, ARC or SRM?

When you power your system on you will see a screen with a blue background and white text if it is using SRM. You will see a graphical boot system if it is using ARC.

### Which bootloader do I use?

If your Alpha system supports SRM, then you should use aboot. If your system supports both SRM and ARCs (ARC, AlphaBIOS, ARCSBIOS), then you should follow [the SRM Firmware Howto](https://cdn.rawgit.com/alphalinux/web/master/faq/x31.html) for switching to SRM and use aboot. If your system can only use ARCs (Ruffian, xl, etc.) then you will need to choose milo.

### Can aboot boot kernels from EXT4, XFS, ReiserFS, or JFS partitions?

No, `aboot` only supports booting kernels from `ext2` and `ext3` partitions. You may still use other files systems for your root partition, you just need to create a /boot partition with either `ext2` or `ext3` on it.

Support for ext4 is planned. In fact, there are patches floating around that add support for [ReiserFS](https://bugs.gentoo.org/show_bug.cgi?id=106855), [XFS](https://bugs.gentoo.org/show_bug.cgi?id=107103), and [ext4](http://alphalinux.org/wiki/index.php/Aboot_EXT4_support).

## Software

### How is Java support in Alpha?

Compaq/HP offered binary packages, called `compaq-jre` and `compaq-jdk`. They stopped all support, and the packages were removed from Gentoo on October 11, 2006. Since then, we haven't supported Java.

### Will there ever be working Java support?

It's possible to use `icedtea` on Alpha, but we have no plans to support it, due to the high maintenance costs involved with testing hundreds of additional packages.

### How is .NET support in Alpha?

Neither of the two main open source efforts, [mono](https://www.mono-project.com/) nor [DotGNU](https://www.dotgnu.org/), support the alpha architecture.

### What is needed for .NET support?

Bringing .NET to Alpha isn't a simple task. It requires coding all of the assembly language code generation macros for the JIT (just in time) compiler and coding architecture specific functions (register allocators, etc).

## Alpha Linux Kernel

### Which kernel sources should I use?

We recommend using `gentoo-sources` but `vanilla-sources` is also supported by Alpha. You should read the [kernel packages](https://wiki.gentoo.org/wiki/Kernel/Packages) article to determine which one is right for your needs.

### My system is broken with kernel-2.6, Can I build a 2.4 based system?

Alpha stopped supporting 2.4 kernel and profile in January 2008. Technically you can build a 2.4 Gentoo System (the kernel is in the tree) but you will have to mask all the 2.6 only packages by yourself and you won't have support from developers.

Alpha will review all bug reports about non-working 2.6 systems and will try to help as much as we can.

### Are hardened-sources supported?

Short answer is no. They were supported some time ago, but we lost the hardware and the maintainer so now they are not supported. If you are interested and want to continue the initial work, please let us know.

### Does the 2.6 kernel support Alpha SMP?

There were some times in the past when it was broken. However, modern kernel versions are known to work well.

## Helping out the Alpha project

### How do I report bugs?

If you find a bug or you want to send us patches, then you need to file a bug report. It only takes you a little bit of your time, and your help is really appreciated. Please follow the [Gentoo Bug Reporting Guide](https://wiki.gentoo.org/wiki/Bugzilla/Guide).

### How can I help the Alpha project?

Users can help us by:

- Filing clear and concise [bugs](https://wiki.gentoo.org/wiki/Alpha/FAQ#How_do_I_report_bugs.3F).
- Testing software that does not yet have the **\~alpha** keyword.
- Answering questions on the [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/), [forum](https://forums.gentoo.org/viewforum-f-32.html) and IRC channel.
- Sending patches
- Becoming an [AT](https://wiki.gentoo.org/wiki/Project:Alpha/Arch_tester)

### Is there an Alpha arch testers project?

Yes, see the [Alpha arch testers project](https://wiki.gentoo.org/wiki/Project:Alpha/Arch_tester) page for details.

## Troubleshooting

### Why is Xorg locking up my Alpha?

This is usually caused by a bad `-mcpu` setting. For example, the AlphaServer 800 needs to be compiled with the `ev5` flag for xorg **and** glibc. Failing to do so will cause undesired results.

## Other Resources

### Why is there no Gentoo on Alpha forum?

On the [Gentoo Forums](https://forums.gentoo.org) there are several arch specific categories like [Gentoo on PPC](https://forums.gentoo.org/viewforum-f-24.html) and [Gentoo on Sparc](https://forums.gentoo.org/viewforum-f-27.html). There is no Gentoo on Alpha category because there simply isn't enough Alpha activity on the forums to warrant one. All Alpha related posts should go in the [Gentoo on Alternative Architectures](https://forums.gentoo.org/viewforum-f-32.html) category.

### Where can I get more help or information about Alpha?

If any of your questions weren't answered here, you can try other resources like the [Alpha Handbook](https://wiki.gentoo.org/wiki/Handbook:Alpha) or [our project homepage](https://wiki.gentoo.org/wiki/Project:Alpha). You can also ask us at [#gentoo-alpha](ircs://irc.libera.chat/#gentoo-alpha) ([webchat](https://web.libera.chat/#gentoo-alpha)), [Gentoo on Alternative Architectures Forum](https://forums.gentoo.org/viewforum-f-32.html), or on the [gentoo-alpha@lists.gentoo.org](mailto:gentoo-alpha@lists.gentoo.org) mailing list.

### Where can I get more help or information about Alpha on Linux?

[AlphaLinux.org](http://www.alphalinux.org/wiki/index.php/Main_Page) is a central repository for all things Alpha/Linux.
