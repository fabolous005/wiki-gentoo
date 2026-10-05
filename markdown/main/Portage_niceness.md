<!-- source: https://wiki.gentoo.org/wiki/Portage/niceness | group: Gentoo Wiki (Main) | wiki-title: Portage/niceness -->
---
title: Portage/niceness
url: https://wiki.gentoo.org/wiki/Portage/niceness
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-30"
fingerprint: "2690d44684839dbc"
license: CC BY-SA 4.0
---

# Portage/niceness

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes some configuration options available for system administrators to help manage [Portage](https://wiki.gentoo.org/wiki/Portage)'s resource usage.

## Scheduling policy

Using Portage's scheduling policy, it is possible to define what scheduling policy the Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) will apply to [emerge](https://wiki.gentoo.org/wiki/Emerge) itself and all the build jobs. System administrators who wish to minimize Portage's impact on system responsiveness should set scheduling policy to `idle`. This will *significantly reduce* the disruption to the rest of the system by scheduling Portage processes as extremely low priority. The `idle` policy is order of magnitude lower than anything running with `PORTAGE_NICENESS` set to level of `19`.

### Configuration

To set the `idle` scheduling policy:

**`/etc/portage/make.conf`**

**Set`PORTAGE_SCHEDULING_POLICY` to idle**

```
# Extremely low priority
PORTAGE_SCHEDULING_POLICY="idle"
```
The supported options are:

- `other`
- `batch`
- `idle`
- `fifo`
- `round-robin`

See the [**sched(7)**](https://man7.org/linux/man-pages/man7/sched.7.html) man page for more information on scheduler options.

For more information about Portage's scheduling ability, search for `PORTAGE_SCHEDULING_POLICY` in man 5 make.conf.

## Portage "niceness"

### Priority and nice values

The priority value (PR) of a process ranges from 0 to 139, giving high to low priority respectively. Real time process occupy 0 to 99 and user processes range of 100 to 139.

User process priority is defined in terms of the nice level (NI) plus 20 (*NI + 20*). The nice level therefore ranges from -20 to 19, which corresponds to a user process priority of 0 to 39 and a PR value of 100 to 139. For example, giving a process a nice value of 0 translates into a PR of 120.

### Controlling priority

Linux has a few options to control system responsiveness by limiting a process' use of resources, including nice (which is POSIX, not Linux-exclusive), ionice, and chrt. The interaction between these is complicated and it's usually hard to reason about.

In short:

- nice controls priority with regard to the CPU scheduler
- ionice controls priority with regard to the disk I/O scheduler
- chrt is like an extended nice - it can change attributes of the process(es) which the CPU scheduler utilizes, rather than just the simplistic 'niceness' level, like priority/task *class*.

Resources online also cover the [distinction](https://serverfault.com/questions/161008/what-is-the-difference-between-renice-and-chrt-commands-in-linux) between nice and chrt.

In any case, all three are valuable tools in making Portage run smoothly in the background without interfering with general system usage from other processes. Anecdotally, chrt seems to make the most difference.

### Configuration

Enable `PORTAGE_SCHEDULING_POLICY`, `PORTAGE_NICENESS`, and `PORTAGE_IONICE_COMMAND` in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

```
# Extremely low priority (per above)
PORTAGE_SCHEDULING_POLICY="idle"
# Lowest priority
PORTAGE_NICENESS="19"
PORTAGE_IONICE_COMMAND="ionice -c 3 -p \${PID}"
```
## See also

- [EMERGE\_DEFAULT\_OPTS](https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS) — a variable for [Portage](https://wiki.gentoo.org/wiki/Portage) that defines entries to be appended to the emerge command line.
- [MAKEOPTS](https://wiki.gentoo.org/wiki/MAKEOPTS) — a variable that defines and limits how many parallel make jobs can be launched from Portage.
- [Knowledge Base:Emerge out of memory](https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_out_of_memory)
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.
- [steve](https://wiki.gentoo.org/wiki/Steve) — a jobserver implementation for Gentoo
