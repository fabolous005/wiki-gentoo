<!-- source: https://wiki.gentoo.org/wiki/Android/SharkBait/Clang_toolchain_with_sanitizers_support_for_Android | group: Gentoo Wiki (Main) | wiki-title: Android/SharkBait/Clang toolchain with sanitizers support for Android -->
---
title: Android/SharkBait/Clang toolchain with sanitizers support for Android
url: https://wiki.gentoo.org/wiki/Android/SharkBait/Clang_toolchain_with_sanitizers_support_for_Android
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: ae84df2b85867d00
license: CC BY-SA 4.0
---

# Android/SharkBait/Clang toolchain with sanitizers support for Android

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Toolchain work for Android has been progressing in the last few weeks, finally reaching a stage where a completely working Clang/LLVM toolchain is available. The toolchain can build test programs that run on vanilla AOSP as well as Lineage OS, with proper sanitizer support. Though actually plugging this toolchain into Android build process is not tested yet, the sanitizers for the toolchain is built with the toolchain itself, and various problems in the headers has been fixed. This article explains complex parts in [the bootstrap script](https://gist.github.com/KireinaHoro/282f6c1fef8b155126aaeb0acccf4280) as well as places where manual intervention is needed.

We're going to build pieces of the toolchain in this order to satisfy dependency:

- GCC toolchain (binutils, libc objects & headers, compiler)
- LLVM + Clang
- compiler-rt builtins
- libunwind
- libc++abi
- libc++
- compiler-rt non-builtins (sanitizers, profilers, etc.)

## Set up environment and repositories

This part defaults to using aarch64-linux-android as the target and using the HEAD version of LLVM tools. Edit accordingly if this is not desired. CMake and Ninja is required to follow this guide; install them if they're not present on your system.

## Set up GCC for target

GCC is needed for cross compiling with Clang/LLVM. Follow [this article on GCC cross-compiling for Android](https://wiki.gentoo.org/wiki/User:Jsteward/Building_a_toolchain_for_aarch64-linux-android) to get a working copy of cross GCC. This should get you through the comment block that reads:

Note that later Clang/LLVM build requires prebuilt libraries in two different locations. Copy the object files accordingly.

## LLVM + Clang

Configure options to note:

- `LLVM_TARGETS_TO_BUILD=AArch64`: Enable only AArch64 target. Remember to substitute this if the target is not aarch64 (e.g. ARM for 32 bit)

## Compiler-rt builtins

Configure options to note:

- `CMAKE_INSTALL_PREFIX=$PREFIX/lib/clang/7.0.0`: Install to Clang "resource path" so that libraries can be automatically found by Clang while compiling.
- `COMPILER_RT_BUILD_BUILTINS=ON` and `COMPILER_RT_BUILD_*=OFF`: Build builtins only as other components require libc++ to build.

## Libunwind

Nothing special here.

## Libcxxabi

Configure options to note:

- `LIBCXXABI_LIBCXX_INCLUDES="../../libcxx/include"`: Specify libc++ header path for reference by libc++abi during build.
- `LIBCXXABI_USE_COMPILER_RT=ON`: This option name speaks for itself.
- `LIBCXXABI_USE_LLVM_UNWINDER=ON`: This option name speaks for itself.

Remember to install libc++abi headers into the prefix as CMake doesn't generate libc++abi header install rules:

## Libcxx

As discussed in [this thread](https://reviews.llvm.org/D46558), LLVM HEAD at the time of writing uses NDK headers that are newer than the prebuilt NDK version (r16 vs r14). As a result, we have to rebase to remove commit 85a7702b4cc5d69402791fe685f151cf3076be71 from Libcxx repository:

Configure options to note:

- `LIBCXX_CXX_ABI="libcxxabi"`: Use libcxxabi as the C++ ABI library (instead of libcxxrt or libsupc++).
- `LIBCXX_CXX_ABI_INCLUDE_PATHS="$PREFIX/include/libcxxabi"`: Reference to libcxxabi headers (installed in the previous step).
- `LIBCXX_USE_COMPILER_RT=ON`: This option name speaks for itself.

Note that Android by default combines libc++, libc++abi, and libunwind into a single libc++.so. We'll do this as well so that the executable does not contain stray dynamic link references.

## Compiler-rt non-builtin (sanitizers, etc.)

Bionic headers as of commit a9713035baecf21f607ef81c8652eb344086966c misses definition for in\_addr\_t in its headers. It is possible that the Linux headers are expected to define this, but android\_kernel\_huawei\_angler did not define this. Apply [this patch](https://gist.github.com/KireinaHoro/141d27321b2aab27fa8292b1bd0f7105) on Bionic headers to continue.

Configure options to note:

- `CMAKE_INSTALL_PREFIX=$PREFIX/lib/clang/7.0.0`: Install to Clang "resource path" so that libraries can be automatically found by Clang while compiling.
- `COMPILER_RT_BUILD_BUILTINS=OFF` and `COMPILER_RT_BUILD_*=ON`: Build components other than builtins as we have libc++ now.

## Testing

C source file:

`user $``/usr/local/aarch64-linux-android/bin/clang hello.c -o hello`
C++ source file:

`user $``/usr/local/aarch64-linux-android/bin/clang++ hello.cc -o hello --stdlib=libc++ --rtlib=compiler-rt`
C++ source file with sanitizers (ubsan as an example):

`user $``/usr/local/aarch64-linux-android/bin/clang++ hello.cc -o hello --stdlib=libc++ -fsanitize=undefined -static-libsan --rtlib=compiler-rt`
