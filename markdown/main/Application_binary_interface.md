<!-- source: https://wiki.gentoo.org/wiki/Application_binary_interface | group: Gentoo Wiki (Main) | wiki-title: Application binary interface -->
---
title: Application binary interface
url: https://wiki.gentoo.org/wiki/Application_binary_interface
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-16"
fingerprint: b5ab340947654fc8
license: CC BY-SA 4.0
---

# Application binary interface

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An **application binary interface** (**ABI**) is an interface exposed by software that is defined for in-process machine code access. It defines the low-level, machine-code-level binding between two or more binary modules, such as an application, a library, and an operating system<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

An ABI is distinct from an **application programming interface** (**API**), which defines access at the source code level. While an API ensures source compatibility, allowing code to be recompiled against a new library version, a stable ABI ensures **binary compatibility**, allowing a compiled application to run with a new library version without being recompiled.

## Aspects defined by an ABI

An ABI is specific to a hardware architecture and typically a programming language or platform. It is a comprehensive specification that covers the following aspects:

- **Data representation:** The size, layout, and alignment of basic data types that the processor can directly access.
- **Calling convention:** The methodology for how functions are invoked and how their arguments and return values are handled. This includes:
  - The use of CPU registers and the call stack for passing parameters.
  - The order in which parameters are passed.
  - The assignment of specific registers for particular function parameters.
  - The responsibility for cleaning up the call stack after a function call (either the caller or the callee).
- **System call interface:** The mechanism and specific instruction numbers an application must use to make system calls to the operating system.
- **Name mangling:** The scheme used by a compiler to encode function names and other symbols within object files, which is necessary to support language features like function overloading.
- **Exception propagation:** The standardized method for handling and propagating exceptions between modules.
- **Object file format:** The structure of the binary object files, program libraries, and executables, such as ELF on Unix-like systems or Portable Executable on Windows.

## ABI examples

- **System V ABI**: A standard ABI for Unix-like systems on various architectures, including [amd64 (x86-64)](https://wiki.gentoo.org/wiki/AMD64), various flavours of ARM, and [PowerPC](https://wiki.gentoo.org/wiki/PowerPC). It is the basis for the ABIs used by Linux, the BSDs, and macOS.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup><sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>
- **Microsoft x64 ABI:** The standard calling convention and ABI for 64-bit applications on the Windows operating system.<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>
- **ARM ABI:** The standard ABI for applications executing on the ARM architecture.<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>
- **Itanium C++ ABI:**<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup> While the Itanium architecture was never common (and is now dead), its C++ ABI was widely adopted and now forms the basis for C++ name mangling and exception handling on most Unix-like platforms.

## See also

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [application binary interface - Wikiedia](https://en.wikipedia.org/wiki/Application_binary_interface). Retrieved July 16, 2025.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [System V ABI - OSDev Wiki](https://wiki.osdev.org/System_V_ABI). Retrieved July 16, 2025.
3. [↑](https://wiki.gentoo.org#cite_ref-3) [System V Application Binary Interface, AMD64 Architecture Processor Supplement](https://refspecs.linuxbase.org/elf/x86_64-abi-0.99.pdf). Retrieved July 16, 2025.
4. [↑](https://wiki.gentoo.org#cite_ref-4) [x64 Calling Convention](https://learn.microsoft.com/en-us/cpp/build/x64-calling-convention) . *Microsoft*. Retrieved July 16, 2025.
5. [↑](https://wiki.gentoo.org#cite_ref-5) [Application Binary Interface for the Arm® Architecture](https://github.com/ARM-software/abi-aa) . *GitHub*. Retrieved July 16, 2025.
6. [↑](https://wiki.gentoo.org#cite_ref-6) [Itanium C++ ABI](https://itanium-cxx-abi.github.io/cxx-abi/). *GitHub*. Retrieved July 16, 2025.
