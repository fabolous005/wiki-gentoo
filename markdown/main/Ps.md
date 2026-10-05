<!-- source: https://wiki.gentoo.org/wiki/Ps | group: Gentoo Wiki (Main) | wiki-title: Ps -->
---
title: ps
url: https://wiki.gentoo.org/wiki/Ps
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-10"
fingerprint: e6d2cd178c83f790
license: CC BY-SA 4.0
---

# ps

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**ps** (short for **p**rocess **s**tatus) is a tool for reporting on a system's active processes. It has a long history on Unix-like operating systems such as BSD and Linux. Consequently, it accepts a very wide variety of input flags in one of three forms: Unix-style options preceded by a single dash, BSD-style options which do not have a dash, and GNU long options which are preceded by two dashes.

## Installation

### USE flags


| [+kill](https://packages.gentoo.org/useflags/+kill) | Build the kill program | 
| [+ncurses](https://packages.gentoo.org/useflags/+ncurses) | Build programs that use ncurses: top, slabtop, watch | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Use sys-auth/elogind for session tracking. | 
| [modern-top](https://packages.gentoo.org/useflags/modern-top) | Enables new startup defaults of top. Keeps old defaults if disabled | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [skill](https://packages.gentoo.org/useflags/skill) | Build the skill and snice programs | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unicode](https://packages.gentoo.org/useflags/unicode) | Add support for Unicode | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

[sys-process/procps](https://packages.gentoo.org/packages/sys-process/procps) is part of the [@system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), so it should be installed by default.

In case it is ever needed, reinstall [sys-process/procps](https://packages.gentoo.org/packages/sys-process/procps):

`root #``emerge --ask --oneshot sys-process/procps`
## Configuration

### Environment variables

- `$PS_FORMAT` — override the default output format.

### Files

- /proc — the virtual file system ps reads to obtain the information required for its reports.

## Usage

### Invocation

To see options available to the ps command:

`user $``ps --help all````
Usage:
 ps [options]
Basic options:
 -A, -e               all processes
 -a                   all with tty, except session leaders
  a                   all with tty, including other users
 -d                   all except session leaders
 -N, --deselect       negate selection
  r                   only running processes
  T                   all processes on this terminal
  x                   processes without controlling ttys
Selection by list:
 -C <command>         command name
 -G, --Group <GID>    real group id or name
 -g, --group <group>  session or effective group name
 -p, p, --pid <PID>   process id
        --ppid <PID>  parent process id
 -q, q, --quick-pid <PID>
                      process id (quick mode)
 -s, --sid <session>  session id
 -t, t, --tty <tty>   terminal
 -u, U, --user <UID>  effective user id or name
 -U, --User <UID>     real user id or name
  The selection options take as their argument either:
    a comma-separated list e.g. '-u root,nobody' or
    a blank-separated list e.g. '-p 123 4567'
Output formats:
 -F                   extra full
 -f                   full-format, including command lines
  f, --forest         ascii art process tree
 -H                   show process hierarchy
 -j                   jobs format
  j                   BSD job control format
 -l                   long format
  l                   BSD long format
 -M, Z                add security data (for SELinux)
 -O <format>          preloaded with default columns
  O <format>          as -O, with BSD personality
 -o, o, --format <format>
                      user-defined format
  s                   signal format
  u                   user-oriented format
  v                   virtual memory format
  X                   register format
 -y                   do not show flags, show rss vs. addr (used with -l)
     --context        display security context (for SELinux)
     --headers        repeat header lines, one per page
     --no-headers     do not print header at all
     --cols, --columns, --width <num>
                      set screen width
     --rows, --lines <num>
                      set screen height
Show threads:
  H                   as if they were processes
 -L                   possibly with LWP and NLWP columns
 -m, m                after processes
 -T                   possibly with SPID column
Miscellaneous options:
 -c                   show scheduling class with -l option
  c                   show true command name
  e                   show the environment after command
  k,    --sort        specify sort order as: [+|-]key[,[+|-]key[,...]]
  L                   show format specifiers
  n                   display numeric uid and wchan
  S,    --cumulative  include some dead child process data
 -y                   do not show flags, show rss (only with -l)
 -V, V, --version     display version information and exit
 -w, w                unlimited output width
        --help <simple|list|output|threads|misc|all>
                      display help and exit
For more details see ps(1).
```
### Show all running processes

ps may be used to show all running processes by using the `a`, `u`, and `x`, options (see invocation section on what these options do):

`user $``ps aux`
## Tips

### Find a specific process

To find details of a specific process by name, the ps command output may be piped to [grep](https://wiki.gentoo.org/wiki/Grep):

`user $``ps aux | grep <process name>`
## See also

- [htop](https://wiki.gentoo.org/wiki/Htop) — a cross-platform interactive process viewer. It is a text-mode application (for console or X terminals) and requires ncurses.
