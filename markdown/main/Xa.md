<!-- source: https://wiki.gentoo.org/wiki/Xa | group: Gentoo Wiki (Main) | wiki-title: Xa -->
---
title: xa
url: https://wiki.gentoo.org/wiki/Xa
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-15"
fingerprint: d488de193bb70ad4
license: CC BY-SA 4.0
---

# xa

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**xa** is a high-speed, two-pass portable cross-assembler for the 6502 CPU with a C-like preprocessor. One of several popular 65xx assemblers, xa is written in C and released under the GPL-2. It has been in continuous development since 1989.

Other tools in the xa package are:

- file65 — a tool for printing information about o65 object files.
- ldo65 — a linker for o65 object files.
- printcbm — a simple CBM BASIC detokenizer similar to the far more powerful petcat proviced by [VICE](https://wiki.gentoo.org/wiki/VICE).
- reloc65 — a relocator for o65 object files.
- uncpk — a c64 cpk archive manager.

While xa does not currently have a dissassembler as part of its suite, dxa is currently under development separately and will be merged into xa once it becomes stable.

## Installation

### Emerge

`root #``emerge --ask dev-embedded/xa`
## Usage

### Invocation

`user $``xa --help````
Usage: xa [options] file
Cross-assembler for 65xx/R65C02/65816
 -v           verbose output
 -C           no CMOS-opcodes
 -W           no 65816-opcodes (default)
 -w           allow 65816-opcodes
 -B           show lines with block open/close
 -c           produce `o65' object instead of executable files (i.e. don't link)
 -o filename  sets output filename, default is `a.o65'
                A filename of `-' sets stdout as output file
 -e filename  sets errorlog filename, default is none
 -l filename  sets labellist filename, default is none
 -r           adds crossreference list to labellist (if `-l' given)
 -M           allow ``:'' to appear in comments for MASM compatibility
 -R           start assembler in relocating mode
 -Llabel      defines `label' as absolute, undefined label even when linking
 -b? addr     set segment base address to integer value addr
                `?' stands for t(ext), d(ata), b(ss) and z(ero) segment
                (address can be given more than once, last one is used)
 -A addr      make text segment start at an address that when the _file_
                starts at addr, relocation is not necessary. Overrides -bt
                Other segments must be specified with `-b?'
 -G           suppress list of exported globals
 -p?          set preprocessor character to ?, default is #
 -DDEF=TEXT   defines a preprocessor replacement
 -Ocharset    set output charset (PETSCII, ASCII, etc.), case-sensitive
 -Idir        add directory `dir' to include path (before XAINPUT)
  --version   output version information and exit
  --help      display this help and exit
== These options are deprecated and will be removed in 2.4+! ==
 -x           old filename behaviour (overrides `-o', `-e', `-l')
 -S           allow preprocessor substitution within strings
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-embedded/xa`
## See also

- [VICE](https://wiki.gentoo.org/wiki/VICE) — the **V**ersat**I**le **C**ommodore **E**mulator for Commodore computers of the 8-bit era

## External resources

- [cc65](https://cc65.github.io/) — a powerful [C](https://wiki.gentoo.org/wiki/C) cross compiler targeting most known 65*xx*-based computers.
- [64tass](http://tass64.sourceforge.net/) — an expressive 6502 assembler with a TASM-like syntax.
- [Codebase64](https://codebase64.org/) — a wiki devoted to C64 development.
- [C64 Wiki](https://www.c64-wiki.com/) — a wiki devoted the Commodore 64 more generally.
