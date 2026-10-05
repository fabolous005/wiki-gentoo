<!-- source: https://wiki.gentoo.org/wiki/DTrace | group: Gentoo Wiki (Main) | wiki-title: DTrace -->
---
title: DTrace
url: https://wiki.gentoo.org/wiki/DTrace
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-04"
fingerprint: a6b5bb5e20efbbf0
license: CC BY-SA 4.0
---

# DTrace

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**DTrace** is a dynamic tracing tool for analysing or debugging the whole system. It can analyse and inspect both the kernel (inc. modules) and userland applications. It might be used for both analysing performance issues, but also analysing unexpected or wrong behavior from either userland or the kernel.

## Installation

### Kernel configuration

This modern DTrace implementation is based on *eBPF* and requires a suite of kernel configuration options which the ebuild checks for. In particular:

**Debug Options**

```
General Setup ->
 Compile-time checks and compiler options ->
   <*> Configure standard kernel features (expert users) ->
     <*> Load all symbols for debugging/ksymoops
     <*> Include all symbols in kallsyms 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_KALLSYMS_ALL</code> to find this item.
File systems ->
   <*> FUSE (Filesystem in Userspace) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FUSE_FS</code> to find this item.
   <*> Character device in Userspace support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CUSE</code> to find this item.
Kernel Hacking  ->
   <*> Tracers -> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FTRACE</code> to find this item.
    <*> Trace syscalls [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FTRACE_SYSCALLS</code> to find this item.
    <*> Enable uprobes-based dynamic events [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_UPROBE_EVENTS</code> to find this item.
    <*> enable/disable function tracing dynamically [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DYNAMIC_FTRACE</code> to find this item.
    <*> Kernel Function Tracer [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FUNCTION_TRACER</code> to find this item.
   <*> Generate BTF typeinfo [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DEBUG_INFO_BTF</code> to find this item.
#### dist-kernel

When using a [Distribution Kernel](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel), enabling [debug](https://packages.gentoo.org/useflags/debug) [for the relevant \*-kernel package covers some of these.](https://wiki.gentoo.org/wiki/USE_flag)

If using gentoo-kernel\[hardened\], the following workaround config.d snippet can be used:

**`/etc/kernel/config.d/50hardened.config`**


A general snippet for DTrace is below:

**`/etc/kernel/config.d/50dtrace.config`**

### Userland

Modern versions of DTrace are Extended Berkeley Packet Filter ([eBPF](https://en.wikipedia.org/wiki/eBPF))-based, so there's no need for a separate kernel module, or any patches.

Simply install the userland tools:

`root #``emerge --ask dev-debug/dtrace`
#### systemd

`root #``systemctl enable --now dtprobed`
#### OpenRC

`root #``rc-update add dtprobed default && rc-service dtprobed start`
## Getting started

The DTrace framework is centred around two concepts:

- *D scripts* (not to be confused with D programming language, also called [Dlang](https://wiki.gentoo.org/wiki/Dlang)): these can be one-liners or whole scripts with a traditional shebang for convenience;
- *probes* which attach to some system component of interest, like a part of a kernel module, or userland application

Other terminology:

- *providers* publishes available *probes*
- *modules* provide an implementation of a *probe*

For example, the *syscall* provider, via the *vmlinux* module, has various probes for each syscall at *entry* and *return*.

Following the [official D script tutorial](https://docs.oracle.com/en/operating-systems/oracle-linux/dtrace-tutorial/) is strongly recommended.

### Syntax

D Script's syntax is roughly [Awk](https://wiki.gentoo.org/wiki/Awk)-like. Below is a basic skeleton:


```
#!/usr/bin/env dtrace -s
PROBE
/optional predicate condition/
{
    /* Action */
}
```
`BEGIN` and `END` blocks are special, like constructors/destructors:


```
#!/usr/bin/env dtrace -s
/* Sample script which starts up, prints a message, then exits and shows
   another message before terminating. */
BEGIN
{
    i = 0;
    printf("Starting up with i=%d\n", i);
    exit(0);
}
END
{
    i++;
    printf("Shutting down with i=%d\n", i);
}
```
A fully qualified probe name takes the form `provider:module:function:name`. Wildcards can be used to match multiple probes. To leave a field blank, use a colon-separator, e.g. `provider::function:name`. In general, DTrace tries to solve ambiguous names rather than bailing out.

For more information, see the [D Program Syntax Reference](https://docs.oracle.com/en/operating-systems/oracle-linux/dtrace-v2-guide/d_program_syntax_reference.html#reference_h2t_nbw_kwb).

## Usage

DTrace shines in investigating emerging incidents where one-liners or small scripts can quickly be written to both gather information but also to prove or disprove a theory about root causes. As a result, the most important thing for using DTrace is being familiar with its primitives, rather than having a large corpus of pre-existing scripts ready to use.

### Investigating locks

Suppose a system has a mysterious lock file being rapidly created and then deleted.

Artificially create such a scenario with a loop:

`root #``while true ; do flock -s $(mktemp) sleep 1 ; done`
This can be easily investigated:


```
#!/usr/bin/env dtrace -s
#pragma D option quiet
syscall::openat:entry
{
  /* Record the pathname arg to openat(2). */
  self->path = args[1];
}
syscall::openat:return
{
  /* Store the fd that openat(2) returns, corresponding to the pathname from earlier. */
  map[args[0]] = self->path;
}
syscall::flock:entry
{
  /* Log the fd we're locking. */
  trace(stringof(args[0]));
  /* Use the information we recorded earlier to show what PID is locking what file. */
  printf("Lock was taken by pid=%d for file=%s\n", pid, stringof(map[args[0]]));
}
```
### Probing a probe

When writing a script, one might need to know the argument types (prototype) of a probe. For example, to investigate the parameters for *syscall::vmlinux::openat* and *syscall::vmlinux::openat2*:

`root #``dtrace -l -n openat*:entry -v````
   ID   PROVIDER            MODULE                          FUNCTION NAME
124115    syscall           vmlinux                            openat entry
[...]
        Argument Types
                args[0]: int
                args[1]: char *
                args[2]: int
                args[3]: umode_t
124113    syscall           vmlinux                           openat2 entry
[...]
        Argument Types
                args[0]: int
                args[1]: char *
                args[2]: struct open_how *
                args[3]: size_t
```
## Troubleshooting

### Basic sanity checks

First, have DTrace enable a trivial probe and return once it is hit:

`root #``dtrace -n 'BEGIN { exit(0); }'`
dtrace: description 'BEGIN ' matched 1 probe
CPU     ID                    FUNCTION:NAME
 28      1                           :BEGIN

Check whether the syscall probe has very basic functionality:

`root #``dtrace -n 'syscall:::entry { exit(0); }'`
dtrace: description 'syscall:::entry ' matched 344 probes
CPU     ID                    FUNCTION:NAME
  3 124015                        bpf:entry

If either of these commands hang or don't produce a table as above, DTrace isn't working correctly. It may be caused by exotic optimization flags.

All the available probes can be listed with dtrace -l:

`root #``dtrace -l````
   ID   PROVIDER            MODULE                          FUNCTION NAME
    1     dtrace                                                     BEGIN
    2     dtrace                                                     END
    3     dtrace                                                     ERROR
    4        cpc                                                     perf_count_hw_cpu_cycles-all-1000000000
    5        cpc                                                     cycles-all-1000000000
    6        cpc                                                     cpu_cycles-all-1000000000
    7        cpc                                                     perf_count_hw_instructions-all-1000000000
    8        cpc                                                     instructions-all-1000000000
[...]
```
As of August 2024, on an **\~amd64** system with linux-6.6, around 125000 probes are registered. If the number is substantially **lower** than that, it's possible some required kernel config options are not enabled.

### Faulting on a bad address

Using a valid argument in an entry probe which the process hasn't yet accessed [might cause a fault](https://docs.oracle.com/en/operating-systems/solaris/oracle-solaris/11.4/dtrace-guide/avoiding-errors.html). To workaround this, store the argument in `this->foo` with *copyinstr*.

### Online examples not working

Some examples online may not work for a few reasons:

1. Relying on unstable kernel ABI so 'function' names or types may have changed
2. The example relies on assumptions about libc and how it wraps syscalls
3. Overly specific names

Below are a few examples of both a problem and analysis to fix it. The same methods can be applied to other bad examples on the internet.

#### execveat\_common

For example, consider this script for tracing *execve*:

`root #``dtrace -n 'proc::do_execveat_common:exec { trace(stringof(args[0])); }'````
dtrace: invalid probe specifier proc::do_execveat_common:exec { trace(stringof(args[0])); }: probe description proc::do_execveat_common:exec does not match any probes
```
The specification *proc::do\_execveat\_common:exec* is prone to internal changes. BPF-based DTrace provides a tracepoint for *exec* elsewhere.

To fix it, use the more generic proc tracepoint:

`root #``dtrace -n 'proc:::exec { trace(stringof(args[0])); }'`
dtrace: description 'proc:::exec ' matched 1 probe
CPU     ID                    FUNCTION:NAME
  2 123651                            :exec   /usr/sbin/utempter
  3 123651                            :exec   /usr/sbin/utempter
 10 123651                            :exec   /bin/bash
  1 123651                            :exec   /usr/bin/dircolors

#### open

For example, consider this script for tracing *open*:

`root #``dtrace -n 'syscall::open:entry { printf("%-16s %-16s\n",execname,copyinstr(arg0)); }'`
dtrace: description 'syscall::open:entry ' matched 1 probe
\[Likely to have no further output\]

Most functions used to open files don't actually use the *open* syscall in glibc, but one of the other wrappers instead. Note that arg0 changes to arg1 here too as we see *openat* is being used which has different parameters:

`root #``dtrace -n 'syscall::open*:entry { printf("%-16s %-16s\n",execname,copyinstr(arg1)); }'`
dtrace: description 'syscall::open\*:entry ' matched 5 probes
CPU     ID                    FUNCTION:NAME
  5 128754                     openat:entry cc1plus          /usr/lib/gcc/x86\_64-pc-linux-gnu/15/include/g++-v15/bits/stl\_algobase.h
  2 128754                     openat:entry cc1plus          /usr/lib/gcc/x86\_64-pc-linux-gnu/15/include/g++-v15/bits/stl\_heap.h
  2 128754                     openat:entry cc1plus          /usr/lib/gcc/x86\_64-pc-linux-gnu/15/include/g++-v15/bits/uniform\_int\_dist.h
  2 128754                     openat:entry cc1plus          /usr/lib/gcc/x86\_64-pc-linux-gnu/15/include/g++-v15/bits/stl\_construct.h
  2 128754                     openat:entry cc1plus          /usr/lib/gcc/x86\_64-pc-linux-gnu/15/include/g++-v15/bits/stl\_tempbuf.h

## See also

- [SystemTap](https://wiki.gentoo.org/wiki/SystemTap) — powerful tool that provides an infrastructure to simplify the gathering of information about the running Linux kernel or userspace programs
