<!-- source: https://wiki.gentoo.org/wiki/Upgrading_from_gcc-4.x_to_gcc-5.x | group: Gentoo Wiki (Main) | wiki-title: Upgrading from gcc-4.x to gcc-5.x -->
---
title: Upgrading from gcc-4.x to gcc-5.x
url: https://wiki.gentoo.org/wiki/Upgrading_from_gcc-4.x_to_gcc-5.x
hostname: gentoo.org
sitename: Upgrading from gcc-4.x to gcc-5.x
date: "2025-05-04"
fingerprint: b311f269cd3c0b10
license: CC BY-SA 4.0
---

# Upgrading from gcc-4.x to gcc-5.x

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This article deals with instructions for upgrading systems from GCC 4.x to GCC 5.x. It has been deprecated in favor of the more modern [Upgrading GCC](https://wiki.gentoo.org/wiki/Upgrading_GCC) article.

## Critical news

The following ([GLEP 42](https://www.gentoo.org/glep/glep-0042.html)) message has been sent to inform users about the important changes<sup>[\[1\]](https://wiki.gentoo.org#cite_note-GCC_5_Defaults-1)</sup>.

Title: GCC 5 Defaults to the New C++11 ABI
 Author: Mike Frysinger \<vapier@gentoo.org>
 Content-Type: text/plain
 Posted: 2015-10-22
 Revision: 1
 News-Item-Format: 1.0
 Display-If-Installed: >=sys-devel/gcc-5
 
 GCC 5 uses the new C++ ABI by default.  When building new code, you might run
 into link time errors that include lines similar to:
 ...: undefined reference to '\_ZNSt6chrono12steady\_clock3nowEv@GLIBCXX\_3.4.17'
 
 Or you might see linkage failures with "std::\_\_cxx11::string" in the output.
 
 These are signs that you need to rebuild packages using the new C++ ABI.
 You can quickly do so by using revdep-rebuild (from gentoolkit).
 
 For gentoolkit-0.3.1 or higher:
 # revdep-rebuild --library 'libstdc++.so.6' -- --exclude gcc
 
 For previous versions of gentoolkit:
 # revdep-rebuild --library 'libstdc\+\+\.so\.6' -- --exclude gcc
 
 For more details, feel free to peruse:
 https://developerblog.redhat.com/2015/02/05/gcc5-and-the-c11-abi/
 https://blogs.gentoo.org/blueness/2015/03/10/the-c11-abi-incompatibility-problem-in-gentoo/

## For Gentoo users

### C++ changes

In order to be compliant with the C++11 standard, a number of standard template library (STL) types needed to be changed (specifically std::string and std::list in libstdc++). For backwards compatibility the SONAME of libstdc++.so was not bumped, instead, inline namespaces in combination with ABI tags are used now. These necessitate a recompilation of all C++ code, otherwise code crossing file interface boundaries could fail with

### undefined references to std::\_\_cxx11

It is not a bug, if packages fail with undefined references to std::\_\_cxx11 or \[abi:cxx11\] like

cmGlobalGenerator.cxx:(.text+0x12781): undefined reference to \`Json::Value::Value(std::\_\_cxx11::basic\_string\<char, std::char\_traits\<char>, std::allocator\<char> > const&)'

It means you need to **rebuild the package** that provided that symbol first.

See [https://gcc.gnu.org/onlinedocs/libstdc++/manual/using\_dual\_abi.html](https://gcc.gnu.org/onlinedocs/libstdc++/manual/using_dual_abi.html) for more info.

## For Gentoo developers

### Instructions by the GNU project

- [https://gcc.gnu.org/gcc-5/porting\_to.html](https://gcc.gnu.org/gcc-5/porting_to.html) Porting to GCC 5

### C changes

A significant change of GCC 5 was the move from `-std=gnu89` to `-std=gnu11` as default C standard. Many packages in the tree have implicitly relied on the C standard being `-std=gnu89` and now fail with different types of errors:

Missing symbol (`-std=gnu89 inline` emits an externally visible definition, `-std=gnu11 inline` does not):

x86\_64-pc-linux-gnu-gcc -Wl,-O1 -Wl,--as-needed background.o list.o main.o notes.o options.o savegeom.o  -lpangoxft-1.0 -lpangoft2-1.0 -lpango-1.0 -lgobject-2.0 -lglib-2.0 -lfontconfig -lfreetype -lXft -lXrender -lX11 -lXrandr -lXext -o xnots
main.o: In function \`processConfigureNotify.part.3':
main.c:(.text+0x133): undefined reference to \`resetNoteWidth'
savegeom.o: In function \`getGeometryFromList':

Redefinition (permitted by `-std=gnu89 inline`):

argp-eexst.c:(.text+0x0): multiple definition of \`argp\_usage'
../../lib/libmisc.a(argp-help.o):argp-help.c:(.text+0x3590): first defined here
../../lib/libmisc.a(argp-eexst.o): In function \`\_option\_is\_short':

The simplest fix for both cases is restoring pre-GCC 5 inline semantics, i.e., inheriting `flag-o-matic` and adding `-std=gnu89` to the `CFLAGS` variable:

This will lead to the same C language behavior as with GCC 4.9 and below.

### Bugzilla entries about gcc-5.3

- [bug #536984](https://bugs.gentoo.org/show_bug.cgi?id=536984) the Tracker bug **(gcc-5) GCC 5 porting**
- [Open Bugzilla entries regarding gcc-5.3](https://bugs.gentoo.org/buglist.cgi?quicksearch=gcc-5.3&list_id=3005290)

## See also

- [Upgrading GCC](https://wiki.gentoo.org/wiki/Upgrading_GCC) — among the most widely used compiler toolchains in the world with official support for: [C](https://wiki.gentoo.org/wiki/C), [C++](https://wiki.gentoo.org/wiki/C%2B%2B), [Objective-C](https://en.wikipedia.org/wiki/Objective-C), [Objective-C++](https://en.wikipedia.org/wiki/Objective-C%2B%2B), [Modula-2](https://en.wikipedia.org/wiki/Modula-2), [Fortran](https://wiki.gentoo.org/wiki/Fortran), [Ada](https://wiki.gentoo.org/wiki/Ada), [Go](https://wiki.gentoo.org/wiki/Go), [COBOL](https://en.wikipedia.org/wiki/COBOL), and [D](<https://en.wikipedia.org/wiki/D_(programming_language)>)
