<!-- source: https://wiki.gentoo.org/wiki/Awk | group: Gentoo Wiki (Main) | wiki-title: Awk -->
---
title: awk
url: https://wiki.gentoo.org/wiki/Awk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-04"
fingerprint: d70b180c07f33b04
license: CC BY-SA 4.0
---

# awk

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**awk** is a scripting language for data extraction often used in tandem with [sed](https://wiki.gentoo.org/wiki/Sed) and [grep](https://wiki.gentoo.org/wiki/Grep) for complex reporting needs. While large awk programs are possible, the language itself was intended primarily to be used to construct one-liners to filter data and perform simple computations.

The awk utility [is specified by](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/awk.html#tag_20_06) [POSIX](https://wiki.gentoo.org/wiki/POSIX); several versions of awk are provided by Gentoo. A specific implementation may be selected with [Project:Base/Alternatives](https://wiki.gentoo.org/wiki/Project:Base/Alternatives). By default [GNU awk](https://www.gnu.org/software/gawk/), [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk), will be pulled in, and that implementation will be used as an example for this article. However, the behavior required by POSIX can be requested via the `-P`/`--posix` option or the `POSIXLY_CORRECT` environment variable; refer to the [gawk(1)](https://man.archlinux.org/man/gawk.1.en) [man page for further information.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## Installation

Installation usually happens by [Unpacking the stage tarball](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#Unpacking_the_stage_tarball).

### USE flags

The [app-alternatives/awk](https://packages.gentoo.org/packages/app-alternatives/awk) [USE flags](https://wiki.gentoo.org/wiki/USE_flag) select which version of awk to pull in:


### USE flags for
            [app-alternatives/awk](https://packages.gentoo.org/packages/app-alternatives/awk)
            
            /bin/awk and /usr/bin/awk symlinks

By default, [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk) will be pulled in. To use a different implementation of awk, set the appropriate USE flags on [app-alternatives/awk](https://packages.gentoo.org/packages/app-alternatives/awk) in [package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use).

USE flags for [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk):


### USE flags for
            [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk)
            
            GNU awk pattern-matching language

| [+mpfr](https://packages.gentoo.org/useflags/+mpfr) | Use dev-libs/mpfr for high precision arithmetic (-M / --bignum) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pma](https://packages.gentoo.org/useflags/pma) | Experimental Persistent Memory Allocator (PMA) support which allows persistence of variables, arrays, and user-defined functions across runs. | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

Install awk:

`root #``emerge --ask app-alternatives/awk`
## Configuration

### Environment variables

- `AWKPATH` a list of directories searches to find file names passed at runtime with the `--file` option.
- `AWKLIBPATH` a list of directories searches to find file names passed at runtime with the `--load` option.
- `GAWK_READ_TIMEOUT` the amount of time (in milliseconds) awk waits for input before giving up. (for [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk))
- `GAWK_SOCK_RETRIES` the total number of retries when reading data from a socket. (for [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk))
- `GAWK_MSEC_SLEEP` the amount of time (in milliseconds) awk sleeps between retries. (for [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk))
- `POSIXLY_CORRECT` duplicates the `--posix` switch.

## Usage

### Invocation

Invocation information for [sys-apps/gawk](https://packages.gentoo.org/packages/sys-apps/gawk):

`user $``awk --help````
Usage: awk [POSIX or GNU style options] -f progfile [--] file ...
Usage: awk [POSIX or GNU style options] [--] 'program' file ...
POSIX options:		GNU long options: (standard)
	-f progfile		--file=progfile
	-F fs			--field-separator=fs
	-v var=val		--assign=var=val
Short options:		GNU long options: (extensions)
	-b			--characters-as-bytes
	-c			--traditional
	-C			--copyright
	-d[file]		--dump-variables[=file]
	-D[file]		--debug[=file]
	-e 'program-text'	--source='program-text'
	-E file			--exec=file
	-g			--gen-pot
	-h			--help
	-i includefile		--include=includefile
	-I			--trace
	-l library		--load=library
	-L[fatal|invalid|no-ext]	--lint[=fatal|invalid|no-ext]
	-M			--bignum
	-N			--use-lc-numeric
	-n			--non-decimal-data
	-o[file]		--pretty-print[=file]
	-O			--optimize
	-p[file]		--profile[=file]
	-P			--posix
	-r			--re-interval
	-s			--no-optimize
	-S			--sandbox
	-t			--lint-old
	-V			--version
To report bugs, see node `Bugs' in `gawk.info'
which is section `Reporting Problems and Bugs' in the
printed version.  This same information may be found at
https://www.gnu.org/software/gawk/manual/html_node/Bugs.html.
PLEASE do NOT try to report bugs by posting in comp.lang.awk,
or by using a web forum such as Stack Overflow.
gawk is a pattern scanning and processing language.
By default it reads standard input and writes standard output.
Examples:
	awk '{ sum += $1 }; END { print sum }' file
	awk -F: '{ print $1 }' /etc/passwd
```
### Usage in ebuilds

## Removal

Removal is not recomended since awk is a member of [@system](<https://wiki.gentoo.org/wiki/System_set_(Portage)>).

### Unmerge

Uninstall package:

`root #``emerge --ask --depclean --verbose app-alternatives/awk`
## See also

- [ed](https://wiki.gentoo.org/wiki/Ed) — a [line-based](https://en.wikipedia.org/wiki/Line_editor) text editor with support for regular expressions
- [sed](https://wiki.gentoo.org/wiki/Sed) — a program that uses regular expressions to programmatically modify streams of text
- [grep](https://wiki.gentoo.org/wiki/Grep) — a tool for searching text files with regular expressions
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.
- [Raku](https://wiki.gentoo.org/wiki/Raku) — a high-level, general-purpose, and gradually typed programming language with low boilerplate objects, optionally immutable data structures, and an advanced macro system.
