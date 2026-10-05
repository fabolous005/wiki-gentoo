<!-- source: https://wiki.gentoo.org/wiki/Shell/Scripting | group: Gentoo Wiki (Main) | wiki-title: Shell/Scripting -->
---
title: Shell/Scripting
url: https://wiki.gentoo.org/wiki/Shell/Scripting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
fingerprint: b63f180d64b53b08
license: CC BY-SA 4.0
---

# Shell/Scripting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page is a reference guide to **scripting-related differences** between [shells](https://wiki.gentoo.org/wiki/Shell). It is not intended to be a general introduction to shell scripting, either in general or for a particular shell (e.g. [Bash](https://wiki.gentoo.org/wiki/Bash)).

The shells referenced in this page are:

| Name | Package | Notes | 
|---|---|---|
| Bash | [app-shells/bash](https://packages.gentoo.org/packages/app-shells/bash) | The Bourne-Again Shell, first released in 1989; used by Portage. | 
| Dash | [app-shells/dash](https://packages.gentoo.org/packages/app-shells/dash) | Debian Almquist Shell; intended to be POSIX-conformant. | 
| ATT Ksh (ksh93u+m) | [app-shells/ksh](https://packages.gentoo.org/packages/app-shells/ksh) | The original KornShell / Ksh, first released in 1983. | 
| OpenBSD Ksh | [app-shells/loksh](https://packages.gentoo.org/packages/app-shells/loksh) | A Linux port of OpenBSD's Ksh. | 
| BusyBox sh | [sys-apps/busybox](https://packages.gentoo.org/packages/sys-apps/busybox) | The sh applet provided by the BusyBox binary. This page assumes BusyBox has been compiled with the default configuration (e.g. with `ASH_BASH_COMPAT` enabled). | 
| Zsh | [app-shells/zsh](https://packages.gentoo.org/packages/app-shells/zsh) | The Z Shell, first released in 1990. | 

## POSIX

- POSIX doesn't require echo to be provided as a shell builtin, only as a utility. [The utility is not required to have any options](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/echo.html#tag_20_37).

- POSIX doesn't require printf to be provided as a shell builtin, only as a utility. The conversion specifiers defined by POSIX for the printf utility are described in [the "File Format Notation" section](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap05.html#tag_05): `a`, `A`, `c`, `d`, `e`, `E`, `f`, `F`, `g`, `G`, `i`, `o`, `s`, `u`, `x`, `X`, `%`. Additionally, the `b` specifier is [also defined](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/printf.html#tag_20_96_13). However, "[\[t\]he a, A, e, E, f, F, g, and G conversion specifiers need not be supported](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/printf.html#tag_20_96_13)".

### Non-POSIX

The following are non-[POSIX](https://wiki.gentoo.org/wiki/POSIX), as per [Volume 3 of POSIX.1-2024](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/toc.html), "Shells and Utilities". Note that something being specified by POSIX does *not* mean that all shells have necessarily implemented it (although they might plan to).

- `[[ ... ]]` for tests. [`test` / `\[ ... \]`](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/test.html#tag_20_121) should be used instead.

- `==` in test expressions<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. Instead, `=` should be used to compare strings, `-eq` to compare numbers.

- The `function` keyword for defining functions.

- Arrays (e.g. `$VAR[1]`) and associative arrays (e.g. `$VAR['key']`).

- Various forms of parameter expansion syntax:
  - `${<var>:<offset>}` and `${<var>:<offset>:<length>}`, for subscripting.
  - `${<var>/<pattern>/<string>}`, `${<var>//<pattern>/<string>}`, `${<var>/#<pattern>/<string>}`, and `${<var>/%<pattern>/<string>}`, for pattern substitution.
  - `${<var>^<pattern>}`, `${<var>^^<pattern>}`, `${<var>,<pattern>}`, and `${<var>,,<pattern>}`, for case modification.

- Process substitution, e.g. diff \<$(command one) \<$(command two).

- Options to read other than [`-r` and `-d`](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/read.html#tag_20_100).

- select, for creating a menu of options selectable by number.

- rehash to refresh the hash containing the locations of utilities. hash -r should be used instead.

- Options to the type utility. Note also that POSIX doesn't require shells to provide type as a builtin. If type -P functionality is required, use command -v.

- local, for creating a variable scoped to a function and its children. However, it *is* supported by Dash.

- disown, for specifying that a job should either be removed from the jobs table, or not get sent a SIGHUP if the shell receives a SIGHUP. This builtin is implemented (with varying semantics) by Bash, ATT Ksh, and Zsh, but not by Dash, OpenBSD Ksh, or BusyBox sh.

- declare and typeset, for setting and getting shell variables and their attributes. Refer to the "[declare and typeset](https://wiki.gentoo.org/wiki/Shell/Scripting#declare_and_typeset)" section for shell differences involving these builtins.

### Introduced in POSIX-1.2024

- [Dollar-single-quotes](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/V3_chap02.html#tag_19_02_04), the `$'...'` construct (e.g. `$'\n'`).

- [the `;&` construct in `case`](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/V3_chap02.html#tag_19_09_04_05), to allow fallthrough to subsequent cases.

- The `pipefail` shell option.

- The `-print0` option for [find(1p)](https://man.archlinux.org/man/find.1p.en)

- The `-d` option for the read builtin.

- [make(1p) changes](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/make.html#tag_20_76_25) include, but are not limited to:
  - new operators for macro assignment: `::=`, `?=`, `+=`, `!=`, `:::=`;
  - the `-j <maxjobs>` option and the `.NOTPARALLEL` and `.WAIT` special targets;
  - slashes and hyphens in target names.

For a more comprehensive (but non-official) list, refer to [this document](https://gist.github.com/Earnestly/29deee4f18346da6630ed1df760f1590).

### POSIX conformance modes in shells

#### Bash

POSIX behavior in Bash can be requested via either the `--posix` option (when starting the shell) or set -o posix (from within a running shell). Additionally, invoking Bash as sh will enable POSIX mode once startup files are read.

When in POSIX mode, the `POSIXLY_CORRECT` variable is set.

Details about how Bash behaves in POSIX mode can be found in [this document](https://tiswww.case.edu/php/chet/bash/POSIX).

#### ATT Ksh

POSIX behavior can be requested via set -o posix. Refer to the [relevant section of the man page](https://manpages.debian.org/unstable/ksh93u+m/ksh93.1.en.html#posix) for details about behavioral changes in POSIX mode.

#### OpenBSD Ksh

The man page for [app-shells/loksh](https://packages.gentoo.org/packages/app-shells/loksh), the Linux port of OpenBSD's Ksh, states:

The shell is intended to be POSIX compliant; however, in some cases, POSIX behaviour is contrary either to the original Korn shell behaviour or to user convenience.


POSIX behavior can be requested via set -o posix or by setting the `POSIXLY_CORRECT` variable in the environment from which ksh is started.

Refer to the [relevant section of the man page](https://man.openbsd.org/ksh.1#POSIX_mode) for details about behavioral changes in POSIX mode.

#### Zsh

- To enable POSIX-style word splitting, set the `SH_WORD_SPLIT` option.

## Shell comparisons

### Behavior of echo

As noted above, echo is not required to be a shell builtin, and the echo utility is not required to support any options. Additionally, however, the behavior of echo varies (as at 2025-03-06):

| Version | Behavior of `echo '\n'` | Behavior of echo "\n" | Behavior of echo $'\n' | 
|---|---|---|---|
| Bash builtin echo | $ echo '\n' \n $ | $ echo "\n" \n $ | $ echo $'\n' $ | 
| ATT Ksh builtin echo | $ echo '\n' \n $ | $ echo "\n" \n $ | $ echo $'\n' $ | 
| OpenBSD Ksh builtin echo | $ echo '\n' $ | $ echo "\n" $ | $ echo $'\n' $ $ | 
| BusyBox sh builtin echo | $ echo '\n' \n $ | echo "\n" \n $ | echo $'\n' $ | 
| Zsh builtin echo | $ echo '\n' $ | $ echo "\n" $ | $ echo $'\n' $ | 
| GNU utility echo | $ /usr/bin/echo '\n' \n $ | $ /usr/bin/echo "\n" \n $ | $ /usr/bin/echo $'\n' $ | 
| OpenBSD utility echo | $ /bin/echo '\n' \n $ | $ /bin/echo "\n" \n $ | $ /bin/echo $'\n' $\n $ | 

Note that, as shells are not required by POSIX to provide an echo builtin, Dash uses the available echo utility (i.e. by default on Gentoo, the GNU echo utility).

All of Bash, OpenBSD Ksh and Zsh provide the `-e`, `-E` and `-n` options to their echo builtin (although OpenBSD Ksh only treats `-e` and `-E` as options in non-POSIX mode). BusyBox sh only provides the `-e` and `-n` options. However, the escape sequences affected by the `-e` and `-E` options differ between shells:

| Sequence | Description | Bash | OpenBSD Ksh | BusyBox sh | Zsh | 
|---|---|---|---|---|---|
| `\a` | Alert (bell) |  |  |  |  | 
| `\b` | Backspace |  |  |  |  | 
| `\c` | Suppress further output |  |  |  |  | 
| `\e` | Escape character |  |  |  |  | 
| `\E` | Escape character |  |  |  |  | 
| `\f` | Form feed |  |  |  |  | 
| `\n` | New line |  |  |  |  | 
| `\r` | Carriage return |  |  |  |  | 
| `\t` | Horizontal tab |  |  |  |  | 
| `\v` | Vertical tab |  |  |  |  | 
| `\\` | Backslash |  |  |  |  | 
| `\0`*nnn* | The eight-bit character whose value is the octal value *nnn* (zero to three octal digits) |  |  |  |  | 
| `\x`*HH* | The eight-bit character whose value is the hexadecimal value *HH* (one or two hex digits) |  |  |  |  | 
| `\u`*HHHH* | The Unicode character whose value is the hexadecimal value *HHHH* (one to four hex digits) |  |  |  |  | 
| `\u`*HHHHHHHH* | The Unicode character whose value is the hexadecimal value *HHHHHHHH* (one to eight hex digits) |  |  |  |  | 

### Behavior of arrays

- Dash doesn't support arrays, as arrays are not specified by POSIX.
- BusyBox sh doesn't appear to support arrays (as at 2025-03-06).

#### Declaration syntax

| Shell | Syntax | 
|---|---|
| Bash | x=(1 2 3), declare -a x | 
| ATT Ksh | x=(1 2 3) | 
| OpenBSD Ksh | set -A x 1 2 3 | 
| Zsh | x=(1 2 3), set -A x 1 2 3 | 

#### Subscripting syntax

By default, and after having defined an array `a` with the elements `1`, `2`, and `3`:

| Syntax | Bash | ATT Ksh | OpenBSD Ksh | Zsh | 
|---|---|---|---|---|
| `$a` | `1` | `1` | `1` | `1 2 3` | 
| `$a[1]` | `1[1]` | `1[1]` | `1[1]` | `1` | 
| `${a[0]}` | `1` | `1` | `1` | Empty string | 
| `${a[1]}` | `2` | `2` | `2` | `1` | 
| `${a[-1]}` | `3` | `3` | Error | `3` | 
| `${a[1,-1]}` | `3` | `3` | Error | `1 2 3` | 
| `${a[1..-1]}` | Error | `2 3` | Error | Error | 

Zsh can be configured to use 0-based array indexing via the `KSH_ZERO_SUBSCRIPT` and `KSH_ARRAYS` options.

Additionally, when Zsh's `KSH_ARRAYS` option is set, braces are required when subscripting.

### Behavior of pipelines

In general, each command in a pipeline is run in a subshell. However, there are differences in how shells handle a pipeline's final command: Bash, Dash, OpenBSD Ksh, and BusyBox sh run the final command in a subshell, but ATT Ksh and Zsh run the final command in the current shell. Bash's behavior can be changed to run the final command in the current shell by disabling job control (set +m) and then setting the `lastpipe` option (shopt -s lastpipe).

### Expansion precedence

In ATT Ksh, OpenBSD Ksh, and Zsh, parameter expansion is done before brace expansion; in Bash, the reverse is true.

### declare and typeset

In Bash and Zsh, declare is a synonym for typeset. declare is not available in ATT Ksh or OpenBSD Ksh, only typeset. Neither declare nor typeset are available in BusyBox sh or Dash.

#### ATT Ksh

- Bash supports the local builtin, ATT Ksh does not.
- ATT Ksh allows subscript ranges (e.g. `${a[1..-1]}`), Bash does not.

#### OpenBSD Ksh

- Bash supports the local builtin, OpenBSD Ksh does not.
- As of 2024-09-23, OpenBSD ksh [no longer accepts NUL bytes in scripts](http://www.undeadly.org/cgi?action=article;sid=20240924105732).

- Bash's builtin to set options is shopt; Zsh's analogous builtin is setopt. The list of available Zsh options is not a strict superset of Bash options; for example, `cdspell` is a Bash option but not a Zsh option.

- Zsh's alias builtin has options unavailable in Bash's alias builtin. In particular, Bash's alias only has the `-p` option, equivalent to providing no option at all, which prints defined aliases on standard output.

- The `**` syntax for 'recursive globbing' is available in Zsh by default, but is not enabled by default in Bash; it can be enabled via the `globstar` option.

- In Bash, by default, if the `*` glob has no matches, no error will be produced (option `nofailglob`). However, in Zsh, by default, an error will be produced (option `NOMATCH`).

- The available options for the read builtin differ between Bash and Zsh.

Bash:

read \[ -ers \] \[ -a aname \] \[ -d delim \] \[ -i text \] \[ -n nchars \] \[ -N nchars \] \[-p prompt \] \[ -t timeout \] \[ -u fd \] \[ name ... \]

Zsh:

read \[ -rszpqAclneE \] \[ -t \[ num \] \] \[ -k \[ num \] \] \[ -d delim \] \[ -u n \] \[ \[name\]\[?prompt\] \] \[ name ...  \]

- Bash supports format specifications for the printf builtin which aren't supported by Zsh: `%Q` and `%(<datefmt>)T`.

#### Arrays

- Bash uses 0-based indexing, Zsh uses 1-based indexing unless the Zsh `KSH_ZERO_SUBSCRIPT` option or the `KSH_ARRAYS` option is set.
- Bash requires curly braces around subscripted array references (e.g. `${a[1]}`), Zsh does not, unless the Zsh `KSH_ARRAYS` option is set.
- Zsh allows specifying subscript ranges (e.g. `${a[1,-2]}`), Bash does not.
