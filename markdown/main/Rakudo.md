<!-- source: https://wiki.gentoo.org/wiki/Rakudo | group: Gentoo Wiki (Main) | wiki-title: Rakudo -->
---
title: Rakudo
url: https://wiki.gentoo.org/wiki/Rakudo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-31"
fingerprint: fe0fb80d53e3020e
license: CC BY-SA 4.0
---

# Rakudo

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Rakudo** is a compiler that implements the [Raku](https://wiki.gentoo.org/wiki/Raku) programming language. Rakudo targets Raku's native virtual machine [MoarVM](https://wiki.gentoo.org/wiki/MoarVM) as well as the [Java](https://wiki.gentoo.org/wiki/Java) and [JavaScript](https://wiki.gentoo.org/index.php?title=JavaScript&action=edit&redlink=1) virtual machines.

## Installation

### USE flags


### Emerge

Emerge the package base

`root #``emerge --ask dev-lang/rakudo`
## Configuration

### Environment variables

#### Module loading

- `RAKUDOLIB` (str) a comma-delimited path list for Raku modules.
- `RAKUDO_MODULE_DEBUG` (bool) If true, extra debugging information is sent to STDERR.

#### Error handling

- `RAKU_EXCEPTIONS_HANDLER` (str) define the exception handling class, defaults to Exceptions::JSON if undefined.
- `RAKUDO_NO_DEPRECATIONS` (bool) If true, suppresses warnings when deprecated language features are used.
- `RAKUDO_DEPRECATIONS_FATAL` (bool) If true, use of deprecated language features become fatal errors.
- `RAKUDO_VERBOSE_STACKFRAME` (int) If true, provides stack frame information for debugging purposes out to a maximum specified number of lines of context.
- `RAKUDO_BACKTRACE_SETTING` (bool) If true, .setting files are included in stack traces.

#### Precompilation

- `RAKUDO_PREFIX` (str) When set, this will cause Raku to look for module repositories in a specified alternative location.
- `RAKUDO_LOG_PRECOMP` (bool) If true, diagnostic information is emitted regarding Raku's precompilation process.

#### Line editor

- `RAKUDO_LINE_EDITOR` (str) When set, this specifies the default line editor for Raku to use. When set to none Raku will not complain about the absence of a line editor. Currently, either Readline or Linenoise are expected values.
- `RAKUDO_DISABLE_MULTILINE` (bool) disable multi-line input when Raku is in interactive mode.
- `RAKUDO_HIST` (str) specifies the location of Raku's line editor history.

#### Miscellaneous

- `RAKUDO_OPT` (str) set default command line options.
- `RAKUDO_DEFAULT_READ_ELEMS` (int) When set, this defines the number of characters read by an IO::Handle.
- `RAKUDO_ERROR_COLOR` (bool) Controls whether compiler error output is color coded or not; defaults to true in POSIX environments.
- `RAKUDO_MAX_THREADS` (int) Controls the maximum number of threads created by ThreadPoolScheduler; default 64.
- `TMPDIR` (str) When set IO::Spec::Unix.tmpdir uses the specified alternative temporary directory; defaults to /tmp.
- `RAKUDO_SNAPPER` (float) Specifies the interval between virtual machine state snapshots created locally by the Rakudo compiler's telemetry class. This defaults to 0.1 or 10 snapshots per second.
- `RAKUDO_HOME` (str) override Raku's installation path.
- `NQP_HOME` (str) override NQP's installation path.

### Files

- \~/.raku/rakudo-history Raku's history file used by the line editor when Raku is run interactively.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-lang/rakudo`
## See Also

- [Raku](https://wiki.gentoo.org/wiki/Raku) — a high-level, general-purpose, and gradually typed programming language with low boilerplate objects, optionally immutable data structures, and an advanced macro system.
- [NQP](https://wiki.gentoo.org/wiki/NQP) — a lightweight [Raku](https://wiki.gentoo.org/wiki/Raku)-like environment for MoarVM, JVM, and other virtual machines.
- [MoarVM](https://wiki.gentoo.org/wiki/MoarVM) — [Rakudo] compiler's virtual machine for the [Raku](https://wiki.gentoo.org/wiki/Raku) Programming Language.
- [Zef](https://wiki.gentoo.org/index.php?title=Zef&action=edit&redlink=1)
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.

## External Resources

- [https://raku.guide](https://raku.guide), a quick overview of the Raku programming language.
- [https://docs.raku.org/programs/03-environment-variables/](https://docs.raku.org/programs/03-environment-variables/)
