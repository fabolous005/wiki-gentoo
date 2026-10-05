<!-- source: https://wiki.gentoo.org/wiki/Hardened/PaX_Quickstart | group: Gentoo Wiki (Main) | wiki-title: Hardened/PaX Quickstart -->
---
title: Hardened/PaX Quickstart
url: https://wiki.gentoo.org/wiki/Hardened/PaX_Quickstart
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-07"
fingerprint: ffb2197beda29f28
license: CC BY-SA 4.0
---

# Hardened/PaX Quickstart

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


The [Hardened Gentoo project](https://wiki.gentoo.org/wiki/Project:Hardened) focuses on adding features to a Gentoo system that help resist security compromises. The approach is not always systematic, and many features that enhances security have been added to the project. Some features complement one another and some are mutually exclusive.

This document focuses on **PaX** which adds security enhancement to the area between the kernel and user land.

## What is PaX?

PaX is a patch to the Linux kernel that provides hardening in three ways:

1. Judicious enforcement of non-executable memory
2. Address Space Layout Randomization (ASLR)
3. Miscellaneous hardening on stack- and memory handling

These hardening measures are explained in more detail in the following sections.

### Judicious enforcement of non-executable memory

Judicious enforcement of non-executable memory prevents a common form of attack where executable code is inserted into the address space of a process by an attacker and then triggered, thus hijacking the process and possibly escalating privileges. The typical vector for the insertion is via user provided data that finds its way into executable memory. By ensuring that "data" only lives in memory which is non-executable, and that only "text" is found in memory which is executable, PaX preemptively protects against this class of attacks.

Try running the following with PaX's MPROTECT enforced and then not enforced to see this feature in action:

**`mmap-rwx.c`**

**Violate MPROTECT with RWX mmap**

```
/*
 * Contrast compiling with:
 *   gcc -UBAD -o mmap-rw mmap-rwx.c
 *   gcc -DBAD -o mmap-rwx mmap-rwx.c
 */
  
#include <stdio.h>
#include <stdlib.h>
#include <sys/mman.h>
#include <errno.h>
#include <string.h>
  
int main() {
	size_t *m;
  
#ifdef BAD
	m = mmap( NULL, 1024, PROT_READ | PROT_WRITE | PROT_EXEC, MAP_PRIVATE | MAP_ANONYMOUS, -1, 0 );
#else
	m = mmap( NULL, 4096, PROT_READ | PROT_WRITE, MAP_PRIVATE | MAP_ANONYMOUS, -1, 0 );
#endif
  
	if( m == MAP_FAILED )
		printf("mmap failed: %s\n", strerror(errno));
	else
		printf("mmap succeeded: %p\n", m);
  	
        return 0;
}
```
Similarly, try running the following with EMULTRAMP both enabled and disabled. This convoluted code forces gcc to set up a trampoline for the nested function `f2()`. The trampoline is executable code which lives on the stack and could allow the user to inject their own code via the variable `i`.

**`trampoline.c`**

**Force gcc to generate a trampoline**

```
/*
 * Contrast compiling with:
 *   gcc -DTRAMPOLINE -o trampoline trampoline.c
 *   gcc -UTRAMPOLINE -o trampoline trampoline.c
 *
 */
  
#include <stdio.h>
  
typedef void (*fptr)(void) ;
  
void f0(fptr f) {
        (*f)();
}
  
void f1() {
	int i ;
	printf("Enter an integer: ");
	scanf("%d", &i);
  	
        void f2() 	{
		printf("%d: Bouncey bouncey bounce!\n", i);  
        }
  
#ifdef TRAMPOLINE
	f0(f2);
#endif
}
  
int main () {
	f1() ;
	return 0;
}
```
### Address Space Layout Randomization (ASLR)

With Address Space Layout Randomization (ASLR), a randomization of the memory map of a process (as reported, for example, by `pmap`) is provided and thus makes it harder for an attacker to find the exploitable code within that space. Each time a process is spawned from a particular ELF executable, its memory map is different. Thus exploitable code which may live at 0x00007fff5f281000 for one running instance of an executable may find itself at 0x00007f4246b5b000 for another. While the vanilla kernel does provide some ASLR, a PaX patched kernel increases the randomization. Furthermore, when an application is built as a Position Independent Executable (PIE), even the base address is randomized. Try repeatedly running the following on both a vanilla and PaX kernel, with and without PIE:

**`aslr-test.c`**

**Randomize base addess for PIE**

```
/*
 * Contrast compiling with:
 *   gcc -o aslr-test-withpie -fPIC -pie aslr-test.c
 *   gcc -o aslr-test-without -fno-PIC -nopie aslr-test.c
 *
 */
  
#include <stdio.h>
  
void doit() {
        ;
        return ;
}
  
int main() {
        printf("main @ %p\n", main);
        printf("doit @ %p\n", doit);
        return 0;
}
```
For more information on PIE, see our documentation on the [Hardened Toolchain](https://wiki.gentoo.org/wiki/Hardened/Toolchain).

### Miscellaneous hardening

Finally, the PaX patches provide some miscellaneous hardening: erasing the stack frame when returning from a system call, refusing to dereference user-land pointers in some contexts, detecting overflows of certain reference counters, correcting overflows of some integer counters, enforcing the size on copies between kernel and user land, and providing extra entropy.

More information on PaX can be found on its [official homepage](https://pax.grsecurity.net).

## Understanding PaX

The first step in working with PaX is to configure and boot a PaX patched kernel. Depending on whether or not one has configured PaX for SOFTMODE or non-SOFTMODE, the kernel will automatically start enforcing memory restrictions and address space randomization on all running processes.

- With **SOFTMODE** enabled, PaX protection will not be enforced by default for those features which can be turned on or off at runtime, so this is the "permit by default" mode. In SOFTMODE the user must explicitly mark executables to enforce PaX protections.
- Without **SOFTMODE** (the **non-SOFTMODE** approach), PaX protections are immediately activate ("forbid by default"). The user must explicitly mark binaries to relax PaX protections selectively.

Ideally it should not be necessary to do anything else; however, for problematic executables in non-SOFTMODE, a second step is required: relax certain PaX restrictions on a per ELF object basis. This is done by tweaking the PaX flags which are read by the kernel when the ELF is loaded into memory and execution begins. This second step is usually straight forward except when the ELF object that requires the relaxation is a library. In that case, the library's flags have to be back ported to the executable that links against it. When PaX enforces or relaxes a feature, it does so on the basis of the executable's flags, not those of the libraries it links against. Both steps will be discussed in detail below, but first an overview of PaX's features will be presented.

### Enforce non-executable pages

The first important feature of PaX is its ability to enforce non-executable pages of memory. The following kernel features are used to configure this behavior:

| `PAX_NOEXEC` | This option enables the protection of allocated pages of memory as non-executable if they are not part of the text segment of the running process. It is needed for `PAGEEXEC`, `SEGMEXEC`, and `KERNEXEC`. | 
| `PAGEEXEC` | The kernel will protect non-executable pages based on the paging feature of the CPU. This is sometimes called "marking pages with the *NX* bit" in other OSes. This feature can be controlled on a per ELF object basis by the PaX `-P` and `-p` flags. | 
| `SEGMEXEC` | This is like `PAGEEXEC`, but based on the segmentation feature of the CPU and it is controlled by the PaX `-S` and `-s` flags. Note that `SEGMEXEC` is only available on CPUs that support memory segmentation, namely **x86**. | 
| `EMUTRAMP` | The kernel will emulate trampolines (snippets of executable code written on the fly) for processes that need them, e.g. nested functions in C and some JIT compilers. Since trampolines try to execute code written by the process itself to memory marked as non-executable by `PAGEEXEC` or `SEGMEXEC`, the PaX kernel would kill any process that tries to make use of one. `EMUTRAMP` allows these processes to run without having to fully disable enforcement of non-executable memory. This feature can be controlled on a per ELF object basis by PaX `-E` and `-e` flag. | 
| `MPROTECT` | The kernel will prevent the introduction of new executable pages into the running process by various techniques: it will forbid the changing of the executable status of pages, or the creation of anonymous RWX mappings, or making RELRO data pages as writable again. It is controlled on a per ELF object basis by the PaX `-M` and `-m` flag. | 
| `KERNEXEC` | This is the kernel land equivalent of `PAGEEXEC` and `MPROTECT`. It cannot be disabled during while the kernel is running. | 

### Enhanced Address Space Layout Randomization (ASLR)

The following features trigger the ASLR aspects of PaX:

| `PAX_ASLR` | The kernel will expand the number of randomized bits for the various section of the address space. This option is needed for `RANDMMAP`, `RANDKSTACK`, and `RANDUSTACK`. | 
| `RANDMMAP` | The kernel will use a randomized base address for `mmap()` requests that do not specify one via the `MAP_FIXED` variable. It is controlled by the PaX `-R` and `-r` flags. | 
| `RANDKSTACK` | The kernel will randomize every task's kernel stack on all system calls. It cannot be disabled while the kernel is running. | 
| `RANDUSTACK` | The kernel will randomize every task's userland stack. This feature can be controlled on a per ELF binary basis by the PaX `-R` and `-r` flags. | 

### Miscellaneous Memory Protection

None of the following features can be disabled while the kernel is running:

| `STACKLEAK` | The kernel will erase its stack before it returns from a system call. This feature cannot be disabled while the kernel is running. | 
| `UDEREF` | The kernel will not de-reference userland pointers in contexts where it expects only kernel pointers. This feature cannot be disabled while the kernel is running. | 
| `REFCOUNT` | The kernel will detect and prevent overflowing various (but not all) kinds of object reference counters. | 
| `USERCOPY` | The kernel will enforce the size of heap objects when they are copied in either direction between the kernel and userland. | 
| `SIZE_OVERFLOW` | The kernel recomputes expressions of function arguments marked by a size\_overflow attribute with double integer precision. | 
| `LATENT_ENTROPY` | The kernel will use early boot code to generate extra entropy, which is especially useful on embedded systems. | 

As stated above, some PaX features can be enforced (in the case of SOFTMODE) or relaxed (in the case of non-SOFTMODE) on a "per ELF object" basis. These are `PAGEEXEC`, `EMULTRAP`, `MPROTECT`, `RANDMMAP`, and `SEGMEXEC` and these are respectively controlled by the following flags: `-P`, `-E`, `-M`, `-R`, `-S` and `-p`, `-e`, `-m`, `-r`, `-s`. The upper case means "enforce" in SOFTMODE and the lower case means "relax" in non-SOFTMODE. A third possibility is that neither the enforce or relax flags are set, in which case the kernel simply uses the default for that particular protection. If neither `-P` nor `-p` is set on an ELF object, then the kernel will not enforce `PAGEEXEC` in SOFTMODE and will enforce it in non-SOFTMODE.

## Marking for PaX

Currently the PaX patches support three ways of doing PaX markings: `EI_PAX`, `PT_PAX`, and `XATTR_PAX`.

`EI_PAX` places the PaX flags in bytes 14 and 15 of the e\_ident field of an ELF objects's header. But this is broken in recent versions of glibc and is no longer supported. (See [bug #365825](https://bugs.gentoo.org/show_bug.cgi?id=365825) for more details.)

`PT_PAX` places the flags in an ELF objects' program header called `PAX_FLAGS`. This has the advantage that the flags are in the body of the object and will always be carried with it when copied; however it has the disadvantage that the object must have the `PAX_FLAGS` program header to work. Most Linux distributions don't build their executables and libraries with this program header, and adding it is problematic. There may not be enough space in the ELF object to add it or converting a GNU\_STACK program header, which is not used by the PaX kernel, might later cause problems under other kernels if the object is exported. In the worst case, changing the ELF binary will break self-checking executables which detect that they have been "tampered" with. (See [bug #100507](https://bugs.gentoo.org/show_bug.cgi?id=100507) for more details.)
Is deprecated in the 17.0 profile so use `XATTR_PAX`.

`PT_PAX` is not supported in the 17.0 profile, the preferred approach is to use `XATTR_PAX` which places the PaX flags in a file system's extended attributes. This has the advantage that it does not modify the ELF object in any way, but the disadvantage that the filesystems which house these objects, and the utilities used to copy, move and archive, them, must support xattrs. In the case of Gentoo and Portage, this means that tmpfs must support the user.pax.\* xattr namespace in which the PaX flags are placed, not just the security.\* and trusted.\* namespaces.

One final caveat about the two supported methods of doing PaX markings: the PaX kernel allows both `PT_PAX` and `XATTR_PAX` to be enabled, however if both features have been enabled PaX will not impose the markings unless the same flags are found in *both* locations. For this reason, the Hardened Project not recommend enabling both features at the same time, even for migration which is described below.

## Building a PaX kernel

The hardened-sources come with the Grsecurity patches ([http://grsecurity.net/](https://grsecurity.net/)), which bundle the PaX patches. If only the PaX patches are desired they can be obtained in isolation from [http://www.grsecurity.net/\~paxguy1/](https://www.grsecurity.net/~paxguy1/). Those interested in learning more about Grsecurity hardening in general should read the [Grsecurity Quickstart](https://wiki.gentoo.org/wiki/Hardened/Grsecurity2_Quickstart) article.

`root #``emerge --ask sys-kernel/hardened-sources`
Emerging a [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources) kernel places the kernel source tree in /usr/src/, but it does not configure the kernel features themselves. It is now up to each reader to make sure PaX is configured so that it enforces or relaxes the necessary items. Below recommended configurations are suggested, but readers feel free to deviate. The previous section should have given you some idea as to what each of the options provide so you can make intelligent choices. We recommend starting off with as much hardening as possible and relaxing only when there is no other workaround.

As stated, the PaX patches are bundled with Grsecurity, so the PaX configuration options are found under that menu:

**Location of the PaX configuration options**

You also have the option of selecting one of Grsecurity's preconfigured profiles.

**Location of the Grsecurity preconfigured profiles**

These will give you a meaningful starting point configuration for PaX.

If configuring for only `XATTR_PAX`, the following should be sufficient.

**Kernel configuration for XATTR\_PAX**

Since the PaX flags will now live on the extended attributions of your filesystems, you would need to enable xattr on those filesystems, but the PaX team has already set up a dependency. For example, [ext4](https://wiki.gentoo.org/wiki/Ext4) has:

**Automatic selection of EXT4\_FS\_XATTR by XATTR\_PAX\_FLAGS**

Here `Ext4 extended attributes` was automatically selected by `PAX_XATTR_PAX_FLAGS [=y] && GRKERNSEC [=y] && PAX [=y] && EXT4_FS [=y]`. Nonetheless, in case you are using some exotic filesystem which doesn't have this selection dependency, you may want to check and then file a bug report to have your filesystem better supported with respect to PaX.

Please note that these are recommended settings and you can deviate from them, but be careful when doing so. On a play box, you can and should try new features to see what they might do or break.  But on a production box, do not turn on or off some feature without knowing what you are doing.  This is particularly true for EMUTRAMP (Emulate trampolines) which is turned on by default, and turning it off can break python and therefore your system! The reason is that python and libffi write code on the fly, either by RWX mmap-ings which PaX's MPROTECT will kill, or by falling back on trying to write little code snippets to files which other features in Grsecurity will kill, like Trused Path Execution. (See [bug #484472](https://bugs.gentoo.org/show_bug.cgi?id=484472).) In Gentoo, we've opted to patch python and libffi to use trampolines that can be safely handled by emulation in a PaX kernel. (See [bug #329499](https://bugs.gentoo.org/show_bug.cgi?id=329499).) We then PaX mark the python executable with `-E` to enable emulation of trampolines.  Without EMUTRAMP on in the kernel, python will fail in trying to generate its RWX mmap-ings and die every time.  (As a *temporary* workaround if you get into this situation, you can disable MPROTECT on python with `paxctl-ng -m` and then restore the original markings once you have booted back into a PaX kernel with EMUTRAMP.)

If you can't use XATTR\_PAX flags over PT\_PAX. In that case, all of the above can remain in place, but we would change the PaX Control configuration:

**Kernel configuration with PT\_PAX flags**

If you decide on PaX marking method, you should adjust `PAX_MARKINGS` variable in the system's /etc/portage/make.conf file with either `XT` (for extended attributes) or `PT` (for program header marking). Both (`XT PT`) can be set if desired but you should only use one of them. Default is `XT` in the 17.0 profile.

## Controlling PaX

As we mentioned above, there are five PaX protections that can be enforced (in SOFTMODE) or relaxed (in non-SOFTMODE) on a per ELF object basis: **PAGEEXEC**, **EMULTRAP**, **MPROTECT**, **RANDMMAP** and **SEGMEXEC**. The later, SEGMEXEC, is only available on **x86** CPUs which support segmentation, unlike paging which is supported on all CPUs, even **x86**. Since some programs break for one reason or another under full PaX enforcement, we are faced with the choice of either fixing the code to work with PaX or relaxing one or more of these protections. In practice, "fixing the code" may be very difficult and we resort to the latter. The general approach should be to try full enforcement, and if something breaks, use `dmesg` to obtain a report from the kernel regarding why and then relax that particular protection. Even this is not generally needed on a Gentoo system because the ebuilds should set the correct flags for you via the pax-util.eclass. If you find that you have to set your own flags, we would ask that you file a bug report.

Generally setting the PaX flags is straightforward, but the user should keep a few things in mind:

1. One can set either PT\_PAX and/or XATTR\_PAX flags on the ELF object independently of one another. Similarly, the kernel can be configured to read either, both or neither fields. It is up to you to make sure that you set the flags in the field being used by the kernel to get the desired results. For example, if you have PT\_PAX="Pe---" while XATTR\_PAX missing on the object, but the kernel is configured only to use XATTR\_PAX, you may not get the desired result!

1. The recommended approach is to mark both PT\_PAX and XATTR\_PAX fields identically on the objects whenever possible, and set the kernel to read only XATTR\_PAX. The "whenever possible" is where things get complicated and the two fields may not wind up containing the same flags. If the ELF does not contain a PAX\_FLAGS program header, PT\_PAX marking will fail. However, the absence of this program header will not affect XATTR\_PAX markings. If the ELF is busy (i.e. there is a running process making use of the ELF's text), then one can read the PT\_PAX flags but not set them. Again, this does not affect setting or getting XATTR\_PAX flags. On the other hand, if you are using any file systems which do not support extended attributes, then XATTR\_PAX marking will fail on those file systems while PT\_PAX marking is uneffected, except as already stated. This can be fairly subtle because copying a file from a file system with xattrs to one without, and then back again will drop the XATTR\_PAX flags. Or tarring with an older version of tar which does not preserve xattrs will again drop the flags.

1. The PaX flags are only enforced when a process is loaded from an ELF executable. This executable in turn usually links dynamically against shared objects in memory. Using `cat /proc/<pid>/status | grep PaX` gives you the resulting PaX enforcement on the running process with PID=\<pid>. But since the executable and shared objects can have different flags, the question arises, which ones are used to determine the final running PaX enforcements? The answer is the executable for reasons of control and security. If the libraries were to set the runtime PaX enforcement, then which of the libraries would "win" if an executable linked against many? And one overly relaxed library could relax the privileges on many executables that link against it. E.g. Relaxing all PaX protection on glibc would effectively turn PaX off on one's system. Nonetheless, it may be the code in the library itself that needs the relaxation of some PaX enforcement. In that case, one has to "back port" the flags from the library to the executable that uses it.

Below we describe the utilities provided on a Gentoo system for working PaX markings. There are several since as PaX evolved, new features were needed. Each one emphasizes a different need with respect to the above caveats.

### paxctl

This is the traditional upstream package for setting PaX flags. It is limited only in that it sets PT\_PAX only, not XATTR\_PAX. It is provided by emerging [sys-apps/paxctl](https://packages.gentoo.org/packages/sys-apps/paxctl). It does have one functionality that no other utility has: it can either create a PAX\_FLAGS program header or convert a GNU\_STACK to PAX\_FLAGS. Both of these are not recommended since they can break the ELF under certain circumstances. However, in the extreme case that you cannot use XATTR\_PAX (e.g. you can't use file systems that support extended attributes) and you are dealing with an ELF object that wasn't build with a PAX\_FLAGS program header, then these options are available to you via this utility.

Here is a synopsis of its usage:

`root #``paxctl -h`
PaX control v0.7
Copyright 2004,2005,2006,2007,2009,2010,2011,2012 PaX Team \<pageexec@freemail.hu>
  
usage: paxctl \<options> \<files>
  
options:
	-p: disable PAGEEXEC		-P: enable PAGEEXEC
	-e: disable EMUTRAMP		-E: enable EMUTRAMP
	-m: disable MPROTECT		-M: enable MPROTECT
	-r: disable RANDMMAP		-R: enable RANDMMAP
	-x: disable RANDEXEC		-X: enable RANDEXEC
	-s: disable SEGMEXEC		-S: enable SEGMEXEC
  
	-v: view flags			-z: restore default flags
	-q: suppress error messages	-Q: report flags in short format
	-c: convert PT\_GNU\_STACK into PT\_PAX\_FLAGS (see manpage!)
	-C: create PT\_PAX\_FLAGS (see manpage!)

Note that `paxctl` also reports on an older PaX protection called RANDEXEC. This is now deprecated - the randomization of the base address of a processes now simply part of ASLR on all executables build ET\_DYN rather than EX\_EXEC.

Here is `paxctl` in action:

`root #``paxctl -v /usr/bin/python3.2`
PaX control v0.7
Copyright 2004,2005,2006,2007,2009,2010,2011,2012 PaX Team \<pageexec@freemail.hu>
  
- PaX flags: -----m-x-e-- \[/usr/bin/python3.2\]
	MPROTECT is disabled
	RANDEXEC is disabled
	EMUTRAMP is disabled
  
# paxctl -P /usr/bin/python3.2 
# paxctl -v /usr/bin/python3.2 
PaX control v0.7
Copyright 2004,2005,2006,2007,2009,2010,2011,2012 PaX Team \<pageexec@freemail.hu>
  
- PaX flags: P----m-x-e-- \[/usr/bin/python3.2\]
	PAGEEXEC is enabled				\<--- Note: this added to the earlier flags, it didn't overwrite them.
	MPROTECT is disabled
	RANDEXEC is disabled
	EMUTRAMP is disabled

### getfattr setfattr

These are not PaX specific utilities but are general utilities to set a file's extended attributes. On Gentoo, they are provided by emerging [sys-apps/attr](https://packages.gentoo.org/packages/sys-apps/attr). Since XATTR\_PAX uses the user.\* namespace, specifically it uses "user.pax.flags", you can use set/getfattr to work with this field. However, keep in mind that setfattr and getfattr know nothing about PaX, so they will not perform any sanity checking of what you are putting into that field. Only if you set user.pax.flags to some meaningful combination of the chars PpEeMmRrSs will the kernel respect your choice, else it falls back on the default. The following listing gives examples.

The following is an example of their usage for XATTR\_PAX:

`root #``getfattr -n user.pax.flags /usr/bin/python3.2`
getfattr: Removing leading '/' from absolute path names
# file: usr/bin/python3.2
user.pax.flags="em"

`root #````
setfattr -n user.pax.flags -v P /usr/bin/python3.2
```
`root #``getfattr -n user.pax.flags /usr/bin/python3.2`
getfattr: Removing leading '/' from absolute path names
# file: usr/bin/python3.2
user.pax.flags="P"

`root #````
setfattr -n user.pax.flags -v "Hi Mom, wish you were here."  /usr/bin/python3.2
```
`root #``getfattr -n user.pax.flags /usr/bin/python3.2`
getfattr: Removing leading '/' from absolute path names
# file: usr/bin/python3.2
user.pax.flags="Hi Mom, wish you were here."

### paxctl-ng

paxctl-ng is the new swiss army knife of working with PT\_PAX an XATTR\_PAX markings. It can be built with support for just one or the other or both types of markings. When built with support for both, it can copy PT\_PAX to XATTRP\_PAX fields or vice versa, to make sure you have consistency. In sum, it can do everything paxctl and set/getfattr can do, except it will not try to create or convert a PAX\_FLAGS program header. This is discouraged and should only be use in the corner case mentioned above. Here is a synopsis of its usage:

`user $``paxctl-ng -h````
Package Name : elfix 0.7.1
Bug Reports  : http://bugs.gentoo.org/
Program Name : paxctl-ng
Description  : Get or set pax flags on an ELF object
  
Usage        : paxctl-ng -PpEeMmRrSsv ELF {{!}} -Zv ELF {{!}} -zv ELF
             : paxctl-ng -Cv ELF {{!}} -cv ELF {{!}} -dv ELF
             : paxctl-ng -Fv ELF {{!}} -fv ELF
             : paxctl-ng -Lv ELF {{!}} -lv ELF
             : paxctl-ng -v ELF {{!}} -h
  
Options      : -P enable PAGEEXEC	-p disable  PAGEEXEC
             : -E enable EMUTRAMP	-e disable  EMUTRAMP
             : -M enable MPROTECT	-m disable  MPROTECT
             : -R enable RANDMMAP	-r disable  RANDMMAP
             : -S enable SEGMEXEC	-s disable  SEGMEXEC
             : -Z all secure settings	-z all default settings
             :
             : -C create XATTR_PAX with most secure setting
             : -c create XATTR_PAX all default settings
             : -F copy PT_PAX to XATTR_PAX
             : -f copy XATTR_PAX to PT_PAX
             : -L set only PT_PAX flags
             : -l set only XATTR_PAX flags
             :
             : -v view the flags, along with any accompanying operation
             : -h print out this help
  
Note         :  If both enabling and disabling flags are set, the default - is used
```
The following is an example of paxctl-ng in action.

First view both PT\_PAX and XATTR\_PAX fields (`-v`):

`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX   : Pem--
	XATTR\_PAX: Pem--

Set default '-' for PAGEEXEC (-Pp) for XATTR\_FLAGS only (`-l`) and report the result (`-v`):

`root #``paxctl-ng -lPpv /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX   : Pem--
	XATTR\_PAX: -em--

Delete the XATTR\_PAX field altogether (`-d`) and report the result (`-v`):

`root #``paxctl-ng -dv /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX   : Pem--
	XATTR\_PAX: not found

Set "em" flags (`-em`) for XATTR\_FLAGS only (`-l`) and report the result (`-v`):

`root #``paxctl-ng -lemv /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX   : Pem--
	XATTR\_PAX: -em--

Copy the XATTR\_PAX to PT\_PAX flags, overwriting the latter (`-f`) and report the result (`-v`):

`root #``paxctl-ng -fv /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX   : -em--
	XATTR\_PAX: -em--

Silently delete the XATTR\_PAX field (`-d` but no -v):

`root #``paxctl-ng -d /usr/bin/python3.2`
View both PT\_PAX and XATTR\_PAX fields (`-v`):

`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX   : -em--
	XATTR\_PAX: not found

### The pax.so Python module and the pypaxctl Python frontend

We also provide bindings to python via a module, pax.so, which is installed by emerging [dev-python/pypax](https://packages.gentoo.org/packages/dev-python/pypax). This package is a dependency of [sys-apps/elfix](https://packages.gentoo.org/packages/sys-apps/elfix) since the both revdep-pax and migrate-pax import it. It can be compiled with either PT\_PAX and/or XATTR\_PAX support, like paxctl-ng, but it is not as featureful since its scope is limited to just the needs of `revdep-pax` and `migrate-pax`. This may change in the future if there is a need to better integrate PaX marking with portage which is written in python.

Currently pax.so publicly exports the following:

- **pax.setstrflags(str\_flags):**
- This function will set both PT\_PAX and XATTR\_PAX flags to the same value, whenever possible. The flags are specified as a string of the following chars: PpEeMmRrSs. If both the enable and disable flags are given for a particular protection, then the default '-' is used.

- **pax.setbinflags(bin\_flags):**
- This function is the same as pax.setstrflags but it takes the flags as a bitwise OR of the binary representation of the flags. The PT\_PAX field is stored as this way, while XATTR\_PAX is stored as a string of chars PpEeMmRrSs.

- **pax.getflags(elf):**
- This function returns the PaX flags as a tuple of both forms (str\_flags, bin\_flags), i.e., both as a string and its equivalent binary representation are returned. If both PT\_PAX and XATTR\_PAX are set, then the XATTR\_PAX flags will override the PT\_PAX flags.

- **pax.deletextpax(elf):**
- This function will delete the XATTR\_PAX field completely. It does not touch the PT\_PAX field, which cannot be deleted (easily!).

- **pax.PaxError:**
- This exception is thrown whenever getting or setting the flags fails, or when deleting the XATTR\_PAX field fails.

`pypaxctl` is a simple front end to pax.so which just gets and sets the PaX flags using the same logic. When getting the flags, if both PT\_PAX and XATTR\_PAX are present, the latter will override the former. When setting, it will set both fields whenever possible. Here it is in action:

Retrieve either PT\_PAX or XATTR\_PAX. The latter has priority if it exists. The canonical order is PEMRS.

`root #``pypaxctl -g /usr/bin/python3.2`
-em--

It turns out that XATTR\_PAX didn't exist, so we got PT\_PAX.

`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX: -em--
	XT\_PAX: not found

Set some XATTR\_PAX flags:

`root #``paxctl-ng -lPMEv /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX: -em--
	XT\_PAX: PEM--

Now its reporting XATTR\_PAX flags:

`root #``pypaxctl -g /usr/bin/python3.2`
PEM--

Set "me" on both PT\_PAX and XATTR\_PAX:

`root #``pypaxctl -s me /usr/bin/python3.2`
\# paxctl-ng -v /usr/bin/python3.2
/usr/bin/python3.2:
	PT\_PAX: -em--
	XT\_PAX: Pem--

But what if we want PAGEEXEC off?  Then set Pp for the default '`-`':

`root #````
pypaxctl -s Pp /usr/bin/python3.2
```
`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX: -em--
	XT\_PAX: -em--

If the logic of XATTR\_PAX taking precedence over PT\_PAX is not what you want, it can be compiled with just PT\_PAX xor XATTR\_PAX support. In this case, the other field is not ever touched, as demonstrated below:

`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX: -em--
	XT\_PAX: PEM--

`root #``pypaxctl -g /usr/bin/python3.2`
-em--

`root #````
pypaxctl -s p /usr/bin/python3.2
```
`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX: pem--
	XT\_PAX: PEM--

`root #````
pypaxctl -s PpEem /usr/bin/python3.2
```
`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX: --m--
	XT\_PAX: PEM--

### revdep-pax

This utility is used to map out the linkings between all the ELF objects on your system and their shared objects in both directions. It can then search for PaX flag markings that are mismatched between the shared objects than the executables; and optionally, allows the user to migrate the markings forwards or backwards on a per ELF object basis. Here is a synopsis of its usage:

`user $``revdep-pax -h````
Package Name : elfix
Bug Reports  : http://bugs.gentoo.org/
Program Name : revdep-pax
Description  : Get or set pax flags on an ELF object
  
Usage        : revdep-pax -f [-v]             print out all forward mappings for all system binaries
             : revdep-pax -r [-ve]            print out all reverse mappings for all system sonames
             : revdep-pax -b OBJECT  [-myv]   print all forward mappings only for OBJECT
             : revdep-pax -s SONAME  [-myve]  print all reverse mappings only for SONAME
             : revdep-pax -l LIBRARY [-myve]  print all reverse mappings only for LIBRARY file
             : revdep-pax [-h]                print out this help
             : -v                             verbose, otherwise just print mismatching objects
             : -e                             only print out executables in shell $PATH
             : -m                             don't just report, but mark the mismatching objects
             : -y                             assume "yes" to all prompts for marking (USE CAREFULLY!)
```
Here's `revdep-pax` in action. Since the out is long, we've replaced some of it with ellipses:

Report all mismatching forward linkings:

`root #``revdep-pax -f````
          .....
/usr/bin/python3.2 ( --m-- )
  
	libpython3.2.so.1.0	/usr/lib64/libpython3.2.so.1.0 ( -e--- )
	libpthread.so.0	/lib64/libpthread-2.16.so ( -e--- )
	libc.so.6	/lib64/libc-2.16.so ( -e--- )
	libdl.so.2	/lib64/libdl-2.16.so ( -e--- )
	libutil.so.1	/lib64/libutil-2.16.so ( -e--- )
	libm.so.6	/lib64/libm-2.16.so ( -e--- )
          .....
```
Forward port PaX flags for python3.2 only:

`root #``revdep-pax -m -b /usr/bin/python3.2`
/usr/bin/python3.2 (--m--)
  
	libpython3.2.so.1.0	/usr/lib64/libpython3.2.so.1.0 ( -e--- )
	libpthread.so.0	/lib64/libpthread-2.16.so ( -e--- )
	libc.so.6	/lib64/libc-2.16.so ( -e--- )
	libdl.so.2	/lib64/libdl-2.16.so ( -e--- )
	libutil.so.1	/lib64/libutil-2.16.so ( -e--- )
	libm.so.6	/lib64/libm-2.16.so ( -e--- )
  
	Will mark libraries with --m--
  
	Set flags for /usr/lib64/libpython3.2.so.1.0 (y/n): n		\< --- You will be prompted unless you give -y.
	Set flags for /lib64/libpthread-2.16.so (y/n): n		\< --- We'll say 'n' to each for this demo.
	Set flags for /lib64/libc-2.16.so (y/n): n
	Set flags for /lib64/libdl-2.16.so (y/n): n
	Set flags for /lib64/libutil-2.16.so (y/n): n
	Set flags for /lib64/libm-2.16.so (y/n): n

Report all mismatching reverse linkings:

`root #``revdep-pax -r````
          .....
libpython3.2.so.1.0	/usr/lib64/libpython3.2.so.1.0 ( -e--- )	< --- The format is soname /path/to/its/elf_object (flags).
  
	/usr/bin/python3.2 ( --m-- )					< --- There is a mismatch here as expected
          .....										from the previous step.
```
Let's migrate from the library's flags to the executable using the soname (`-s`). We could also have used /usr/lib64/libpython3.2.so.1.0 with the `-l` flag.

`root #``revdep-pax -m -s libpython3.2.so.1.0`
libpython3.2.so.1.0	/usr/lib64/libpython3.2.so.1.0 (-e---)
  
	/usr/bin/python3.2 ( --m-- )
  
	Will mark binaries with -e---
  
	Set flags for /usr/bin/python3.2 (y/n): y			\< --- Confirm 'y' we want to do this.
  
		/usr/bin/python3.2 ( -em-- )

`root #``paxctl-ng -v /usr/bin/python3.2`
/usr/bin/python3.2:
	PT\_PAX: -em--
	XT\_PAX: -em--

### migrate-pax

At this point one can be easily 'fed up' dealing with both PT\_PAX and XATTR\_PAX fields and their relationship to the kernel's configuration, and you just want to drop the older PT\_PAX and get on with life! `migrate-pax` does only that: it will go through all ELF objects on the system and migrate the PT\_PAX field to XATTR\_PAX. For more details on how to migrate, see the article on [migrating PaX flags from PT\_PAX to XATTR\_PAX](https://wiki.gentoo.org/wiki/Hardened/PaX_flag_migration_from_PT_PAX_to_XATTR_PAX).

`user $``migrate-pax -h````
Package Name : elfix
Bug Reports  : http://bugs.gentoo.org/
Program Name : migrate
Description  : Migrate PT_PAX to XATTR_PAX Flags on all system ELF objects
  
Usage        : migrate -v        print out all system ELF objects
             : migrate -m [-v]   migrate flags on all system ELF objects
             : migrate -d [-v]   delete XATTR_PAX on all system ELF objects
             : migrate [-h]      print out this help
             : -v                be verbose when migrating
```
