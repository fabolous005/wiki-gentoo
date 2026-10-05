<!-- source: https://wiki.gentoo.org/wiki/Android/SharkBait/Building_a_toolchain_for_aarch64-linux-android | group: Gentoo Wiki (Main) | wiki-title: Android/SharkBait/Building a toolchain for aarch64-linux-android -->
---
title: Android/SharkBait/Building a toolchain for aarch64-linux-android
url: https://wiki.gentoo.org/wiki/Android/SharkBait/Building_a_toolchain_for_aarch64-linux-android
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: "2f8cfa5b5c066d22"
license: CC BY-SA 4.0
---

# Android/SharkBait/Building a toolchain for aarch64-linux-android

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

To build the Android Platform, we need a toolchain that can produce executables that link correctly against Android's libc, Bionic. To achieve this, a toolchain targeting aarch64-linux-android (and arm-linux-androideabi) is needed. This article aims to provide a step-by-step guide for reproducing the desired toolchain on all architecture that GCC supports running on, including AArch64, which my GSoC project needs. Unfinished work on this topic is documented at the end of this article.

## Step 0 -- Clone source and set up environment variables

Clone the following repositories which hold Google's modifications to the GCC toolchain for Android target:

The rest of this article assumes that $PWD is in the corresponding project for each step.

Export the environment variables to get the paths right:

`user $``export TARGET=aarch64-linux-android``user $``export PREFIX=/usr/local/$TARGET`
## Step 1 -- Build and install binutils

Binutils that can process binaries for the target is needed. Create a separate build directory, configure, compile, and then install for target:

`user $``mkdir build && cd build``user $``../binutils-2.27/configure --target=$TARGET --prefix=$PREFIX --enable-werror=no``user $``make -j8 && sudo make install`
The `--enable-werror=no` option is used to work around failed compiles due to newer versions of GCC generating new warnings. Examine $PREFIX and see if the binutils for the target has been properly installed.

## Step 2 -- Install prebuilt libc & headers

The next step, as most cross-compiler creation guides instructs, is to install libc. Unfortunately, we have not figured out a proper way to compile Bionic without Android's (gigantic) build system, so we're just doing a voodoo copy-and-paste. The libc part in this section is expected to get better when we develop a mature solution for building Bionic.

Install sys-kernel/linux-headers-3.10.73 from [here](https://github.com/KireinaHoro/android/tree/master/sys-kernel/linux-headers). Set up libc and kernel headers.

`user $``sudo emerge -av =sys-kernel/linux-headers-3.10.73``user $``mkdir -p $PREFIX/$TARGET/sys-include``user $``cp -Rv bionic/libc/include/* $PREFIX/$TARGET/sys-include/``user $``for a in linux asm asm-generic; do ln -s /usr/include/$a $PREFIX/$TARGET/sys-include/; done`
The following object files are needed for a successful generation of the toolchain:

- crtbegin\_so.o: from NDK
- crtend\_so.o: from NDK
- crtbegin\_dynamic.o: from NDK
- crtend\_android.o: from NDK
- libc.so: from AOSP
- libm.so: from AOSP
- libdl.so: from AOSP
- ld-android.so: from AOSP

Obtain the above files and place them under $PREFIX/$TARGET/lib for discovery by the linker.

## Step 3 -- Build and install GCC

The final part is relatively simple. Just compile GCC and install it into the toolchain prefix:

`user $``mkdir build && cd build``user $``../gcc-4.9/configure --target=$TARGET --prefix=$PREFIX --without-headers --with-gnu-as --with-gnu-ld --enable-languages=c,c++``user $``make -j8 && sudo make install`
## Step 4 -- Build and verify "Hello, world!"

We need to verify that the toolchain is really working by creating executables for our target. Write a simple "Hello, world!" program:

**`hello.cc`**

```
#include <iostream>
int main() {
    std::cout << "Hello, world!" << std::endl;
    return 0;
}
```
Compile it with:

`user $``aarch64-linux-android-g++ hello.cc -o hello -pie -static-libgcc -nostdinc++ -I/usr/local/aarch64-linux-android/include/c++/v1 -nodefaultlibs -lc -lm -lc++`
Explanation for the commandline options used:

- `-pie`: Android requires [Position Independent Executables](https://en.wikipedia.org/wiki/Position-independent_code) property for dynamically-linked executables.
- `-static-libgcc`: Android platform does not have libgcc\_s.so available; we'll have to make it statically-linked.
- `-nostdinc++` and `-I...`: Android uses libc++ as its default STL implementation, and we need this to get the right symbols used by including right C++ headers.
  - This suggests that a correct copy of libcxx headers should be present at the path shown above.
- `-nodefaultlibs` and `-l...`: by default GCC links to libstdc++, which is not desirable in this case; we manually specify what to consider during the linking process.
  - This suggests that libc++.so should be present in the linker search path.

## What else?

The work is not finished yet on this topic:

- We still copy-and-paste libc object files instead of properly building them separately. Proper packaging of bionic is needed.
- The toolchain build process needs integrating with crossdev, Gentoo's flexible cross-compile toolchain generator.
