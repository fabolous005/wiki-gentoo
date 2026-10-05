<!-- source: https://wiki.gentoo.org/wiki/Modern_C_porting | group: Gentoo Wiki (Main) | wiki-title: Modern C porting -->
---
title: Modern C porting
url: https://wiki.gentoo.org/wiki/Modern_C_porting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-05"
fingerprint: e651fa116a65b3a0
license: CC BY-SA 4.0
---

# Modern C porting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This document catalogs the requirements that older [C](https://wiki.gentoo.org/wiki/C) software must now meet in order to correctly build with modern compilers, and includes explanations and tips on how to **port older codebases to modern C**.

Compilers have historically tolerated certain non-standards-conformant code or allowed various problematic constructs that would be rejected by current compilers, that now impose stricter requirements.

Fixes will be required to be able to build codebases containing any such non-conformant code on [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) 16 or [GCC](https://wiki.gentoo.org/wiki/GCC) 14, for example.

With [GCC 15](https://gcc.gnu.org/gcc-15/changes.html) set to adopt [C23](<https://en.wikipedia.org/wiki/C23_(C_standard_revision)>) (the current standard for the C programming language), further steps may be need to ensure that builds continue to function with this new compiler - see the C23 section below.

**Todo:**

- mention GNU\_SOURCE and other FTMs

## What has changed and why it matters

Problematic constructs and invalid C99 or C89 that were tolerated to avoid disrupting developer processes (usually simply triggering warnings that could be dealt with later date) are now considered errors that abort compilation. **Lots of packages that might contain the problematic constructs will now fail to build** until the offending sections are fixed.

Many, many of these failures indicate real runtime problems including crashes, memory corruption, or security issues, so fixing these codebases to work with the new compiler constraints is very useful work.

### Requirements as of now

These are the compiler default configuration changes that will already impact codebases that used problematic constructs:

- Clang 15 now reports the following as errors, by default:
  - `-Werror=int-conversion`

- Clang 16 (released March 2023) now reports the following as errors, by default:
  - `-Werror=implicit-function-declaration`
  - `-Werror=implicit-int`
  - `-Werror=incompatible-function-pointer-types` (GCC does not have a specific equivalent error ([PR109835](https://gcc.gnu.org/PR109835)), use `-Werror=incompatible-pointer-types` instead when testing)

- GCC 14 (released May 2024) [now reports the following as errors, by default](https://inbox.sourceware.org/gcc-patches/cover.1700473918.git.fweimer@redhat.com/):
  - `-Werror=int-conversion`
  - `-Werror=implicit-function-declaration`
  - `-Werror=implicit-int`
  - `-Werror=incompatible-pointer-types`
  - `-Werror=return-mismatch` ('new' warning in GCC 14, split out from `-Wreturn-type`; Clang 19 adds this too)
  - `-Werror=declaration-missing-parameter-type` (new warning in GCC 14)

- GCC 15 (released April 2025) makes [-std=gnu23](https://gcc.gnu.org/onlinedocs/gcc/C-Dialect-Options.html) the default (from `-std=gnu17`). C23 makes additional changes like removing unprototyped functions:
  - Clang has `-Wdeprecated-non-prototype` for this
  - GCC 15 [will also have this warning](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=108694)
  - Older GCC versions have `-Wstrict-prototypes` (Clang has this too)

### Examples

- SDL thought no joystick support existed ([https://github.com/libsdl-org/SDL/pull/6217](https://github.com/libsdl-org/SDL/pull/6217))
- [app-shells/zsh](https://packages.gentoo.org/packages/app-shells/zsh) stopped installing any extensions and would hang at runtime - [bug #869539](https://bugs.gentoo.org/show_bug.cgi?id=869539)
- [dev-libs/apr](https://packages.gentoo.org/packages/dev-libs/apr) would install but cause [dev-libs/apr-util](https://packages.gentoo.org/packages/dev-libs/apr-util) compilation to loop forever - [bug #870004](https://bugs.gentoo.org/show_bug.cgi?id=870004)
- Bash's *printf* misbehaved - [bug #935411](https://bugs.gentoo.org/show_bug.cgi?id=935411) (although it was fixed in Gentoo long before GCC 14 was released)
  - *Note*: there was another bug here in that the fallback implementation seems to have malfunctioned (fixed [upstream](https://git.savannah.gnu.org/cgit/bash.git/commit/?id=37b7e91d64ad10b1a1815d12128c9475636df670))

## General procedure

Some pointers when fixing these errors:

- Read the compiler errors carefully.
- File a bug upstream if an issue cannot be fixed for now (even just because of time constraints) - it informs them of the need to work on it.
- Check what other distributions did if unsure.

## Fixes (C99)

### For Gentoo packages

For Gentoo packages, **all of these fixes require a new revision** ([revbump](https://devmanual.gentoo.org/general-concepts/ebuild-revisions/index.html)) - they concern potential breakages so package rebuilds should be propagated. A new revision will help developers quickly notice if the fix is somehow insufficient, to weed out any problems.

#### What if an un-last-ritable package is hopelessly broken?

For most packages, this is *not* the case. But occasionally, there are indeed core packages which are unmaintained upstream, have a broken codebase, and there's seemingly no alternatives around. Some possibilities to consider for such packages:

- Assess whether other distributions have patches that can be borrowed
- Consider forking the package and collaborating with other distributions
- Investigate possible replacements/alternatives
- Pass `-std=gnu89 -fno-strict-aliasing` and `filter-lto` (with GCC 14, `-fpermissive` is also an option)

### -Wimplicit-function-declaration

- GCC will usually helpfully emit a 'fixit' (an annotation to the warning/error with the missing header).
- Add the relevant `#include` - determine this possibly by looking at man pages for the missing functions, or grepping in the codebase
- Internal functions
  - grep the codebase for uses of the function to determine the correct return type.
  - Sometimes packages are just missing includes for their own internal functions
  - Sometimes adding a prototype into an internal header is needed

### -Wimplicit-int

- grep the codebase for uses of the function to determine the correct return type.
- Do not assume it is supposed to be an int.

### -Wint-conversion

- Often missing padding members. Use C99's designated initializers instead.

Examples:

### -Wincompatible-pointer-types

This includes `-Wincompatible-function-pointer-types` which is a Clang-specific subset of `-Wincompatible-pointer-types`.

- Casting
  - **Do not** simply cast to the "other side" of the error. Casts will silence warnings/errors, but that does not mean the cast is correct.
  - They will always "work" at compile-time, but that doesn't make them correct or do the right thing.
  - By casting, a real problem may be being obscured!
  - It's possible there's e.g. a typo in the variables instead, or a variable needs to be split into two instead of reused for another type.
  - Casting should be the the last resort, if it's even needed at all, after verifying what the intended type should be.
- grep the codebase for uses of the function to determine the correct return type.
- This can often be somewhat convoluted and may require filling in various prototypes both to head off possible C23 issues but also to make the compiler give better errors
  - Sometimes, to get a better understanding of what is wrong, it's useful to temporarily put in the *wrong* type just to get a better error, rather than *no* type (obviously not for the patch to be committed, just for diagnostic purposes)
- These bugs are the hardest to solve and often require understanding the intent of the software's author. It's okay to feel stuck with these.
- Many of these end up being last-rite candidates because they're abandoned upstream and have other code smells.
- It's not always possible (or at least practical) to determine the correct types if the codebase is particularly old because they relied on ambiguity.
  - In some extreme cases where a code generator is broken like Cython or Vala, it *may* be okay to pass `-Wno-error=incompatible-pointer-types`, but please avoid it.
  - If doing this, make sure there's an upstream report, or if upstream is gone, that there's truly no alternative to this software available (so we can last-rite).

### Configure tests

This is a mixed bag. Rich Felker (dalias), the musl author, [wrote](https://ewontfix.com/13/) about the primary issue here on his blog in 2013. This problem predominantly affects the autoconf build system but it is **not** exclusive to it: such bugs have occurred in CMake and Meson checks too.

configure tests which gave a "yes" or "present" result before may now give "no" or "absent" because the changed compiler behavior causes them to be confused.

There are several cases:

- Some tests will legitimately have an implicit function declaration error when they're *working as intended* because the function in question genuinely **doesn't exist** on the system. For example, `memset_s` was part of Annex K in the C standard and was never implemented by glibc. There is no header to include to fix that, nor will defining a *Feature Test Macro* (FTM) help. It is OK to ignore these and define `QA_CONFIG_IMPL_DECL_SKIP=( memset_s )` in the ebuild to silence the QA warning.

- Other tests are broken because they check for a function like `malloc` (or check its *behavior*) without including `stdlib.h`. These should be fixed by adding the needed includes.

- Tests might legitimately check for two versions of an API, like `strerror_r` (POSIX) vs. `strerror_r` (GNU). It is OK if *one* of these fails with an incompatible-pointer-types error, but not *both*. Always check the full context for a failing test.

- Tests might have never even worked as intended, by e.g. passing an integer to a check for *pthread\_create* when a pointer was required. They may have always failed or always succeeded - but not tested the property they were intending, even before these strict default changes.

Take extreme care and consider diffing *config.log* before/after fixes and possibly with relaxed CFLAGS vs strict/default CFLAGS.

Sometimes a confused configure test will lead to the build being misconfigured but "succeeding" (installing the wrong contents or with broken or missing functionality), and sometimes a confused configure test will lead to the build failing later on in a mysterious way (nonsensical error or similar).

## Fixes (C23)

- C23 drops unprototyped functions: `int foo()` is now equivalent to `int foo(void)` in some contexts (i.e. *foo* takes no arguments). This is the most disruptive change.
- C23 introduces `bool` as a proper keyword which may conflict with custom typedefs in projects.

The prototypes change sometimes exposes real bugs where a function was used inconsistently across translation units (TUs), although our [LTO efforts](https://wiki.gentoo.org/wiki/Project:Toolchain/LTO) have smoked some of those out already.

When working on these fixes, use `-Wmissing-parameter-name` with GCC to avoid accidentally introducing C23isms into upstream patches. GCC accepted 'void z (int)' where 'int' is unused (so no need to name it) as a GNU extension for all language modes from GCC 11 onwards.

### bool

C99 introduced a real Boolean type called *\_Bool* and provided macros defining *bool*, *true*,  and *false*. It was made available via `<stdbool.h>`. In C23, it was promoted to being always available as *bool* (i.e. it is no longer opt-in).

This breaks some projects which had their own compatibility layers to provide bool for pre-C99 compliers. Often, these typedefs can simply be removed.

The errors often look like the following ([PR117629](https://gcc.gnu.org/PR117629) tracks improving this):

`user $``gcc foo.c````
error: two or more data types in declaration specifiers
  113 | typedef int bool;
      |             ^~~~
```
Beware of changing this naively, though. If the project's custom bool type appeared on an ABI boundary or was serialized to disk, the types cannot be swapped out, as a naive custom bool implemented as int is not the same as the \_Bool type. A good write-up on this can be found at [https://blog.svgames.pl/article/the-little-bool-of-doom](https://blog.svgames.pl/article/the-little-bool-of-doom).

### Unprototyped functions

C23 removes support for unprototyped functions. Function definitions need to match the prototype. Often, the prototypes are wrong and declare no arguments with "()" rather than "(int a, int b)" or whatever the function really gets passed. See [https://lists.fedoraproject.org/archives/list/devel@lists.fedoraproject.org/message/VJLMEHISU4BVJVMM3RPPW3SNTXPDATKX/](https://lists.fedoraproject.org/archives/list/devel@lists.fedoraproject.org/message/VJLMEHISU4BVJVMM3RPPW3SNTXPDATKX/).

The prototype below for *foo* is wrong:

We can see that *foo* actually takes one argument of type *int*, therefore the fixed version should be:

The same is true for function pointers. The prototype below for *call\_a\_function* is wrong:

We can see that *function* actually takes one argument of type *int*, therefore the fixed version should be:

While use of unprototyped functions is sloppy as it can lead to bugs being missed across TUs, it's acceptable for our purposes to build with `-std=gnu17` (which was the previous default) as long as an upstream bug has been filed.

#### -Wdeprecated-non-prototypes, -Wstrict-prototypes

There's some nuance with these warnings and what precisely they diagnose, see [PR95445](https://gcc.gnu.org/PR95445) and especially the discussion in [PR108694](https://gcc.gnu.org/PR108694).

- Replace "()" with the actual types.
- Add `-std=gnu17` instead in CFLAGS in the ebuild if you don't fix them.

### -Wold-style-declarations

- Convert to ISO C declarations.
- Add `-std=gnu17` instead in CFLAGS in the ebuild if you don't fix them.

#### Fixing K&R C declarations with cproto

Often errors are caused by old K&R style function definitions. So this:

needs to be reworked into this:

This is not a very hard task, but it becomes exhausting when doing this for a larger project.

[dev-util/cproto](https://packages.gentoo.org/packages/dev-util/cproto) can automate this. For a given file, myCfile, cproto will convert (and return the prototypes of all functions it can find) with

`user $``cproto -a myCfile.c`
Or for all the .c-files in a project:

`user $``find ./ -name "*.c*" | xargs cproto -a`
## FAQ

### Where can I find a list of Gentoo bugs to hack on?

See [bug #870412](https://bugs.gentoo.org/show_bug.cgi?id=870412) and the list [here.](https://bugs.gentoo.org/showdependencytree.cgi?id=870412&hide_resolved=1)

Additionally, for C23 preparedness (see above), see [bug #880545](https://bugs.gentoo.org/show_bug.cgi?id=880545).

### How do I reproduce these bugs?

#### C23 issues

In general:

1. Use Clang and set `-std=gnu23`
2. Use GCC \<15 and set `-std=gnu23`
3. Use GCC 15

#### C99 issues

In general:

1. Use Clang 16 and set `CC=clang-16`, or
2. Use Clang 15 and set `CC=clang-15` and `=llvm-core/clang-common-15* stricter` in /etc/portage/package.use/clang, or
3. Use GCC \<14 and set `-Werror=implicit-function-declaration -Werror=implicit-int -Werror=int-conversion -Werror=incompatible-pointer-types`
4. Use GCC 14

#### configure or build system bugs

Developers may need to follow the above to setup their environment, run ./configure, then:

- grep config.log, or
- inspect ./configure, or
- check other build system-generated files if the problem does not appear in build.log.

A /etc/portage/bashrc [hook](https://gist.github.com/thesamesam/4ddaa95f3f42c2be312b676476cbf505) is available to save logs in /var/tmp/clang to help capture issues from homebrew configure scripts which do not log. In order to use this without root rights with the ebuild command, make sure that users have writing privileges for /var/tmp/clang.

### Is this cosmetic?

**No!**

Implicit function declarations can affect code generation. They've been a long-standing cause of runtime failures like crashes. They are particularly a problem if the calling convention for an architecture is sufficiently "different", e.g. Apple's ARM64 ABI.

Even on amd64, it can cause problems: if a function returns a `_Bool` in reality but the prototype is missing, the compiler will assume `int`. On amd64, this causes messy corruption because there's no obligation for a `_Bool` to have filled the remaining bits correctly.

Another issue is missing attributes and aliases. `_FORTIFY_SOURCE` cannot be effective with implicit function declarations, nor can redirects for `time_t` on 32-bit platforms for e.g. `openat`->`openat64`.

A **new revision** of the ebuild is required for fixing these bugs because of the possible runtime effects.

### Do I have to send patches upstream?

- If upstream still exists, yes, *please* do. We need other distributions to do the same as well. This is a huge task and we can't be needlessly duplicating work. It's also just part of being a good FOSS citizen, of course.
- If upstream is completely gone, of course, you need not feel guilt.

## Tips & Tricks

### Using Clang on a package-basis

Clang can be used only for specific packages by leveraging Portage's package.env mechanism. Files similar to the following should be created.

**`/etc/portage/env/clang_fixes/use_clang.conf`**

**Clang overrides**

```
# Build packages with clang instead of gcc
CC="clang"
CXX="clang++"
AR="llvm-ar"
NM="llvm-nm"
RANLIB="llvm-ranlib"
# Uncomment if you want to use lld. It's optional and not needed for these bugs, but it can help find other problems like underlinking.
#LDFLAGS="${LDFLAGS} -fuse-ld=lld -Wl,--as-needed"
```
**`/etc/portage/package.env/clang_fixes`**

**Tell Portage to use Clang for some package**

```
# Bug 000000
category/package clang_fixes/use_clang.conf
```
Including a link to the relevant bug as a comment in the package.env entry makes it easier to keep track of the context for that package.

### Using Portage to find build system bugs

Portage (as of version 3.0.45.1) will scan the standard configure logs (config.log, CMakeError.log, meson-log.txt) for configure-time implicit function declarations as part of a post-install QA check. Any results that found are given as a QA message as well as logged into qa.log in the package build tree in a script-friendly format.

If the message is a false positive (e.g. BSD-only functions), mark them as such in `QA_CONFIG_IMPL_DECL_SKIP` in the ebuild.

If the message is from tests built in to autoconf (not from the package's own configure.ac or m4 macros), then try `eautoreconf`.

**`/var/tmp/portage/dev-lang/python-3.10.9-r1/temp/build.log`**

**Example QA message**

**`/var/tmp/portage/dev-lang/python-3.10.9-r1/temp/qa.log`**

**Example qa.log**

## See also

- [C](https://wiki.gentoo.org/wiki/C) — a programming language developed for Bell Labs in the early 1970s
- [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) — a C/C++/Objective-C/C++, CUDA, and RenderScript language front-end for the LLVM project
- [GCC](https://wiki.gentoo.org/wiki/GCC) — among the most widely used compiler toolchains in the world with official support for: [C](https://wiki.gentoo.org/wiki/C), [C++](https://wiki.gentoo.org/wiki/C%2B%2B), [Objective-C](https://en.wikipedia.org/wiki/Objective-C), [Objective-C++](https://en.wikipedia.org/wiki/Objective-C%2B%2B), [Modula-2](https://en.wikipedia.org/wiki/Modula-2), [Fortran](https://wiki.gentoo.org/wiki/Fortran), [Ada](https://wiki.gentoo.org/wiki/Ada), [Go](https://wiki.gentoo.org/wiki/Go), [COBOL](https://en.wikipedia.org/wiki/COBOL), and [D](<https://en.wikipedia.org/wiki/D_(programming_language)>)
- [Project:Toolchain/Compiler\_warnings](https://wiki.gentoo.org/wiki/Project:Toolchain/Compiler_warnings)
- [User:Schievel/fixing\_clang16\_errors](https://wiki.gentoo.org/wiki/User:Schievel/fixing_clang16_errors)

## Resources

- [Porting to GCC 14](https://gcc.gnu.org/gcc-14/porting_to.html)
- [Porting to GCC 15](https://gcc.gnu.org/gcc-15/porting_to.html)
- [c-std-porting list](https://lore.kernel.org/c-std-porting/)
- [Modernizing Fedora's C code](https://lwn.net/Articles/913505/)
- [Modern C for Fedora (and the world)](https://lwn.net/Articles/954018/)
- [Clang 16 is coming - and it'll break your packages!](https://archives.gentoo.org/gentoo-dev/message/dd9f2d3082b8b6f8dfbccb0639e6e240)
  - Clang 16 / modern C porting in Gentoo - [bug #870412](https://bugs.gentoo.org/show_bug.cgi?id=870412)
  - C23 porting in Gentoo - [bug #880545](https://bugs.gentoo.org/show_bug.cgi?id=880545)
- [Porting to Modern C (Fedora)](https://fedoraproject.org/wiki/Changes/PortingToModernC)
- [Configure script breakage with the new -Werror=implicit-function-declaration](https://discourse.llvm.org/t/configure-script-breakage-with-the-new-werror-implicit-function-declaration/65213)
- [Unresolved issues from the LLVM 15.x release](https://discourse.llvm.org/t/unresolved-issues-from-the-llvm-15-x-release/66071)
- [LPC 2021 - GNU Tools Track (Eliminating implicit function declarations)](https://www.youtube.com/watch?v=q5itHU2T5xU&t=2687s)
