<!-- source: https://wiki.gentoo.org/wiki/Hardened/GNU_stack_quickstart | group: Gentoo Wiki (Main) | wiki-title: Hardened/GNU stack quickstart -->
---
title: Hardened/GNU stack quickstart
url: https://wiki.gentoo.org/wiki/Hardened/GNU_stack_quickstart
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-05-26"
fingerprint: "8b0a5a314b33fb18"
license: CC BY-SA 4.0
---

# Hardened/GNU stack quickstart

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Handbook for proper GNU Stack management in ELF systems

## Introduction

With the rise of mainstream consumer machines with hardware stack protection (e.g. the [NX bit](https://en.wikipedia.org/wiki/NX_bit) on **amd64**), we developers have to be doubly sure that our packages build with the correct stack settings. Keep in mind that stack protection is an issue for all architectures, not just **x86** or **amd64**.

The purpose of this document is to help package maintainers fix their packages when they break. We will be focusing our attention on the GNU\_STACK ELF marking. ELF is simply a file format which all modern Linux distros use. An ELF can be an executable (e.g. /bin/ls) or a library (e.g. /lib/libncurses.so). GNU\_STACK is just an ELF program header which tells the system how to control the stack when the ELF is loaded into memory.

Before getting started, you should read through the Wikipedia entry on the [NX bit](https://en.wikipedia.org/wiki/NX_bit). You can skip it of course if you're already familiar with the concept of executable versus non-executable stacks.

## Causes of executable stack markings

ELF files end up with executable stack markings in one of three ways:

1. GCC generates code that uses executable stack
2. An object built from assembler source includes a marking indicating to the linker that it needs an executable stack (the GNU-stack note set for executable stack)
3. An object built from assembler source is missing the GNU-stack note; a very common occurrence especially for code expected to work on many platforms

GCC generates code to be executed on the stack when it implements a [trampoline for nested functions](http://gcc.gnu.org/onlinedocs/gccint/Trampolines.html). To remove the need for an executable stack in this case, it is necessary to rewrite the code another way. Sometimes this is relatively easy, other times not.

If an assembler source file includes a GNU-stack note that indicates it needs an executable stack, presumably this is by design. Again, in order to remove the need for an executable stack, the code probably needs to be rewritten.

If an assembler source contains no GNU-stack note, the system by default assumes that an executable stack may be required. However, usually if there's no GNU-stack note, this is simply because the author didn't include one, rather than the code actually needing an executable stack.

In the first two cases above, the executable stack marking is correct, and should only be removed by rewriting the code to eliminate the executable stack requirement. Such rewriting has to be considered on a case-by-case basis and is outside the scope of this document, at least for now. Here we focus on the third case, where the upstream author has not indicated whether the assembler object needs an executable stack; fixing this means adding the GNU-stack note to the source to indicate an executable stack is not necessary.

## Finding ELFs that ask for an executable stack

Before you can start fixing something, you have to make sure it's broken first, right? For this reason, we've developed a suite of tools named [PaX Utilities](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Utilities). If you are not familiar with these utilities, you should read the [PaX Utilities Guide](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Utilities) now. Gentoo users can simply do emerge [app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils). Non-Gentoo users should be able to find a copy of the source tarball in the distfiles on a [Gentoo Mirror](https://www.gentoo.org/downloads/mirrors/). Once you have the PaX Utilities setup on your system, we can start playing around with scanelf.

Let's see if the system has any ELFs that want an executable stack.

`user $``scanelf -lpqe`
RWX --- ---  /usr/lib/opengl/xorg-x11/lib/libGL.so.1.2
RWX --- ---  /usr/lib/libcrypto.so.0.9.7
RWX --- ---  /usr/lib/libmp.so.3.1.7
RWX --- ---  /usr/lib/libSDL-1.2.so.0.7.2
RWX --- ---  /usr/lib/libsmpeg-0.4.so.0.1.3
RWX --- ---  /usr/lib/libImlib2.so.1.2.0
RWX --- ---  /usr/lib/libOSMesa.so.4.0
RWX --- ---  /usr/lib/libxvidcore.so.4.1
RWX --- ---  /usr/lib/libgmp.so.3.3.3
RWX --- ---  /usr/bin/mencoder
RWX --- ---  /usr/bin/Xorg
RWX --- ---  /usr/bin/mplayer

We really only need to look at the first column (which corresponds to the ELF GNU\_STACK markings). Most of the time, if we fix that field, all the others fall into place. As we can see above, many files are marked with an executable stack (*RWX*). We want to make sure all files are marked with *RW-*. The large majority of the time this means the package was compiled incorrectly, so not much will have to be done with patching up the source code.

## What needs to be fixed

We now know what files need to be fixed, but what source files are causing this breakage? The only way to find this out is to compile the package and analyze the object files before they are combined into the final executable or library.

### Fixing smpeg

So we first have to compile smpeg before we can analyze it.

`user $````
ebuild /usr/portage/media-libs/smpeg/smpeg-0.4.4-r6.ebuild clean unpack compile
```
`user $``cd /var/tmp/portage/smpeg-0.4.4-r6/work/smpeg-0.4.4/`
Now we need to look at each object file and see if it has a *.note.GNU-stack* ELF section. Chances are, the object which is causing us trouble lacks this section completely. In that case, the compiler will assume that the ELF should not be restricted at all and mark it as *RWX*. The scanelf utility will display output slightly different when presented with an object that is missing the ELF section. The **!WX** below means that "Oh no, the GNU-stack is missing and write/execute permissions will be used by default!"

`user $``scanelf -qeR .`
!WX --- ---  ./video/mmxflags\_asm.o
!WX --- ---  ./video/mmxflags\_asm.lo
!WX --- ---  ./video/mmxidct\_asm.o
!WX --- ---  ./video/mmxidct\_asm.lo

Sure enough, these objects lack the *.note.GNU-stack* ELF section and they are linked into the final libsmpeg.so library. If we were to patch the source files video/mmxflags\_asm.S and video/mmxidct\_asm.S so that they contain *.note.GNU-stack*, everything would be peachy.

### Check objects by hand

For fun, lets see how we could use the more common readelf utility (which is part of the [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils) package).

This is what the output should look like, notice the .note.GNU-stack line

`user $``readelf -S plaympeg.o`
There are 12 section headers, starting at offset 0x256c:
  
Section Headers:
  \[Nr\] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  \[ 0\]                   NULL            00000000 000000 000000 00      0   0  0
  \[ 1\] .text             PROGBITS        00000000 000040 001ede 00  AX  0   0 16
  \[ 2\] .rel.text         REL             00000000 0030c0 000728 08     10   1  4
  \[ 3\] .data             PROGBITS        00000000 001f20 000000 00  WA  0   0  4
  \[ 4\] .bss              NOBITS          00000000 001f20 000000 00  WA  0   0  4
  \[ 5\] .rodata.str1.4    PROGBITS        00000000 001f20 0003db 01 AMS  0   0  4
  \[ 6\] .rodata.str1.1    PROGBITS        00000000 0022fb 0001c9 01 AMS  0   0  1
  \[ 7\] .note.GNU-stack   PROGBITS        00000000 0024c4 000000 00      0   0  1
  \[ 8\] .comment          PROGBITS        00000000 0024c4 00003e 00      0   0  1
  \[ 9\] .shstrtab         STRTAB          00000000 002502 000067 00      0   0  1
  \[10\] .symtab           SYMTAB          00000000 00274c 0005e0 10     11   9  4
  \[11\] .strtab           STRTAB          00000000 002d2c 000394 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings)
  I (info), L (link order), G (group), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)

Notice how there is no .note.GNU-stack section here

`user $``readelf -S video/mmxidct_asm.o`
There are 8 section headers, starting at offset 0x738:
  
Section Headers:
  \[Nr\] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  \[ 0\]                   NULL            00000000 000000 000000 00      0   0  0
  \[ 1\] .text             PROGBITS        00000000 000034 0005ee 00  AX  0   0  4
  \[ 2\] .rel.text         REL             00000000 000a4c 0000f0 08      6   1  4
  \[ 3\] .data             PROGBITS        00000000 000630 0000d8 00  WA  0   0 16
  \[ 4\] .bss              NOBITS          00000000 000708 000000 00  WA  0   0  4
  \[ 5\] .shstrtab         STRTAB          00000000 000708 000030 00      0   0  1
  \[ 6\] .symtab           SYMTAB          00000000 000878 000120 10      7  17  4
  \[ 7\] .strtab           STRTAB          00000000 000998 0000b1 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings)
  I (info), L (link order), G (group), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)

## How to fix the stack (in theory)

When you compile source code normally, gcc takes care of adding the GNU\_STACK markings so that the final object code is not marked with an executable stack unless it actually needs it. However, if you compile assembly code, gcc will not automatically add GNU\_STACK markings. So, the most common source of executable stacks in ELF binaries are packages which include raw assembly code. Note that we're not talking about inline assembly code, but rather files like .S which are written in pure assembler.

We can either patch each source file written in assembler and send the fixes upstream, or we can be lazy and simply force the package build system to assemble the source files with the GNU as option `--noexecstack` (but this is highly discouraged).

The advantage to patching the code is that it's easy to do, it's portable, and we can usually convince upstream to add it to their packages with little fuss. The disadvantage to patching is that we may have to patch many many files.

The advantage to just using `--noexecstack` is that you can simply add it to your ebuild and be done. The disadvantage is that the option isn't very portable (it won't work with non-GNU systems, and it probably won't even work with all GNU systems), and we can't really convince upstream to make this change. Thus, the only people who see the benefit here is Gentoo users. You gotta think big baby!

## How to fix the stack (in practice)

### Patching

The great thing about patching is that you can copy and paste this stuff everywhere. Just make sure the code will be preprocessed (e.g. the source file is named with .S and not .s). Stick these code snippets at the end of the source file, recompile, and do a jig.

### Compiling with --noexecstack

Often times you only need to add the following code in your ebuild. You must first be sure that the code does not actually require an executable stack as forcing this flag will break the package otherwise.

On the off chance that you cannot assemble the files, you can tell the linker to disable execstack stack.

### If all else fails

If all else fails, ask around on [#gentoo-dev](ircs://irc.libera.chat/#gentoo-dev) ([webchat](https://web.libera.chat/#gentoo-dev)) on Libera Chat. Or send an e-mail to the [gentoo-dev mailing list](https://archives.gentoo.org/gentoo-dev/). If no one can seem to answer your question, give me a poke either on IRC (nickname [SpanKY](https://wiki.gentoo.org/index.php?title=User:SpanKY&action=edit&redlink=1)  or  [Mike Frysinger (vapier)](https://wiki.gentoo.org/wiki/User:Vapier) ) or via e-mail.

## Arch Status

| Arch | Status | 
|---|---|
| **alpha** | fully supported (gcc-4.4.x/glibc-2.11) | 
| **amd64** | fully supported | 
| **arm** | fully supported (gcc-4.1.x/glibc-2.5) | 
| blackfin | fully supported (gcc-4.3+) | 
| **hppa** | gcc-3.4.x does not generate .note.GNU-stack | 
| ia64 | fully supported (gcc-3.4.4+) | 
| m68k | fully supported (gcc-3.4.x) | 
| **mips** | gcc-3.4.x does not generate .note.GNU-stack | 
| **ppc** | fully supported (gcc-4.4.x/glibc-2.11) | 
| **ppc64** | fully supported (gcc-4.4.x/glibc-2.11) | 
| s390 | fully supported | 
| s390x | fully supported | 
| sh | fully supported (gcc-3.4.x/glibc-2.5) | 
| **sparc** | fully supported | 
| **x86** | fully supported | 

## References

- Thanks to the [PaX team](https://pax.grsecurity.net/) for holding my hand
- Roland McGrath's [brain dump](http://www.redhat.com/archives/fedora-devel-list/2003-November/msg00838.html)
- [NX bit](https://en.wikipedia.org/wiki/NX_bit) Wikipedia entry
