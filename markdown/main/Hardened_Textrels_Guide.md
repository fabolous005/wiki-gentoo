<!-- source: https://wiki.gentoo.org/wiki/Hardened/Textrels_Guide | group: Gentoo Wiki (Main) | wiki-title: Hardened/Textrels Guide -->
---
title: Hardened/Textrels Guide
url: https://wiki.gentoo.org/wiki/Hardened/Textrels_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: aa599b132f9e0f98
license: CC BY-SA 4.0
---

# Hardened/Textrels Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A guide for tracking down and fixing .text relocations (TEXTRELs)

## Introduction

You should make sure to read the [Introduction to Position Independent Code](https://wiki.gentoo.org/wiki/Hardened/Introduction_to_Position_Independent_Code) before tackling this guide.

This guide is x86-centric for now. The reason being, the majority of broken object files are due to poorly written x86 assembly stemming from the simple fact that the x86 architecture has so few registers. Other architectures have a large enough register set that they can reserve a register as the "PIC register" without incurring a performance hit. Every architecture has to be mindful of PIC and its implications, x86 just happens to be the dominant architecture at the moment in the 'desktop' world of open source.

We will update for non-x86 as we acquire details and useful examples.

## Finding broken object code

Before you can start fixing something, you got to make sure it's broken first, right? For this reason, we've developed a suite of tools named [PaX Utilities](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Utilities). If you are not familiar with these utilities, you should read the [PaX Utilities Guide](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Utilities) now. Gentoo users can simply do `emerge pax-utils`. Non-Gentoo users should be able to find a copy of the source tarball in the distfiles on a [Gentoo Mirror](http://www.gentoo.org/main/en/mirrors.xml). Once you have the PaX Utilities setup on your system, we can start playing around with `scanelf`.

Keep in mind that although these utilities are named PaX Utilities, they certainly do not require PaX or anything else like that on your system. The name is a historical artifact and want of a better name, has stuck.

Let's see if your system has any broken files.

`user $``scanelf -lpqt`
TEXTREL  /usr/lib/opengl/xorg-x11/lib/libGL.so.1.2
TEXTREL  /usr/lib/libSDL-1.2.so.0.7.2
TEXTREL  /usr/lib/libdv.so.4.0.2
TEXTREL  /usr/lib/libsmpeg-0.4.so.0.1.3
TEXTREL  /usr/lib/libOSMesa.so.4.0
TEXTREL  /usr/lib/libxvidcore.so.4.1

Ideally, scanelf should not display anything, but on an x86 system, this is rarely the case. Here we can see six libraries with TEXTRELs in them. To quickly find out what package these files come from, Gentoo users can `emerge portage-utils` and use `qfile`.

`user $``` qfile `scanelf -qylpF%F#t` ``
media-libs/libdv (/usr/lib/libdv.so.4.0.2)
media-libs/libsdl (/usr/lib/libSDL-1.2.so.0.7.2)
media-libs/smpeg (/usr/lib/libsmpeg-0.4.so.0.1.3)
media-libs/xvid (/usr/lib/libxvidcore.so.4.1)
x11-base/xorg-x11 (/usr/lib/opengl/xorg-x11/lib/libGL.so.1.2)
x11-base/xorg-x11 (/usr/lib/libOSMesa.so.4.0)

Now that we know the offenders, we have a choice. We can file a bug upstream (who generally don't care unless you can provide a fix), file a bug in the [Gentoo Bugzilla](https://bugs.gentoo.org/) (which is a nice lazy cop out), or we can fix it ourselves (that is why you're reading this guide right?). You should double check that the package version you have installed is the latest upstream has to offer and the latest version your distro has to offer. Who knows, maybe you can get lucky and someone else has already fixed it. If you wish to get feedback on your work, feel free to contact the [Gentoo hardened team](mailto:hardened@gentoo.org).

### "False" Positives

Sometimes you may come across a package which contains a mountain of TEXTRELs with seemingly no relation to assembly code. This may simply be because the objects were not properly compiled with the appropriate PIC flag. The fix is quite simple: make sure every object file that is linked into the final shared object is compiled with the appropriate PIC flag (typically -fPIC).

For example, let's look at the silc-plugin package. It builds up a few modules, but only compiles some of the objects with -fPIC that are linked into the final libsilc\_core.so module. The output of scanelf here is quite extensive!

`user $``scanelf -qT /usr/lib/irssi/modules/libsilc_core.so | wc -l`
10734

`user $``scanelf -qT /usr/lib/irssi/modules/libsilc_core.so`
...
  libsilc\_core.so: silc\_client\_ftp\_ask\_name \[0xD542C\] in silc\_client\_receive\_new\_id \[0xD5380\]
  libsilc\_core.so: silc\_client\_run\_one \[0xD55CA\] in silc\_client\_receive\_new\_id \[0xD5380\]
  libsilc\_core.so: silc\_id\_payload\_parse \[0xD5842\] in silc\_client\_packet\_parse\_type \[0xD57B0\]
  libsilc\_core.so: fgetc@@GLIBC\_2.0 \[0xD5857\] in silc\_client\_packet\_parse\_type \[0xD57B0\]
...

A TEXTREL on glibc's fgetc() function!? Either people are calling fgetc() from assembly (and should be shot), or something else is going on. A good rule of thumb is that if it seems like just about every function/variable reference causes a TEXTREL and it is all done in C code, then the file was not built as PIC. Just review the build output and see if the command to compile it was invoked with -fPIC. If not, go fix the build system as you do not need to dig into the source. Dodged the bullet this time!

## Dissecting broken object code

So we have identified some broken libraries, and we want to fix them. The trouble is, shared library code can be huge. They can have thousands of functions which come from thousands of object files and thousands of source code files which total megabytes in size (source code and compiled objects). Where the hell do we start!? Once again, Mighty Mouse^W^W `scanelf` is here to save the day. Before we dive into source code, lets check out a few libraries.

### Dissect libsmpeg

`user $``scanelf -qT /usr/lib/libsmpeg-0.4.so.0.1.3`
libsmpeg-0.4.so.0.1.3: (memory/fake?) \[0x2FB3C\] in cpu\_flags \[0x2FB10\]
  libsmpeg-0.4.so.0.1.3: (memory/fake?) \[0x2FB42\] in cpu\_flags \[0x2FB10\]
  libsmpeg-0.4.so.0.1.3: (memory/fake?) \[0x2FB55\] in IDCT\_mmx \[0x2FB48\]
  libsmpeg-0.4.so.0.1.3: (memory/fake?) \[0x2FB84\] in IDCT\_mmx \[0x2FB48\]
  /usr/lib/libsmpeg-0.4.so.0.1.3

The output here tells us that the *cpu\_flags* and the *IDCT\_mmx* functions are to blame for our TEXTRELs. The first field indicates that this is poor usage of memory references. Unfortunately, the symbolic name of the memory being referenced has not been retained in the object code (probably because the code is hand written assembly), so we need to do a little more digging. This is where the offset addresses come in to play along with the `objdump` utility from the *binutils* package. The first address (e.g. 0x2FB3C) is the offset of the TEXTREL while the second address is the offset of the function (e.g. 0x2FB10). Get used to this because the behavior of not retaining the symbol name is quite common.

`user $``objdump -d /usr/lib/libsmpeg-0.4.so.0.1.3`
...
   2fb0f:       90                      nop
  
0002fb10 \<cpu\_flags>:
   2fb10:       9c                      pushf
   2fb11:       58                      pop    %eax
...
   2fb32:       60                      pusha
   2fb33:       b8 01 00 00 00          mov    $0x1,%eax
   2fb38:       0f a2                   cpuid
   2fb3a:       89 15 d0 d3 03 00       mov    %edx,0x3d3d0
   2fb40:       61                      popa
   2fb41:       a1 d0 d3 03 00          mov    0x3d3d0,%eax
   2fb46:       c3                      ret
   2fb47:       90                      nop
...

As you can see here, the two lines picked out in the body of *cpu\_flags* have absolute memory references. In this case, they both refer to memory location *0x3d3d0*. Since this object code may be loaded into any address, using an absolute reference obviously won't fly. That means everytime libsmpeg is loaded into memory, the dynamic loader has to rewrite the *0x3d3d0* to the actual calculated address on the fly.

### Dissect libdv

`user $``scanelf -qT /usr/lib/libdv.so.4.0.2`
libdv.so.4.0.2: (memory/fake?) \[0x14AA9\] in dv\_parse\_ac\_coeffs\_pass0 \[0x14A84\]
  libdv.so.4.0.2: (memory/fake?) \[0x14C28\] in dv\_parse\_ac\_coeffs\_pass0 \[0x14A84\]
  libdv.so.4.0.2: (memory/fake?) \[0x14C8A\] in dv\_parse\_video\_segment \[0x14C6F\]
  libdv.so.4.0.2: (memory/fake?) \[0x14CA6\] in dv\_parse\_video\_segment \[0x14C6F\]
  libdv.so.4.0.2: (memory/fake?) \[0x15248\] in \_dv\_idct\_block\_mmx \[0x15210\]
  libdv.so.4.0.2: (memory/fake?) \[0x152BE\] in \_dv\_idct\_block\_mmx \[0x15210\]
  libdv.so.4.0.2: (memory/fake?) \[0x1583D\] in \_dv\_dct\_88\_block\_mmx \[0x157F8\]
  libdv.so.4.0.2: (memory/fake?) \[0x15847\] in \_dv\_dct\_88\_block\_mmx \[0x157F8\]
  libdv.so.4.0.2: (memory/fake?) \[0x15F91\] in \_dv\_dct\_248\_block\_mmx \[0x15F58\]
  libdv.so.4.0.2: (memory/fake?) \[0x15FE6\] in \_dv\_dct\_248\_block\_mmx \[0x15F58\]
  libdv.so.4.0.2: (memory/fake?) \[0x163D3\] in \_dv\_rgbtoycb\_mmx \[0x163C8\]
  libdv.so.4.0.2: (memory/fake?) \[0x163DD\] in \_dv\_rgbtoycb\_mmx \[0x163C8\]
  libdv.so.4.0.2: dv\_vlc\_class\_index\_mask \[0x149A7\] in dv\_decode\_vlc \[0x14998\]
  libdv.so.4.0.2: dv\_vlc\_class\_index\_rshift \[0x149B0\] in dv\_decode\_vlc \[0x14998\]
  libdv.so.4.0.2: dv\_vlc\_classes \[0x149B9\] in dv\_decode\_vlc \[0x14998\]
  libdv.so.4.0.2: dv\_vlc\_index\_mask \[0x149C4\] in dv\_decode\_vlc \[0x14998\]
  libdv.so.4.0.2: sign\_mask \[0x149EB\] in dv\_decode\_vlc \[0x14998\]
  libdv.so.4.0.2: sign\_mask \[0x14A5D\] in \_\_dv\_decode\_vlc \[0x14A1C\]
  libdv.so.4.0.2: sign\_mask \[0x14B82\] in dv\_parse\_ac\_coeffs\_pass0 \[0x14A84\]
  libdv.so.4.0.2: dv\_vlc\_class\_lookup5 \[0x14A2F\] in \_\_dv\_decode\_vlc \[0x14A1C\]
  libdv.so.4.0.2: dv\_parse\_ac\_coeffs\_pass0 \[0x14E03\] in dv\_parse\_video\_segment \[0x14C6F\]
  libdv.so.4.0.2: dv\_parse\_ac\_coeffs \[0x14E51\] in dv\_parse\_video\_segment \[0x14C6F\]
  libdv.so.4.0.2: dv\_quant\_offset \[0x14E69\] in \_dv\_quant\_88\_inverse\_x86 \[0x14E5C\]
  libdv.so.4.0.2: dv\_quant\_offset \[0x14FB3\] in \_dv\_quant\_x86 \[0x14FA4\]
  /usr/lib/libdv.so.4.0.2

Again, we can see that many functions (like *dv\_parse\_ac\_coeffs\_pass0* and *\_dv\_idct\_block\_mmx*) have absolute memory references. What we also see is that a bunch of functions which refer to variables. For example, *dv\_decode\_vlc* misuses the variable *dv\_vlc\_class\_index\_mask* while *dv\_parse\_video\_segment* misuses the variable *dv\_parse\_ac\_coeffs*. Much easier to locate the problem in the source code when you have the symbol name.

### Dissect libSDL

`user $``scanelf -qT /usr/lib/libSDL-1.2.so.0.7.2`
libSDL-1.2.so.0.7.2: (memory/fake?) \[0x4E213\] in \_ConvertMMXpII32\_24RGB888 \[0x4E210\]
  libSDL-1.2.so.0.7.2: (memory/fake?) \[0x4E29E\] in \_ConvertMMXpII32\_16RGB565 \[0x4E29B\]
  libSDL-1.2.so.0.7.2: (memory/fake?) \[0x4E3F6\] in \_ConvertMMXpII32\_16BGR555 \[0x4E3F3\]
  libSDL-1.2.so.0.7.2: (memory/fake?) \[0x4E402\] in \_ConvertMMXpII32\_16RGB555 \[0x4E3FF\]
  libSDL-1.2.so.0.7.2: (memory/fake?) \[0x4E555\] in \_Hermes\_X86\_CPU \[0x4E529\]
  libSDL-1.2.so.0.7.2: \_copy\_row \[0x316A2\] in SDL\_SoftStretch \[0x313C0\]
  libSDL-1.2.so.0.7.2: \_mmxreturn \[0x4E4FB\] in \_ConvertMMXpII32\_16RGB555 \[0x4E3FF\]
  libSDL-1.2.so.0.7.2: \_x86return \[0x4E590\] in \_ConvertX86p16\_16BGR565 \[0x4E560\]
  libSDL-1.2.so.0.7.2: \_x86return \[0x4EE99\] in \_ConvertX86p32\_16BGR555 \[0x4EDCA\]
  libSDL-1.2.so.0.7.2: \_x86return \[0x4EF4D\] in \_ConvertX86p32\_8RGB332 \[0x4EE9D\]
  /usr/lib/libSDL-1.2.so.0.7.2

Doesn't seem to be anything new here. Poor memory usage in functions like *\_ConvertMMXpII32\_24RGB888* and no symbol name which means it's probably pure hand written assembler. The *SDL\_SoftStretch* function misuses the symbol *\_copy\_row* and since the symbol name has been retained, it's probably inline assembly code.

## Finding the broken source code

We've identified the functions and sometimes the variables which are causing us such headaches. Before we can actually fix them though, we have to narrow down the source code to the offending lines. Since we know the function names and either the symbol name or a relative position in the function, we should be able to focus our efforts quite easily.

### libsmpeg source dive

Let's start with libsmpeg. We know that both the *cpu\_flags* and *IDCT\_mmx* functions are broken. But where are they defined?

`user $````
tar zxf smpeg-0.4.4.tar.gz
```
`user $````
cd smpeg-0.4.4.tar.gz
```
`user $``grep -Rl cpu_flags *`
video/mmxflags\_asm.S
video/parseblock.cpp

`user $``grep cpu_flags video/mmxflags_asm.S````
.globl cpu_flags
        .type    cpu_flags,@function ## <-- here is what we want
cpu_flags:
        jz cpu_flags.L1   # Processor is 386
        je cpu_flags.L1
cpu_flags.L1:
        .size    cpu_flags,.Lfe1-cpu_flags
```
`user $``grep -Rl IDCT_mmx *`
video/parseblock.cpp
video/mmxidct\_asm.S

`user $``grep IDCT_mmx video/mmxidct_asm.S````
.globl IDCT_mmx
        .type    IDCT_mmx,@function ## <-- here is what we want
IDCT_mmx:
        .size    IDCT_mmx,.Lfe1-IDCT_mmx
```
As we suspected, both the *cpu\_flags* and the *IDCT\_mmx* functions are written in pure assembly code. This makes tracking down the unamed memory reference easier because the source code should closely match the output of `objdump`. If we review the output from earlier, we know the *cpuid* instruction is used. Since it isn't a common instruction, we search for it in the source code.

`user $``grep -A 8 cpuid video/mmxflags_asm.S````
        cpuid
  
        movl %edx,flags
  
        popa
  
        movl flags,%eax
  
cpu_flags.L1:
```
In GNU assembler, registers are prefixed with a *%* and constants are prefixed with a *$* , that *flags* looks suspicious. It also lines up well with the `objdump` output from earlier. So what is *flags*?

`user $``grep -C 2 flags video/mmxflags_asm.S````
.data
        .align 16
        .type    flags,@object
flags: .long 0
  
.text
```
Seems *flags* is a data variable local to mmxflags\_asm.S which functions access with absolute memory references rather than relative. Now we are pretty much done. That's all there is to it. We started with the library libsmpeg.so and tracked it back to the function *cpu\_flags* and the variable *flags* in the video/mmxflags\_asm.S file. That wasn't so hard now was it? :)

If we analyze the *IDCT\_mmx* function, we find a similar trend.

### libSDL source dive

Again, before we jump into how to fix these, lets analyze a few more source files to get a better handle on identifying problematic code.

Simple enough, the *\_ConvertMMXpII32\_24RGB888* function refers to the *mmx32\_rgb888\_mask* variable.

Another straight forward bug. An absolute reference to the *\_copy\_row* variable in assembly. If we were to let gcc handle the *\_copy\_row* reference instead though...

## How to write PIC (in theory)

### Rules of thumb

Now we know what broken code looks like. We can point out issues in code and confidently declare "that crap is broken". While this is a good thing, it certainly doesn't help much if no one knows how it's supposed to be written. Let's start with some rules of thumb.

General rules

- Do not mix PIC and non-PIC object code
- Shared libraries contain PIC objects
- Static libraries contain non-PIC objects (normal/non-PIE systems only)
- Let gcc figure out the details whenever possible (e.g. inline asm)
- Use the stack for loading of large masks instead of variables
- Do not clobber the PIC register when generating PIC objects

x86-specific rules

- Use @GOT relocations when using external symbols
- Use @GOTOFF relocations when using local symbols

### PIC registers by architecture

| arch | register | 
|---|---|
| blackfin | P3 | 
| frv | GR15 | 
| hppa | r19 | 
| x86 | ebx | 

## Cookie cutter PIC fixes

### Don't use the PIC register

If you come across code which uses the PIC register in some inline assembly, one fix may be to simply use a different register. For example, the x86 architecture has 6 general purpose registers (*eax*, *ebx*, *ecx*, *edx*, *esi*, *edi*). If the code uses just *eax* and *ebx*, just change all references of *ebx* to *ecx* and you're done!

A cleaner fix might be to just let gcc allocate the registers accordingly. If the inline assembly doesn't actually care which registers it uses, change the references from *ebx* to *r* in the clobber list, and refer to the variable by number.

Or, if the assembly uses an instruction which always clobbers *ebx* (e.g. *cpuid*), simply hide the value in another register (like *esi*).

If all else fails, you can fall back to the slow push/pop *ebx* on the stack method.

### MMX/SSE masks

A lot of x86 MMX/SSE code loads bitmasks from local variables since they need to fill up a register which is larger (MMX/64bits or SSE/128bits) than the native bitsize (x86/32bits). They do this by defining the mask in consecutive bytes in memory and then having the cpu load the data from the memory region.

One way to get around this is by being creative with the stack. Rather than use an absolute memory reference for the mask, push a bunch of 32bit values onto the stack and use the address specified by the *esp* register. Once you're finished, just add a constant to *esp* rather than popping off since you don't care about the actual values once they are loaded into the MMX/SSE registers.

### Let gcc worry about it

A lot of inline assembly is written with the symbol names placed right in the code. Rather than trying to write custom code to handle PIC in assembly, just let gcc worry about it. Pass in the symbol via the input operand list as a memory constraint ("m") and gcc will handle all the rest.

If your get a warning/error about one of the memory inputs needing to be an lvalue, then this usually means you're trying to pass in a pointer to an array/structure rather than the memory location itself. Fixing this may be as simple as dereferencing the variable in the constraint list rather than in the assembly itself.

### Thunk it in assembly

Hand written assembly sometimes need to access variables (whether they be local or global). Since none of the previous tricks will work, you just need to grind your teeth and dig in to write real PIC references yourself using the GOT. Make sure you keep in mind the first rule of thumb: Do not mix PIC and non-PIC object code. This probably will require the hand written assembly be preprocessed before it is assembled, so an assembly source file with a *.s* suffix will not work. It needs to be *.S*.

Also keep in mind that using @GOTOFF will return the variable while using @GOT will return a pointer to the variable. So accessing a variable with @GOT will require two steps.

Since we hide the PIC details behind the preprocessor define *\_\_PIC\_\_*, we know that the correct code will be generated for both the PIC and non-PIC cases.

The *\_\_i686.get\_pc\_thunk.bx* function is a standard method for acquiring the address of the GOT at runtime and storing the result in *ebx*. The funky name is what gcc uses by convention when generating PIC objects, so we too use the same name. The *@GOT* and *@GOTOFF* notation tells the assembler where to find the variables in memory. The *.section .gnu.linkonce.t* is useful because it tells the linker to only include one instance of this function in the final object code. So if you have multiple files which declare this same function which are compiled and linked into the same final library, the linker will discard all duplicate instances of the function thus saving space (which is always a good thing).

## How to fix broken PIC (in practice)

So if the previous code snippets were broken, what should they look like you may wonder. Well let's find out.

### Fix libsmpeg

### Fix libSDL

## External resources

- [GCC Inline Assembly HOWTO](http://www.ibiblio.org/gferg/ldp/GCC-Inline-Assembly-HOWTO.html)
- [NASM](http://nasm.sourceforge.net/)'s documentation on [ELF shared libraries](http://nasm.sourceforge.net/doc/html/nasmdoc6.html#section-6.5.2)
- [Linkers and loaders](https://packages.gentoo.org/packages/app-doc/linkers-and-loaders) ([chapter 8](http://www.iecc.com/linker/linker08.html) and [chapter 10](http://www.iecc.com/linker/linker10.html))
- [Textrels overview (Flameeyes blog)](https://flameeyes.blog/2016/01/16/textrels-text-relocations-and-their-impact-on-hardening-techniques/)
