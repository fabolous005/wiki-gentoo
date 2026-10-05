<!-- source: https://wiki.gentoo.org/wiki/Hardened/Gentoo_Hardened_and_Stack_Clash | group: Gentoo Wiki (Main) | wiki-title: Hardened/Gentoo Hardened and Stack Clash -->
---
title: Hardened/Gentoo Hardened and Stack Clash
url: https://wiki.gentoo.org/wiki/Hardened/Gentoo_Hardened_and_Stack_Clash
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-06-22"
fingerprint: "32cfde106c232384"
license: CC BY-SA 4.0
---

# Hardened/Gentoo Hardened and Stack Clash

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Executive summary

With Gentoo Hardened no ebuilds compiled with a hardened toolchain with [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) version 4.8 or higher should be affected by this issue as -fstack-check=specific is enabled by default. The only known exceptions are [media-video/vlc](https://packages.gentoo.org/packages/media-video/vlc) and (on **HPPA**) [dev-lang/tcl](https://packages.gentoo.org/packages/dev-lang/tcl) which disable this feature.

## Introduction

The Gentoo Hardened team has been made aware of a set of vulnerabilities known as Stack Clash.

These vulnerabilities involve the process stack pointer jumping onto other memory regions allowing the attacker to either modify the contents of the stack itself or the memory region on which the stack overflows. As a result, an attacker may be able to modify the data of affected programs or their execution flow and trigger the execution of arbitrary code.

The existence of such issues has been known since at least 2005 when Gael Delalleau presented an example of such an issue affecting mod\_php in apache.

## Requirements to perform the attack

- The attacker needs to be able to move the stack pointer onto an adjacent memory region.
- The attacker needs to ensure this happens without accessing the gap between the stack and the next region.
- The attacker needs to be able overwrite the area where the stack overlaps with the other section in a meaningful way. For example, overwriting the return address of a function to take control of the execution flow.

## Effect of Gentoo Hardened protections

As a way to make exploitation of such issues harder vanilla Linux kernels introduced a 4kiB gap between the stack and the other regions
called guard page, on PaX based kernels like [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources) the size of this gap defaults at 64kiB and can be modified using /proc/sys/vm/heap\_stack\_gap. As shown by Qualys' advisory, such gap isn't large enough for all allocations and can be jumped over in some cases.

It should be taken into account that this measure was never meant as a final solution as this problem can only be addressed correctly during code generation. Therefore, increasing the size of this gap will not deter all exploitation attempts as large enough allocations may still be able to jump over the gap. It will though affect the processes running on the system and limit the amount of virtual memory space available to them. Although this might not be a problem on 64 bit architectures where the virtual memory space is quite large, on 32 bit architectures this may still be problematic and should be taken into account before using a large heap\_stack\_gap value.

The Gentoo Hardened toolchain also makes use of [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc)'s Stack Checking feature using -fstack-check=specific since [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) 4.8. This makes gcc add stack probing code when large allocations are made ensuring that the guard page or the stack gap are accessed. This will then prevent the attack by killing the process.

Although -fstack-check=specific has some known bugs these doesn't seem to affect the specific packages pointed out by Qualys although they might allow exploitability of other yet unknown bugs. Also, since -fstack-check=specific may leave a small (less than 6) number of pages unchecked, use of [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources) is strongly recommended since it has a default gap size of 16 pages in x86 an amd64 arches.

To be fully effective, -fstack-check=specific requires that all the source files used by the process have been compiled making use of this feature. This is because gcc may optimize away some of the checks that are considered redundant.

An assesment over the Gentoo Portage tree hints that only two packages disable this feature. Some ebuilds for [media-video/vlc](https://packages.gentoo.org/packages/media-video/vlc) disable the flag to address [bug #499996](https://bugs.gentoo.org/show_bug.cgi?id=499996). On the other hand, some ebuilds for [dev-lang/tcl](https://packages.gentoo.org/packages/dev-lang/tcl) disables this feature on HPPA builds to address [bug #280934](https://bugs.gentoo.org/show_bug.cgi?id=280934).

It is possible to check the version of the compiler used on a specific ELF binary by checking the value of the .comment section as long as it was not stripped. For this the command readelf -p .comment can be used, for example to check /usr/bin/whoami run the following command:

`user $``readelf -p .comment /usr/bin/whoami`
String dump of section '.comment':
  \[     0\]  GCC: (Gentoo Hardened 4.9.4 p1.0, pie-0.6.4) 4.9.4

Users of the split-debug portage feature should instead check the debug file on /usr/lib/debug/. For example the prior example for /usr/bin/whoami would be:

`user $``readelf -p .comment /usr/lib/debug/usr/bin/whoami.debug`
String dump of section '.comment':
  \[     0\]  GCC: (Gentoo Hardened 4.9.4 p1.0, pie-0.6.4) 4.9.4

All versions of the toolchain make use of -fstack-protector-all which, although not preventing the issue, will make it harder for an attacker to take control of the execution flow of the program as depending on how the attack is performed the attacker may need to find out the value of the canary in the stack.

In this case caution is advised because the amount of ebuilds disabling SSP is much larger and, shall the frame of any function provided by the binaries generated using these ebuilds end up on the overflowed into memory section, the attacker will be able to overwrite their return pointer.

All of the binaries in Gentoo Hardened are also compiled as PIE binaries unless ebuilds or the upstream buildsystem disables it. This allows using ASLR which both vanilla and PaX kernels provide with the second being noticeably stronger. This will make it harder for an attacker to figure out a valid address on which to return.

Finally, [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources) allows enabling *Deter exploit bruteforcing*. This feature limits the number of attack attempts per second that can be made against SUID binaries making exploitation noticeably harder.

On [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources) the increased entropy provided by ASLR will require a large number of attack attempts before succeeding at taking control of the program reliably. This, along with the slowdown introduced by *Deter exploit bruteforcing*, will make the attack require a huge amount of time to succeed. Because of this, such attacks will be unfeasible in the majority of situations.

## Conclusion

With the exception of exploits able to make use of [media-video/vlc](https://packages.gentoo.org/packages/media-video/vlc) to jump over the thread gap or (on HPPA systems) [dev-lang/tcl](https://packages.gentoo.org/packages/dev-lang/tcl) users of Gentoo Hardened who have recompiled their userspaces using [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) 4.8 or higher are protected against stack overflow issues.

Users of prior versions of [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) have also partial protection against some of the exploits thanks to ASLR, PIE, *Deter exploit bruteforcing* and even SSP although they should reconsider rebuilding their userspace with a more modern version of the hardened toolchain.

## Further reading reading and sources

## Thanks

The Gentoo Hardened team would like to thank the PaX Team for its outstanding work and its support whilst writting this statement.

We'd also like to thank [Zorry](https://wiki.gentoo.org/wiki/User:Zorry) for his hard work introducing -fstack-check=specific on the toolchain.

The latest version of this statement can be found on [\[1\]](https://wiki.gentoo.org/wiki/Hardened/Gentoo_Hardened_and_Stack_Clash)
