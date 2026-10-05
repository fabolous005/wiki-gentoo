<!-- source: https://wiki.gentoo.org/wiki/Hardened/Toolchain | group: Gentoo Wiki (Main) | wiki-title: Hardened/Toolchain -->
---
title: Hardened/Toolchain
url: https://wiki.gentoo.org/wiki/Hardened/Toolchain
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-02"
fingerprint: "86cb5a5b2696bdd0"
license: CC BY-SA 4.0
---

# Hardened/Toolchain

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Technical description of, and rationale for, the Gentoo Hardened Toolchain modifications.

## Introduction to the Gentoo Hardened toolchain

### TODO

- Binutils modifications for PaX
- Comment on relationship to SELinux

### Overview

The Gentoo Hardened project introduces a number of changes to the default behaviour of the toolchain (gcc, binutils, glibc/musl) intended to improve security. It supports other initiatives taken by the hardened project, like SELinux. This document describes each of the modifications made to the toolchain, showing what they achieve and how they relate to the Gentoo hardened strategy.

## Changes

| Technique | Vanilla / normal Gentoo | Gentoo Hardened | 
|---|---|---|
| Stack Smashing Protection (SSP) | `-fstack-protector-strong` with reduced buffer threshold needed to apply) | `-fstack-protector-strong` with reduced buffer threshold needed to apply) | 
| FORTIFY\_SOURCE | `-D_FORTIFY_SOURCE=2`) (except on musl: [bug #546692](https://bugs.gentoo.org/show_bug.cgi?id=546692)) | `-D_FORTIFY_SOURCE=3`) (except on musl: [bug #546692](https://bugs.gentoo.org/show_bug.cgi?id=546692)) | 
| libstdc++ assertions ( `-D_GLIBCXX_ASSERTIONS`) |  |  | 
| Position Independent Executables (PIE) |  |  | 
| Stack clash protection |  | `-fstack-clash-protection` is enabled by default | 
| RELRO |  |  | 
| Control Flow Technology (CET) | `-fcf-protection=full` is enabled by default with USE="cet" (except on musl) | `-fcf-protection=full` is enabled by default with USE="cet" (except on musl) | 

### Default addition of the Stack Smashing Protector (SSP)

Normally, the compiler must be explicitly directed to switch on the stack protection via compiler options. Gentoo Hardened's GCC switches on the stack protector by default unless explicitly requested not to. The [SSP](https://wiki.gentoo.org/wiki/Hardened/Toolchain#The_Stack_Smashing_Protector_.28SSP.29) section describes the toolchain modifications to make this happen, and issues that may arise.

### Automatic generation of Position Independent Executables (PIEs)

Gentoo Hardened's GCC automatically builds PIEs when building application code, unless explicitly requested not to (with a few built-in exceptions for cases where it is undesirable). The [PIE](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Position_Independent_Executables_.28PIEs.29) section describes the toolchain modifications to make this happen, and issues that may arise.

### Default to marking read-only, sections that can be so marked after the loader is finished (RELRO)

Gentoo Hardened's binutils automatically sets the linker to set RELRO, unless explicitly requested not to. The [RELRO](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Mark_Read-Only_Appropriate_Sections_.28RELRO.29) section describes the toolchain modifications to make this happen.

### Default full binding at load-time (BIND\_NOW)

The hardened toolchain changes this behaviour so that by default it will set the "BIND\_NOW" flag, which causes the loader to sort out all of these links before starting execution. It improves the effectiveness of RELRO.

Gentoo Hardened's GCC automatically sets the linker to set the BIND\_NOW flag, unless explicitly requested not to. The [BIND\_NOW](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Binding_policy_NOW) section describes the toolchain modifications to make this happen.

### Default Control Flow Technology (CET)

GCC when built with USE="cet hardened" will build with `-fcf-protection=full` by default, see [bug #822036](https://bugs.gentoo.org/show_bug.cgi?id=822036).

## The Stack Smashing Protector (SSP)

The Stack Smashing Protector attempts to protect against stack buffer overflows.

SSP was first developed by Dr Hiroaki Etoh at IBM for the 3.x series of GCC (originally under the name ProPolice). It was later re-developed in a different way for the 4.x series by Red Hat.

It causes the compiler to insert a check for stack buffer overflows before function returns. If an attempt is made to exploit a previously unfixed (and probably undiscovered) error that exposes a buffer overflow vulnerability, the application will be killed immediately. This reduces any potential exploit to a denial-of-service.

### Rationale for enabling the stack smashing protector globally

The stack smashing protector arranges the code so that a stack overflow is very likely to be detected by the application, which then aborts. This means that an attacker who tries to exploit such a stack overflow error can cause the application to die, but cannot exploit the error to execute code. So the threat is reduced from a privilege elevation to at most a denial of service. Obviously, any such errors in the code should be fixed - SSP is not an excuse to avoid fixing them - however it is extremely difficult to be confident no such errors remain, even after thorough code review and testing. SSP remains as a safety-net for unfixed stack overflow errors.

This is an important part of the overall Hardened Toolchain/PaX/Grsecurity strategy. PaX prevents stack overflows from being executable by making the stack non-executable so an attacker cannot just put his shellcode into a buffer that overflows - however it does not prevent attacks that alter data that affect the program flow; in particular attacks that modify return addresses on the stack.

### Toolchain modifications for default SSP

#### Historical

In Gentoo, we add stack smashing protection patches to the GCC 3.x series compilers and the C library (glibc 2.3.x and uclibc). As such, this is the most invasive action taken to harden the toolchain. From GCC 4.1 and glibc 2.4 onwards, a different implementation of SSP is provided by the upstream GNU toolchain; glibc 2.4 is also patched by Gentoo to support the GCC 3.x implementation of SSP so binaries built with either SSP implementation will work.

The patches to GCC 3.x are not trivial, so a detailed explanation here is not suitable, even if we could write one. The modifications to the C library are simpler; they provide the canary value which is randomised separately for every process (and separately for every thread in the GCC 4.x / glibc 2.4 implementation), and the handler function called when the canary is checked and found to be corrupt. The handler function reports an error and shuts down the process; this has to be done very carefully to ensure the handler itself cannot be exploited.

#### Modern-day

Gentoo Hardened only has to apply a patch to reduce the buffer size threshold needed for `-fstack-protector-strong`. The rest is upstreamed and is achieved in Gentoo, and other distributions, by passing an argument to GCC when building it from source within packaging.

### Issues arising from default SSP

#### Historical

The SSP implementation in gcc 3.x was imperfect and caused problems. In particular, C++ code was built incorrectly when SSP was enabled, although the exact details weren't clear.

The SSP implementation in gcc 4.x is completely different, even so far as changing the semantics of the compiler switches.

#### Modern-day

None. This is the standard in most modern Linux distributions.

The efforts on SSP led to gcc adopting a ./configure build-time option to allow default-enabling SSP which empowered other distributions to make the same change!

## Position Independent Executables (PIEs)

Standard executables have a fixed base address, and they must be loaded to this address otherwise they will not execute correctly. Position Independent Executables can be loaded anywhere in memory much like shared libraries, allowing [PaX](https://wiki.gentoo.org/wiki/Hardened/PaX_Quickstart) 's Address Space Layout Randomisation (ASLR) to take effect. This is achieved by building the code to be position-independent, and linking them as ELF shared objects.

In 2003, Hardened Gentoo introduced an approach referred to as `-y etdyn` which consisted of building all code with `-fPIC`, and modifying the link stage to provide an ET\_DYN executable using a modified PIC version of crt1.o, and setting the interp header to cause the executable to be loaded by the loader from glibc. ET\_DYN versions of the crt1.o object were created for x86, parisc, ppc, and sparc.

Further work was undertaken by Red Hat who implemented a `-pie` switch for the linker, and `-fPIE` to be used when compiling objects for linking into a Position Independent Executable. Building an object with `-fPIE` is slightly different from `-fPIC`; in particular not all symbols are vectored through the GOT which means pre-loaded shared libraries do not override these symbols in the executable, which is also the case for ET\_EXEC executables.

### Rationale for building Position Indepedent Executables (PIEs)

The reason for building applications as position-independent is to allow the application to be loaded at a random address; normally the kernel loads all executables to the same fixed address. Randomising this address makes it harder for an attacker to exploit the executable, since it is harder to know where the code (and heap) reside.

This is most effective when running a PaX kernel with Address Space Layout Randomisation (ASLR), which increases the randomness of the various parts of a process significantly. It is also necessary to enable the Grsecurity option to hide the location information in the /proc filesystem - otherwise the attacker can just look there to find the addresses needed!

A note on prelink: prelinking sets hints for the address at which an ELF file will be loaded. These hints, if followed, would make ASLR ineffective. The PaX kernel causes these hints to be ignored, so prelinking does nothing useful for a Gentoo Hardened system. Since there is no point using prelink, just don't use it.

Not using prelink also means you don't have to worry about knock-on effects of prelink. Since prelink modifies the ELF files whenever it prelink is run, these changes need to be propogated to other systems that depend on them. For example host-based Intrusion Detection Systems (IDS) watch for changes to executables and libraries and need to be informed of the changes prelink makes - if prelink is not used then maintenance of such systems is simplified.

There are some technologies on the way which reduce the need for prelink. These include:

- Symbol visibility support, which when used properly, reduces dramatically the number of symbols to resolve and hence the amount of time taken to resolve them
- Hash tables, which will be generated by the linker and included as a extra section in the ELF file, which make looking up symbols to resolve them nearly free.
- Direct binding, which simplifies the search that the loader has to perform by incorporating information in each library detailing exactly where the symbol to be resolved is located.

See [Issues arising from default NOW](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Issues_arising_from_default_NOW) for more.

### Toolchain modifications for automatic PIEs

As the support for PIEs in the upstream toolchain matured, Hardened Gentoo switched to PIE, dropping the earlier `-y et_dyn` support. PIEs have several advantages, not least of which is that upstream have taken on the burden of support in gcc, glibc and binutils.

Support for position-independent executables is provided by the standard GNU toolchain. For PIEs, GCC has different versions of some of the compiler support objects so that for example, instead of using crtbegin.o, crt1.o, and crtend.o, it uses crtbeginS.o, Scrt1.o, and crtendS.o respectively (the exact files vary according to the compiler target).

It also builds code in a similar fashion to library PIC code, although in the case of executables some symbols are not referenced via the Global Offset Table (GOT). The compiler obtains the list of compiler support objects via the "specs" file - see "info gcc" section 3 (Invoking GCC) subsection 15 (Spec Files). Building code for PIEs is achieved by adding `-fPIE` when compiling and `-fPIE -pie` when linking.

To change the default for building executables from absolute to position-independent, it is necessary to change the selection of support objects and to set the `-fPIE` and `-pie` options appropriately. The specs rules for this are `startfile`, `endfile`, `cc1`, and `link_command`. Exact details of the modifications vary according to the target; to illustrate here are the changes for x86:

This looks a lot more scary than it really is. All it says is that unless one or more of the listed options are specified, add `-fPIE` to the compilation. The kernel defines `__KERNEL__` on all its compilations, so the `-D__KERNEL__` check ensures that `-fPIE` is not added when building the kernel; the kernel is a static executable but as [Issues with PIEs](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Issues_with_PIEs) describes, this is not so easy to detect.

When building shared libraries, either `-fPIC` or `-fpic` should be specified, so this is used to prevent adding `-fPIE` when building shared libraries. The \<code.-fno-\* checks are to ensure that if a build explicitly requests no position-indepedent code, `-fPIE` is not added.

As with the cc1 rule, the addition causes `-pie` to be set for link commands unless one of the listed options are specified. Note that this replaces the `%{pie: -pie}` section in the original `link_command` rule.

In both rules, there is an additional condition `!nopie` - this provides a mechanism to stop the hardened compiler defaulting to PIE by adding `-nopie` to CFLAGS. This is what filter-flags does when asked to filter `-fPIE` and the compiler is hardened.

### Issues with PIEs

Ideally, when building a static executable, a shared library, or building without the standard gcc system files, PIE would not be automatically enabled. Unfortunately, the options `-static`, `-shared`, `-nostdlib`, and `-nostartfiles` are link options, so are usually only supplied to the link command of an executable and not to the compilation commands for individual objects.

In these cases, `-fPIE` will be added to the compilation commands when in fact it shouldn't be. This is an unavoidable limitation, since it is impossible to know from a compilation command for an object exactly what the link command is going to be. Indeed in some cases more than one link command can happen using the same objects. Such cases have to be handled by the relevant ebuild on a case-by-case basis. The `-nostdlib` and `-nostartfiles` options occur rarely. The `-shared` option is usually used when the compilation commands have been performed with `-fPIC`, so the majority of such cases are not a problem.

Where an application builds libraries without `-fPIC`, it is necessary to modify the build process to avoid `-fPIE` being added by the compiler. For packages that build only libraries, it is sufficient to do:

However if an ebuild creates both executables and libraries then more detailed modifications need to be made, to add the `-fno-PIE` to the compilation of objects destined for the libraries. Where an object is used for both a shared library and an executable, it is necessary to modify the build process significantly in order to obtain two objects, one built `-fPIC` and one built `-fPIE` for linking to the library and the executable respectively. Most packages that provide both a shared library and a static archive do so by using libtool which does the right thing automatically. Both of these approaches can be taken unconditionally; i.e. it is not necessary to make such changes conditional on the presence of the hardened compiler.

Occasionally application code will fail to compile with `-fPIE`. If this happens it is usually down to non-position-independent assembler code, and is most prevelant on X86 which has a limited general purpose register set. However this is rare in application code as normally application authors push most of their code into shared libraries, although it does happen. Most position-independent build problems occur in shared libraries which are not built position-independent - this is a problem regardless of Hardened, and is nothing to do with PIE; it is just that the issue is highlighted by the hardened compiler due to the automatic enabling of `-fPIE` when `-fPIC` is not specified as described above. See the [PIC fixing guide](https://wiki.gentoo.org/wiki/Hardened/Textrels_Guide) for information on how to fix this sort of problem.

Some applications have been reported to segfault when built as PIEs. Exactly why this occurs is unclear, but it is likely due to a compiler bug so later compiler versions may resolve such problems.

(Debugging PIEs used to require patches, as of around sys-devel/gdb-6.3-r5, which includes patches by Elena Zannoni at Red Hat. Such support now exists upstream in newer GCC versions.)

## Mark Read-Only Appropriate Sections (RELRO)

There are several sections that need to be writable by the loader before the application starts, but do not need to be writable by the application itself later. Setting relro instructs the linker to record which sections this applies to, and the loader will mark them read-only before passing or returning execution control to the application. Typical sections affected include `.ctors`, `.dtors`, `.jcr`, `.dynamic`, and `.got`, although the exact list varies according to arch. If [BIND\_NOW](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Binding_policy_NOW_.28BIND_NOW.29) is also set then on some arches all of the GOT (i.e. including the PLT) can be set read-only in this way, preventing various attack methods that involve overwriting it.

### Rationale for enabling RELRO globally

Various sections in an ELF file should be written only by the loader, and not by the application. However in normal circumstances these sections remain read/write throughout the life of the process, and there are attack methods that exploit this to affect program execution flow. Enabling RELRO causes the linker to include an extra header informing the loader which sections can be marked read-only after the loader has finished with them. It also affects the layout of sections slightly, to avoid having RELRO sections and read-write sections on the same memory page. Combining RELRO with BIND\_NOW allows also the PLT to be managed this way on some arches.

### Toolchain modifications for default RELRO

RELRO is a link option (`-z relro`) provided by the standard GNU toolchain. To switch it on by default, it is simply a matter of adding a small rule to the specs file for GCC as illustrated below:

So a new option is introduced, `norelro`, which can be used to prevent the hardened compiler from automatically switching on RELRO. However this is likely to be phased out, as newer binutils provide a `-z norelro` option which can be appended to LDFLAGS as `-Wl,z,norelro`.

### Issues arising from default RELRO

So far, the hardened project has found no issues with switching on RELRO by default. It can make the executable image a little bit bigger (on average by half a page i.e. 2K bytes) which may be of interest for targets with extremely limited memory.

## Binding policy NOW (BIND\_NOW)

To reduce the time between starting an application and actually being able to use it, most software is built with "lazy binding". This means that references to functions in shared libraries are resolved when they are actually used for the first time, rather than when the application is loaded.

### Rationale for enabling NOW binding globally

As described in the [RELRO](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Mark_Read-Only_Appropriate_Sections_.28RELRO.29) section, setting BIND\_NOW increases the effectiveness of setting RELRO, making attacks that involve overwriting data in the Global Offset Table (GOT) fail.

### Toolchain modifications for default NOW

NOW binding is a link option (`-z now`) provided by the standard GNU toolchain. To switch it on by default, it is simply a matter of adding a small rule to the specs file for GCC as illustrated below:

So a new option is introduced, `nonow`, which can be used to prevent the hardened compiler from automatically switching on NOW binding. However this is likely to be phased out, as newer binutils provide a `-z lazy` option which can be appended to LDFLAGS as `-Wl,z,lazy`.

### Issues arising from default NOW

NOW binding has several noticeable effects. The first is that initial loading time for applications increases, sometimes very noticeably, as the loader resolves all the references before passing execution to the loaded process.

One technology that could reduce this overhead significantly is the introduction of "Direct Binding", something that exists on Unix systems (e.g. Solaris) but does not exist in the GNU toolchain. Direct binding adds information to libraries when they are built, to tell the linker which library contains the symbol it is looking for. Normally the linker performs a search across all referenced libraries to find symbols, which adds significantly to the time taken to resolve them. However the implications of direct-binding are significant, and cannot be taken lightly. Michael Meeks at Novell is working on this; see our [bug #114008](https://bugs.gentoo.org/show_bug.cgi?id=114008) for our status on this.

Other technologies which should help are symbol visibility and hash tables in the ELF files. Both are technologies supported upstream, so when they appear they will be supported directly. With these two together, it is likely that there will not be much further benefit from direct binding and the complications that arise from direct binding may mean it won't be supported.

The second more serious effect is that applications that are not written to refer to shared libraries in the standard way can fail; the most obvious of these is X, which has modules with circular resolution dependencies amongst other unusual behaviour.

Another trick occasionally performed by applications is to decide between a number of shared libraries at run time, and use lazy binding to resolve references to the chosen library. Normally this would be done with `dlopen(3)` and friends, including obtaining symbol addresses via `dlsym(3)`, but it is possible to avoid using `dlsym(3)` and a plethora of pointers in the code by using lazy binding, although it's not pretty.

The following packages have issues with BIND\_NOW at the time of writing, and it has to be relaxed somewhat for them:

- X - some drivers consist of several libraries which are co-dependent, and the modules frequently have references to modules that they load.
- transcode - relies on lazy binding to be able to load its modules; the issues are similar to the X issues.

## References

### See also

- [PaX QuickStart](https://wiki.gentoo.org/wiki/Hardened/PaX_Quickstart)
- [Introduction to Position-Independent Code (PIC)](https://wiki.gentoo.org/wiki/Hardened/Introduction_to_Position_Independent_Code)
- [Guide to fixing non-PIC shared libraries](https://wiki.gentoo.org/wiki/Hardened/Textrels_Guide)

### External resources

- [How to Write Shared Libraries](http://people.redhat.com/drepper/dsohowto.pdf) by Ulrich Drepper (PDF)
