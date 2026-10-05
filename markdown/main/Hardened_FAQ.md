<!-- source: https://wiki.gentoo.org/wiki/Hardened/FAQ | group: Gentoo Wiki (Main) | wiki-title: Hardened/FAQ -->
---
title: Hardened/FAQ
url: https://wiki.gentoo.org/wiki/Hardened/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-20"
fingerprint: "8ecedb9b0d27bfa0"
license: CC BY-SA 4.0
---

# Hardened/FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Frequently Asked Questions that arise on the [#gentoo-hardened](ircs://irc.libera.chat/#gentoo-hardened) ([webchat](https://web.libera.chat/#gentoo-hardened)) IRC channel and the gentoo-hardened mailing list.

### Introduction

The following is a collection of questions collected from [#gentoo-hardened](ircs://irc.libera.chat/#gentoo-hardened) ([webchat](https://web.libera.chat/#gentoo-hardened)) IRC channel and the gentoo-hardened mailing list. As such, is geared towards answering fast and concisely rather than providing a whole insight on the technologies behind Gentoo Hardened. It is advisable reading the rest of the documentation on the Gentoo Hardened Project page and that on the projects' home pages in order to get a better insight.

## General Questions

### What exactly is the "toolchain"?

The term "toolchain" refers to the combination of software packages commonly used to build and develop for a particular architecture. The toolchain you may hear referred to in the [#gentoo-hardened](ircs://irc.libera.chat/#gentoo-hardened) ([webchat](https://web.libera.chat/#gentoo-hardened)) IRC channel consists of the GNU Compiler Collection (GCC), binutils, and the GNU C library (glibc).

### What should I use: AppArmor or SELinux?

The answer to this question is highly subjective, and very dependent on your requisites so the hardened Gentoo project simply tries to lay out each technology and leave the choice up to the user. This decision requires a lot of research that we have hopefully provided clearly in the hardened documentation. However, if you have any specific questions about the security model that each provides, feel free to question the relevant developer in our IRC channel or on the mailing list.

### Do I need to pass any flags to LDFLAGS/CFLAGS in order to turn on hardened building?

No, please see the [current toolchain](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Changes) modifications in Gentoo Hardened. For GCC, they work via patching the compiler, although in the past, we used custom specfiles and may return to that in future. For Clang, they work via configuration files installed by [sys-devel/clang-common](https://packages.gentoo.org/packages/sys-devel/clang-common).



### Can I add -fstack-protector-all or -fstack-protector in the make.conf CFLAGS?

No, they will likely break the building of many packages, amongst others glibc. It is better that you let the profile do its job.

### How do I turn off hardened building?

The hardened compilation options are made up of several different measures.

For GCC, hardening is currently implemented via patching the compiler to tweak default settings. In the past, Gentoo used specfiles, and may return to using them in future.

For Clang, the options are controlled via configuration files in /etc/clang installed by [sys-devel/clang-common](https://packages.gentoo.org/packages/sys-devel/clang-common).

#### SSP

To turn off default SSP building when using the hardened toolchain, GCC can be built with `USE=-ssp`.

One can also append `-fno-stack-protector` to `CFLAGS` and `CXXFLAGS` for either compiler.

#### PIE

To turn off default PIE building, GCC should be built with `USE=-pie`.

One can also append `-nopie` to `CFLAGS` and `CXXFLAGS` for either compiler.

#### FORTIFY\_SOURCE

To turn off default FORTIFY\_SOURCE, append `-U_FORTIFY_SOURCE` to `CFLAGS` and `CXXFLAGS` for either compiler.

#### BIND\_NOW

To turn off default now binding (BIND\_NOW), GCC should be built with `USE=-default-znow`.

One can also append `-z,lazy` to `LDFLAGS`.

#### RELRO

To turn off default relro binding (RELRO), append `-z,norelro` to your `LDFLAGS`.

Binutils also defaults to relro via `--enable-relro` as a configure argument. One can set `EXTRA_ECONF="--disable-relro"` via package.env to disable this.

### I just found out about the hardened project; do I have to install everything on the project page in order to install Hardened Gentoo?

No, the Hardened Gentoo Project is a collection of subprojects that all have common security minded goals. While many of these projects can be installed alongside one another, some conflict as well such as several of the ACL implementations that Hardened Gentoo offers.

### How do I switch to the hardened profile?

To change the system profile use the `eselect` utility.

`root #``eselect profile list`
\[1\]   default/linux/amd64/10.0
\[2\]   default/linux/amd64/10.0/desktop
\[3\]   default/linux/amd64/10.0/desktop/gnome \*
\[4\]   default/linux/amd64/10.0/desktop/kde
\[5\]   default/linux/amd64/10.0/developer
\[6\]   default/linux/amd64/10.0/no-multilib
\[7\]   default/linux/amd64/10.0/server
\[8\]   hardened/linux/amd64
\[9\]   hardened/linux/amd64/no-multilib
\[10\]  selinux/2007.0/amd64
\[11\]  selinux/2007.0/amd64/hardened
\[12\]  selinux/v2refpolicy/amd64
\[13\]  selinux/v2refpolicy/amd64/desktop
\[14\]  selinux/v2refpolicy/amd64/developer
\[15\]  selinux/v2refpolicy/amd64/hardened
\[16\]  selinux/v2refpolicy/amd64/server

`root #``eselect profile set 8`
Of course replace `8` in the command above with the number of the desired hardened profile.

After setting up your profile, you should recompile your system using a hardened toolchain so that you have a consistent base:

`root #``emerge -1 gcc`
Make sure the hardened compiler is being used (`gcc` version may vary):

`root #``gcc-config -l`
\[1\] x86\_64-pc-linux-gnu-4.4.4 \*
 \[2\] x86\_64-pc-linux-gnu-4.4.4-hardenednopie
 \[3\] x86\_64-pc-linux-gnu-4.4.4-hardenednopiessp
 \[4\] x86\_64-pc-linux-gnu-4.4.4-hardenednossp
 \[5\] x86\_64-pc-linux-gnu-4.4.4-vanilla

If the hardened version is not chosen select it. The hardened version is the one without any of the suffixes.

`root #````
gcc-config x86_64-pc-linux-gnu-4.4.4
```
`root #``source /etc/profile`
Now rebuild the rest of the toolchain:

`root #``emerge -1 binutils glibc`
Now keep emerging the system

`root #````
emerge -e --keep-going @system
```
`root #``emerge -e --keep-going @world`
The `--keep-going` option is added to ensure emerge won't stop in case any package fails to build. If that occurs however, you need to make sure that the remainder of the packages is built. You can check the output of emerge at the end to find out which packages were not rebuilt.

### How do I debug with gdb?

We have written a [document on how to debug with Gentoo Hardened](https://wiki.gentoo.org/wiki/Hardened/Debugging), so following the recommendations there should fix your problem.

### Why are the jit and orc flags disabled in the hardened profile?

JIT means Just In Time Compilation and consist on taking some code meant to be interpreted (like Java bytecode or JavaScript code) compile it into native binary code in memory and then executing the compiled code. This means that the program need a section of memory which has write and execution permissions to write and then execute the code which is denied by PaX, unless the `mprotect` flag is unset for the executable. As a result, we disabled the JIT use flag by default to avoid complaints and security problems. ORC uses Just In Time Compilation (jit).

You should bear in mind that having a section which is written and then executed can be a serious security problem as the attacker needs to be able to exploit a bug between the write and execute stages to write in that section in order to execute any code it wants to.

### How do I enable the jit or orc flag?

If you need it, we recommend enabling the flag in a per package basis using /etc/portage/package.use:

Anyway, you can enable the use flag globally using /etc/portage/make.conf:



## PaX questions

### Where is the homepage for PaX?

That is [the homepage for PaX](https://pax.grsecurity.net).

### What Gentoo documentation exists about PaX?

Currently the only Gentoo documentation that exists about PaX is a [PaX quickstart guide](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Quickstart).

### How do PaX markings work?

PaX markings are a way to tell PaX which features should enable (or disable) for a certain binary.

Features can either be enabled, disabled or not set. Enabling or disabling them will supersede the kernel action, so a binary with a feature enabled will always use the feature and one with a feature disabled won't ever used it.

When the feature status is not set the kernel will choose whether to enable or disable it. By default, the hardened kernel will enable those features with only two exceptions, the feature is not supported by the architecture/kernel or PaX is running in Soft Mode. In those two cases, it will be disabled.



Text relocations are a way in which references in the executable code to addresses not known at link time are solved. Basically they just write the appropriate address at runtime marking the code segment writable in order to change the address then unmarking it. This can be a problem as an attacker could try to exploit a bug when the text relocation happens in order to be able to write arbitrary code in the text segment which would be executed. As this also means that code will be loaded on fixed addresses (not be position independent) this can also be exploited to pass over the randomization features provided by PaX.

As this can be triggered for example by adding a library with text relocations to the ones loaded by the executable, PaX offers the option `CONFIG_PAX_NOELFRELOCS` in order to avoid them. This option is enabled like this:

**Menuconfig Options**

If you are using the gentoo hardened toolchain, typically compiling your programs will create PIC ELF libraries that do not contain text relocations. However, certain libraries still contain text relocations for various reasons (often ones that contain assembly that is handled incorrectly). This can be a security vulnerability as an attacker can use non-PIC libraries to execute his shellcode. Non-PIC libraries are also bad for memory consumption as they defeat the code sharing purpose of shared libraries.

To disable this error and allow your program to run, you must sacrifice security and allow runtime code generation for that program. The PaX feature that allows you to do that is called MPROTECT. You must disable MPROTECT on whatever executable is using the non-PIC library.

To check your system for textrels, you can use the program `scanelf` from [app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils). For information on how to use the `pax-utils` package please consult the [Gentoo PaX Utilities Guide](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Utilities).

### Ever since I started using PaX I can't get Java/JIT code working, why?

As part of its design, the Java virtual machine creates a considerable amount of code at runtime which does not make PaX happy. Although, with current versions of portage and java, portage will mark the binaries automatically, you still need to enable PaX marking so PaX can do an exception with them and have paxctl installed so the markings can be applied to the binaries (an reemerge them so they are applied).

This of course can't be applied to all packages linking with libraries with JIT code, so if it doesn't, there are two ways to correct this problem:

**Enable the marking on your kernel**

`root #``emerge --ask paxctl`
When you already have `paxctl` emerged you can do:

`root #``paxctl -pemrxs /path/to/binary`
This option will slightly modify the ELF header in order to correctly set the PAX flags on the binaries.

The other way is using your security implementation to do this using the kernel hooks.

### Can I disable PaX features at boot?

Although this is not advised except when used to rescue the system or for debugging purposes, it is possible to change a few of PaX behaviours on boot via the kernel command line.

Passing `pax_nouderef` in the kernel cmdline will disable uderef which can cause problems on certain virtualization environments and cause some bugs (at times) at the expense leaving the kernel unprotected against unwanted userspace dereferences.

Passing `pax_softmode=1` in the kernel cmdline will enable the softmode which can be useful when booting a not prepared system with a PaX kernel. In soft mode PaX will disable most features by default unless told otherwise via the markings. In a similar way, `pax_softmode=0` will disable the softmode if it was enabled in the config.

## Grsecurity questions

### Where is the homepage for Grsecurity?

That is the [homepage for Grsecurity](https://www.grsecurity.net).

### What Gentoo documentation exists about Grsecurity?

The most current documentation for Grsecurity is a [Grsecurity2 quickstart guide](https://wiki.gentoo.org/wiki/Project:Hardened/Grsecurity2_Quickstart).

### How does TPE work?

We have written a [document](https://wiki.gentoo.org/wiki/Project:Hardened/Grsecurity_Trusted_Path_Execution) with some information on how TPE works in the different settings.

## SELinux questions

There is a [SELinux specific FAQ](https://wiki.gentoo.org/wiki/SELinux/FAQ).
