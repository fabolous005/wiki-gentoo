<!-- source: https://wiki.gentoo.org/wiki/C | group: Gentoo Wiki (Main) | wiki-title: C -->
---
title: C
url: https://wiki.gentoo.org/wiki/C
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-01"
fingerprint: "94a27e9f42b733d4"
license: CC BY-SA 4.0
---

# C

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**C** is a programming language developed for Bell Labs in the early 1970s. C is an imperative and procedural language that supports structured programming. It was designed to be portable across architectures instead of requiring the programmer to reimplement entire programs in the target architecture's native instruction set. At the time, this was accomplished either via a purpose-built assembler — typically supporting macros, comments and other convenient features — or by hand directly in machine language with a minimalist hex editor with no abstraction whatsoever.

This freed the programmer to think abstractly about solving the problem at hand while most of the underlying hardware specifics were abstracted away. From the perspective of scripting languages, such as [BASIC](https://wiki.gentoo.org/wiki/BASIC), [Perl](https://wiki.gentoo.org/wiki/Perl), [Raku](https://wiki.gentoo.org/wiki/Raku), [Python](https://wiki.gentoo.org/wiki/Python), or [Java](https://wiki.gentoo.org/wiki/Java) — all of which have virtual machines or interpreter runtimes — C is considered a low level language. Relative to assembly language, which abstracts individual CPU instructions into short human readable mnemonics, and [Forth](https://wiki.gentoo.org/wiki/Gforth), which has "words" (subroutines) which may directly execute low-level CPU op-codes or directly manipulate the CPU's stack, C is a higher level language.

## Hello world program in C

The following is an example of a hello world program, written in the C programming language.

## Developing in C on Gentoo

A considerable portion of open source software is written in C, most notably the Linux kernel. Gentoo has several C compilers, two of which are installed by default in the Gentoo base-build:

| Name | Package | Description | 
|---|---|---|
| [GCC](https://wiki.gentoo.org/wiki/GCC) | [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) | The GNU Compiler Collection, including C compiler. | 
| [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) | [sys-devel/clang](https://packages.gentoo.org/packages/sys-devel/clang) | The Clang frontend for LLVM. | 

Other C compilers are available in Gentoo, many with specialized use cases:

| Name | Package | Description | 
|---|---|---|
| [CPIK](http://pikdev.free.fr/) | [dev-embedded/cpik](https://packages.gentoo.org/packages/dev-embedded/cpik) | C compiler for PIC18 devices. | 
| [cproc](https://sr.ht/~mcf/cproc/) | [sys-devel/cproc](https://packages.gentoo.org/packages/sys-devel/cproc) | C11 compiler using QBE as backend. | 
| [dev86](https://github.com/lkundrak/dev86) | [sys-devel/dev86](https://packages.gentoo.org/packages/sys-devel/dev86) | C compiler for generating standalone 8086 code. | 
| [PCC](https://en.wikipedia.org/wiki/Portable_C_Compiler) | [dev-lang/pcc](https://packages.gentoo.org/packages/dev-lang/pcc) | The portable C compiler. | 
| [SDCC](https://sdcc.sourceforge.net/) | [dev-embedded/sdcc](https://packages.gentoo.org/packages/dev-embedded/sdcc) | A compiler suite intended for various microprocessors. | 
| [tcc](https://wiki.gentoo.org/wiki/Tcc) | [dev-lang/tcc](https://packages.gentoo.org/packages/dev-lang/tcc) | The tiny C compiler. | 

## Learning C today

There are many free resources to help the novice programmer learn the C language. Stack Overflow has a [list of C books and guides](https://stackoverflow.com/questions/562303/the-definitive-c-book-guide-and-list) which its userbase considers definitive. Additional resources include:

- [Learn C Build Your Own Lisp](https://buildyourownlisp.com/) teaches C and basic interpreter design, sometimes paired with [Crafting Interpreters](https://craftinginterpreters.com/) for more complete coverage of interpreter design.
- [C Programming](https://en.wikibooks.org/wiki/C_Programming) Wikibook (CC BY-SA 3.0).

## See also

- [Assembly language](https://wiki.gentoo.org/wiki/Assembly_language) — the lowest level of all programming languages, typically represented as a series of CPU architecture specific mnemonics and related operands.
- [Forth](https://wiki.gentoo.org/wiki/Forth) — a heavily stack-oriented self-compiling procedural programming language that is only slightly more abstract than [assembly](https://wiki.gentoo.org/wiki/Assembly_language).
- [Modern C porting](https://wiki.gentoo.org/wiki/Modern_C_porting) — catalogs the requirements that older [C] software must now meet in order to correctly build with modern compilers, and includes explanations and tips on how to **port older codebases to modern C**
- [C++](https://wiki.gentoo.org/wiki/C%2B%2B) — a general-purpose programming language that originated from C
- [Rust](https://wiki.gentoo.org/wiki/Rust) — a  general-purpose, multi-paradigm, compiled, programming language.
