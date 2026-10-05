<!-- source: https://wiki.gentoo.org/wiki/Gforth | group: Gentoo Wiki (Main) | wiki-title: Gforth -->
---
title: Gforth
url: https://wiki.gentoo.org/wiki/Gforth
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-10"
fingerprint: a206584a421bdaee
license: CC BY-SA 4.0
---

# Gforth

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GNU Forth** is a fast and portable implementation of the ANSI [Forth](https://wiki.gentoo.org/wiki/Forth) language.

Forth is a low-level language being composed of a stack based virtual machine that is only slightly more abstract than hand written assembly language. It is a common task assigned to computer science undergraduates to write a Forth implementation from scratch in assembler. This is often done on microcontrollers with too few registers to easily support C, such as the MOS 6502.

Because it is designed to be portable, GNU's implementation of Forth, gforth, is written in portable ANSI C.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-lang/gforth`
### Environment variables

- `GFORTHHIST` – specifies the location of *.gforth-history*. Default: `$HOME`.
- `GFORTHPATH` – specifies the location of Forth source files.
- `LANG` – see LC\_CTYPE
- `LC_ALL` – see LC\_CTYPE
- `LC_CTYPE` – By default this is set to UTF-8, otherwise an 8-bit encoding is used. If *LC\_CTYPE* is unset, Gforth checks LC\_ALL and LANG (in that order) for character encoding information.

### Files

- \~/.gforth-history — Per user configuration file (default).

### Invocation

`user $``cmd --help`
gforth --help
Usage: gforth \[engine options\] \['--'\] \[image arguments\]
Engine Options:
  --appl-image FILE		    Equivalent to '--image-file=FILE --'
  --clear-dictionary		    Initialize the dictionary with 0 bytes
  -d SIZE, --data-stack-size=SIZE   Specify data stack size
  --debug			    Print debugging information during startup
  --diag			    Print diagnostic information during startup
  --die-on-signal		    Exit instead of THROWing some signals
  --dynamic			    Use dynamic native code
  -f SIZE, --fp-stack-size=SIZE	    Specify floating point stack size
  -h, --help			    Print this message and exit
  --ignore-async-signals	    Ignore instead of THROWing async. signals
  -i FILE, --image-file=FILE	    Use image FILE instead of \`gforth.fi'
  -l SIZE, --locals-stack-size=SIZE Specify locals stack size
  -m SIZE, --dictionary-size=SIZE   Specify Forth dictionary size
  --no-dynamic			    Use only statically compiled primitives
  --no-offset-im		    Load image at normal position
  --no-super			    No dynamically formed superinstructions
  --offset-image		    Load image at a different position
  -p PATH, --path=PATH		    Search path for finding image and sources
  --print-metrics		    Print some code generation metrics on exit
  --print-sequences		    Print primitive sequences for optimization
  -r SIZE, --return-stack-size=SIZE Specify return stack size
  --ss-greedy			    Greedy, not optimal superinst selection
  --ss-min-codesize		    Select superinsts for smallest native code
  --ss-min-ls			    Minimize loads and stores
  --ss-min-lsu			    Minimize loads, stores, and pointer updates
  --ss-min-nexts		    Minimize the number of static superinsts
  --ss-number=N			    Use N static superinsts (default max)
  --ss-states=N			    N states for stack caching (default max)
  --tpa-noequiv			    Automaton without state equivalence
  --tpa-noautomaton		    Dynamic programming only
  --tpa-trace			    Report new states etc.
  -v, --version			    Print engine version and exit
  --vm-commit			    Use OS default for memory overcommit
SIZE arguments consist of an integer followed by a unit. The unit can be
  \`b' (byte), \`e' (element; default), \`k' (KB), \`M' (MB), \`G' (GB) or \`T' (TB).
Image Options:
  FILE				    load FILE (with \`require')
  -e STRING, --evaluate STRING      interpret STRING (with \`EVALUATE')
Report bugs on \<https://savannah.gnu.org/bugs/?func=addbug&group=gforth>

## Troubleshooting

### Gforth isn't recognizing my shabang line

Gforth has a slightly weird shabang syntax compared to other interpreters.

The Gforth interpreter treats the shabang as a special type of comment and accepts the executable path that follows as an argument. To pull this off, it requires a space between #! and the path to the Gforth executable. Consequently, the shabang line has to look something like this:

**`example.fs`**

**A proper Gforth shabang line**

### Defining new words uexpectedly results in an *undefined word* Error

This is a very common mistake for those new to Forth. A space must exist between the : and the start of the word definition. Similarly, after the word definition ends there must be a space prior to the ; which ends the word. Thus, a valid word definition might look like this:

: foo \<the logic of foo> ;

The is a consequence of the fact that the space character delineates tokens in Forth.

### How do I write one-liner style code in Gforth?

Gforth supports this functionality with the `-e` switch:

`user $``gforth -e '10 10 * . bye'`
100

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-lang/gforth`
## External resources

- [Learn Forth in Y Minutes](https://learnxinyminutes.com/docs/forth/).
- [DurexForth](https://github.com/jkotlinski/durexforth) a modern Forth in active development targeting the [Commodore 64](https://en.wikipedia.org/wiki/Commodore_64).
