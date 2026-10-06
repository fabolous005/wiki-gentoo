<!-- source: https://wiki.gentoo.org/wiki/ARM | group: Gentoo Wiki (Main) | wiki-title: ARM -->
---
title: ARM
url: https://wiki.gentoo.org/wiki/ARM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-22"
fingerprint: "1b82122be8533faa"
license: CC BY-SA 4.0
---

# ARM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is about compiling code for an ARM processor, taking advantage of its [FPU](https://en.wikipedia.org/wiki/Floating-point_unit).

## Prepare the toolchain

### Building with crossdev

If *nanolib*, *hardfloat* and *C++* support are not important, you may proceed to [building](https://wiki.gentoo.org#Building).



#### Building

We use [crossdev](https://wiki.gentoo.org/wiki/Crossdev) to create a local [ebuild repo](https://wiki.gentoo.org/wiki/Ebuild_repository) with symlinks to your "regular" Gentoo ebuild repository.
The ebuilds are thus the same as for your x86\_64 system, but are now used for cross-compiling to ARM.

The simplest command to build a toolchain with gcc's the default target configuration:

`root #````
crossdev --target arm-none-eabi
```
Depending on your CPU, this may not be enough though:

There are several profiles to enable ARM CPU targets in the gcc source code.
This feature is documented in `gccsource/INSTALL/configure.html` or `info gccinstall -n Configuration` searching for `with-multilib-list`:

| ARM GCC CPU profiles |  |  | 
|---|---|---|
| `profile_name` | CPU family | `gcc` source file | 
|---|---|---|
| aprofile | Cortex-A CPUs | gcc/config/arm/t-aprofile | 
| rmprofile | Cortex-R and Cortex-M CPUs | gcc/config/arm/t-rmprofile | 

These can be set (also multiple profiles with **comma separation**) when installing gcc with `--with-multilib-list`

`root #````
crossdev --target arm-none-eabi --genv 'EXTRA_ECONF="--with-multilib-list=$profile_name"'
```
#### Enable C++ support

Just enable the `cxx` `USE` flag in your cross *compiler* package [cross-arm-none-eabi/gcc](https://packages.gentoo.org/packages/cross-arm-none-eabi/gcc).

After you set the flag, rebuild the package.



#### Enable nanolib support

To enable the nano C library, [cross-arm-none-eabi/newlib](https://packages.gentoo.org/packages/cross-arm-none-eabi/newlib) needs the `nano` `USE` flag.



#### Create a ARM gdb

Either build [dev-debug/gdb](https://packages.gentoo.org/packages/dev-debug/gdb) with `multitarget` `USE` flag, or emerge the dedicated [cross-arm-none-eabi/gdb](https://packages.gentoo.org/packages/cross-arm-none-eabi/gdb).



#### Build for specific CPUs

You probably don't need this when you installed the toolchain with gcc `rmprofile` (see [above](https://wiki.gentoo.org#Building)).
Using it, your toolchain supports all various sub-architectures and flavors with floating point units and without.

To build toolchains for specific CPUs:

Quoting Embedded Artistry<sup>[\[1\]](https://wiki.gentoo.org#cite_note-Embedded_Artistry-1)</sup>:

It's a bit tricky to enable *hard floating point* targets.
One way to enable it, supposing the target processor has an [FPU](https://en.wikipedia.org/wiki/Floating-point_unit), is the following:

`root #````
crossdev --target arm-hardfloat-eabi --env \
```
```
           'EXTRA_ECONF="--with-cpu=cortex-m4 
                         --with-float-abi=hard 
                         --with-mode=thumb"'
```
Analyzing the above command, we replaced `-none-` with `-hardfloat-` in the cross-compile target speficier.

As for the `EXTRA_ECONF` flags, they were copied from a readme.txt file found in the ARM toolchain [source](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm)'s root or the following path for the pre-built version: share/doc/gcc-arm-none-eabi/. Here are the contents of the readme.txt for convenience:

`user $``cat readme.txt`
--------------------------------------------------------------------------
| Arm core   | Command Line Options                       | multilib     |
|------------|--------------------------------------------|--------------|
| Cortex-M0+ | -mthumb -mcpu=cortex-m0plus                | thumb        |
| Cortex-M0  | -mthumb -mcpu=cortex-m0                    | /v6-m        |
| Cortex-M1  | -mthumb -mcpu=cortex-m1                    |              |
|------------|--------------------------------------------|--------------|
| Cortex-M3  | -mthumb -mcpu=cortex-m3                    | thumb        |
|            |                                            | /v7-m        |
|------------|--------------------------------------------|--------------|
| Cortex-M4  | -mthumb -mcpu=cortex-m4                    | thumb        |
| (No FP)    |                                            | /v7e-m       |
|------------|--------------------------------------------|--------------|
| Cortex-M4  | -mthumb -mcpu=cortex-m4 -mfloat-abi=softfp | thumb        |
| (Soft FP)  |                                            | /v7e-m+fp    |
|            |                                            | /softfp      |
|------------|--------------------------------------------|--------------|
| Cortex-M4  | -mthumb -mcpu=cortex-m4 -mfloat-abi=hard   | thumb        |
| (Hard FP)  |                                            | /v7e-m+fp    |
|            |                                            | /hard        |
|------------|--------------------------------------------|--------------|
| Cortex-M7  | -mthumb -mcpu=cortex-m7                    | thumb        |
| (No FP)    |                                            | /v7e-m       |
|            |                                            | /nofp        |
|------------|--------------------------------------------|--------------|
| Cortex-M7  | -mthumb -mcpu=cortex-m7 -mfloat-abi=softfp | thumb        |
| (Soft FP)  |                                            | /v7e-m+dp    |
|            |                                            | /softfp      |
|------------|--------------------------------------------|--------------|
| Cortex-M7  | -mthumb -mcpu=cortex-m7 -mfloat-abi=hard   | thumb        |
| (Hard FP)  | -mfpu=fpv5-sp-d16                          | /v7e-m+dp    |
|            |                                            | /hard        |
|------------|--------------------------------------------|--------------|
| Cortex-M23 | -mthumb -mcpu=cortex-m23                   | thumb        |
|            |                                            | /v8-m.base   |
|------------|--------------------------------------------|--------------|
| Cortex-M33 | -mthumb -mcpu=cortex-m33                   | thumb        |
|  (No FP)   |                                            | /v8-m.main   |
|            |                                            | /nofp        |
|------------|--------------------------------------------|--------------|
| Cortex-M33 | -mthumb -mcpu-cortex-m33                   | thumb        |
| (Soft FP)  | -mfloat-abi=softfp                         | /v8-m.main+fp|
|            |                                            | /softfp      |
|------------|--------------------------------------------|--------------|
| Cortex-M33 | -mthumb -mcpu=cortex-m33                   | thumb        |
| (Hard FP)  | -mfloat-abi=hard                           | /v8-m.main+fp|
|            |                                            | /hard        |
|------------|--------------------------------------------|--------------|
| Cortex-R4  | \[-mthumb\] -mcpu=cortex-r?                  | thumb        |
| Cortex-R5  |                                            | /v7          |
| Cortex-R7  |                                            | /nofp        |
| Cortex-R8  |                                            |              |
| (No FP)    |                                            |              |
|------------|--------------------------------------------|--------------|
| Cortex-R5  | \[-mthumb\] -mcpu=cortex-r?                  | thumb        |
| Cortex-R7  | -mfloat-abi=softfp                         | /v7+fp       |
| Cortex-R8  |                                            | /softfp      |
| (Soft FP)  |                                            |              |
|------------|--------------------------------------------|--------------|
| Cortex-R5  | \[-mthumb\] -mcpu=cortex-r?                  | thumb        |
| Cortex-R7  | -mfloat-abi=hard                           | /v7+fp       |
| Cortex-R8  |                                            | /hard        |
| (Hard FP)  |                                            |              |
|------------|--------------------------------------------|--------------|
| Cortex-R52 | \[-mthumb\] -mcpu=cortex-r52                 | thumb        |
| (No FP)    |                                            | /v7          |
|            |                                            | /nofp        |
|------------|--------------------------------------------|--------------|
| Cortex-R52 | \[-mthumb\] -mcpu=cortex-r52                 | thumb        |
| (Soft FP)  | -mfloat-abi=softfp                         | /v7+fp       |
|            |                                            | /softfp      |
|------------|--------------------------------------------|--------------|
| Cortex-R52 | \[-mthumb\] -mcpu=cortex-r52                 | thumb        |
| (Soft FP)  | -mfloat-abi=hard                           | /v7+fp       |
|            |                                            | /hard        |
|------------|--------------------------------------------|--------------|
| Cortex-A\*  | \[-mthumb\] -mcpu=cortex-a\*                  | thumb        |
| (No FP)    |                                            | /v7          |
|            |                                            | /nofp        |
|------------|--------------------------------------------|--------------|
| Cortex-A\*  | \[-mthumb\] -mcpu=cortex-a\*                  | thumb        |
| (Soft FP)  | -mfloat-abi=softfp                         | /v7+fp       |
|            |                                            | /softfp      |
|------------|--------------------------------------------|--------------|
| Cortex-A\*  | \[-mthumb\] -mcpu=cortex-a\*                  | thumb        |
| (Hard FP)  | -mfloat-abi=hard                           | /v7+fp       |
|            |                                            | /hard        |
--------------------------------------------------------------------------

### Using the pre-built toolchain

A pre-built toolchain is the [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm). Remember to update your `PATH` by prepending the location of the toolchain's bin folder:

`user $````
export PATH="/path/to/toolchain/bin:$PATH"
```
## Writing code

#### Error message: .... uses VFP register arguments ... does not

This probably means that some of the libraries being linked, were compiled with `hardfloat` (`-mfloat=hardfloat`) while others with `floatfp` or `float`. This can also happen when the compiler was compiled in an opposite to the code manner.

#### Error message: undefined reference to \`\_\_stack\_chk\_guard'

The `__stack_chk*` symbols are defined by libc.a and are used for [stack smashing protection](https://en.wikipedia.org/wiki/Buffer_overflow_protection).

In the case of compilation errors like ``undefined reference to `__stack_chk_guard'``, you can disable stack smashing guards by disabling the `ssp` `USE` for cross-arm\*/gcc.



### Using Mbed

[Mbed](https://www.mbed.com) is an online platform for writing and compiling code for various boards. It has an *export* function that enables retrieving the said code including a Makefile and the imported libraries.

#### Missing mbed\_config.h

If compilation fails with a missing mbed\_config.h file, Makefile needs to be adapted with a point to the root folder's mbed\_config.h file.

### Using STM32CubeMX

[STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html) can be used to initialize code. In provides a graphical user interface to choose pin modes and clocks.

### Using pre-built toolchain's samples

The pre-built [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm), comes with code samples and Makefiles.

## See also

## External resources

1. [Embedded Artistry](https://embeddedartistry.com/blog/2017/10/9/r1q7pksku2q3gww9rpqef0dnskphtc) explains the difference between `hard`, `softfp` and `soft` (the three ARM floating point compiler options).
2. [Discussion](http://gentoo.2317880.n4.nabble.com/Building-a-bare-metal-ARM-hard-float-compiler-what-ABI-td300828.html) on the problems enabling *hardfloat*.
3. Another similar [discussion](https://forums.gentoo.org/viewtopic-t-1067608-start-0.html) in the Gentoo forums.

## Referencies

1. [↑](https://wiki.gentoo.org#cite_ref-Embedded_Artistry_1-0) Phillip Johnston. ["Demystifying ARM Floating Point Compiler Options"](https://embeddedartistry.com/blog/2017/10/9/r1q7pksku2q3gww9rpqef0dnskphtc), October 11, 2017. Retrieved on 2019-09-30
