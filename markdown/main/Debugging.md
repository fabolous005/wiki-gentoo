<!-- source: https://wiki.gentoo.org/wiki/Debugging | group: Gentoo Wiki (Main) | wiki-title: Debugging -->
---
title: Debugging
url: https://wiki.gentoo.org/wiki/Debugging
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: e2933b8a2d7721ec
license: CC BY-SA 4.0
---

# Debugging

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides general advice on how to enable **debugging** symbols and information.

## Installing debugging information for packages

Debug information is stripped by default to save space. This article explains how to enable it either selectively (preferred) or globally (expensive).

### Per-package

Portage has several related feature flags:

- To install debugging information, use the `splitdebug` feature.
- To install the source code, activate the `installsources` feature, which requires [dev-util/debugedit](https://packages.gentoo.org/packages/dev-util/debugedit).

These features are integrated to provide an environment where [dev-debug/gdb](https://packages.gentoo.org/packages/dev-debug/gdb) can find both the debugging information and the sources, allowing full use of its interactive debugging functionality.

If `nostrip` is in the default [FEATURES](https://wiki.gentoo.org/wiki/FEATURES), `splitdebug` won't do anything, so disable it when using splitdebug.

#### Setup

Create two files in /etc/portage/env:

```
CFLAGS="${CFLAGS} -ggdb3"
CXXFLAGS="${CXXFLAGS} -ggdb3"
LDFLAGS="${LDFLAGS} -ggdb3"
# nostrip is disabled here because it negates splitdebug
FEATURES="${FEATURES} splitdebug compressdebug -nostrip"
```
```
FEATURES="${FEATURES} installsources"
```
`root #``emerge --ask dev-util/debugedit dev-debug/gdb`
Then configure packages as required to use the newly-created environment (env) snippets above, depending on whether the source code and/or debugging symbols are needed:

```
# Example package where you only want debug symbols
category/some-package debugsyms
# Example package where you want debug symbols and nice source lines in the debugger
# If in doubt, choose this one!
category/some-library debugsyms installsources
```
Now rebuild the package(s) that were configured in /etc/portage/package.env to get debug symbols:

`root #``emerge --ask --oneshot category/some-package category/some-library`
#### Example of getting a backtrace



#### Example of it working

Now, when debugging a program with gdb, it will find the sources and debugging information.

[Gdb](https://wiki.gentoo.org/wiki/Gdb) finds debug symbols using the paths specified by [Separate Debug Files](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Separate-Debug-Files.html) and the `.gnu_debuglink` section of the ELF binary (try objdump -g \<file>).

The file containing debug symbols must be readable for the user running the debugger. Often debug symbols are readable only to `root`. If for example gdb runs as a non-privileged user (say, because the debugged program runs under X, and the X server is not running as a privileged process; or, perhaps the program is untrusted for privileged access), it may be necessary to adjust permissions:

`root #``chmod a+r /usr/lib/debug/…`
### Don't strip symbols globally

It's also possible to keep debugging information for [Executable and Linkable Format (ELF)](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format) files via the `nostrip` [FEATURE](https://wiki.gentoo.org/wiki/FEATURES). This will lead to larger binaries on the system and may impact startup times slightly, as the debug information is stored within the main executable.

That way, all functions in the code will keep their name and [sys-devel/gdb](https://packages.gentoo.org/packages/sys-devel/gdb) can then show names in a backtrace instead of function addresses only.

**`/etc/portage/make.conf`**

**Keep all ELF symbols in each binary**

```
# Warning: if doing this system wide, a lower level of debug information like -g may be more appropriate
# to save memory during builds and disk space.
CFLAGS="${CFLAGS} -ggdb3"
CXXFLAGS="${CXXFLAGS} -ggdb3"
LDFLAGS="${LDFLAGS} -ggdb3"
FEATURES="nostrip"
```
### Troubleshooting

1. If /usr/src doesn't appear, check for any errors in build log.

## Compression

`FEATURES="compressdebug"` enables compression of debug symbols in Portage. This may make loading debug info in gdb and such a bit slower but is worth it for most where the disk space saving is valuable. The default compression algorithm is zlib.

Newer versions of the toolchain can use [zstd if configured](https://wiki.gentoo.org/wiki/Zstd#Debug_symbols) for a better compression ratio.

[sys-devel/dwz](https://packages.gentoo.org/packages/sys-devel/dwz) can also be used to optimize debugging info size with >=sys-apps/portage-3.0.62 with `FEATURES="dedupdebug"`.

## Valgrind

See [Valgrind](https://wiki.gentoo.org/wiki/Valgrind), a dynamic analysis tool which detects memory errors and memory leaks.

## Eclass debugging

To enable eclass debugging, set the following in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

```
ECLASS_DEBUG_OUTPUT=on
```
## See also

- [GDB](https://wiki.gentoo.org/wiki/GDB) — used to investigate runtime errors that normally involve memory corruption
- [AddressSanitizer](https://wiki.gentoo.org/wiki/AddressSanitizer) — a compiler feature in [GCC](https://wiki.gentoo.org/wiki/GCC) and [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) that is able to detect several memory access errors.
- [UndefinedBehaviorSanitizer](https://wiki.gentoo.org/wiki/UndefinedBehaviorSanitizer) — a compiler feature in [GCC](https://wiki.gentoo.org/wiki/GCC) and [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) that is able to detect various forms of undefined behaviour (UB).
- [Valgrind](https://wiki.gentoo.org/wiki/Valgrind) — dynamic analysis tool which detects memory errors and memory leaks.
- [Stack smashing debugging guide](https://wiki.gentoo.org/wiki/Stack_smashing_debugging_guide) — a step-by-step guide to debug stack smashing violations
- [Debuginfod](https://wiki.gentoo.org/wiki/Debuginfod)
- [Project:Quality Assurance/Backtraces 1.4 debug USE flag](https://wiki.gentoo.org/wiki/Project:Quality_Assurance/Backtraces#debug_USE_flag)
