<!-- source: https://wiki.gentoo.org/wiki/MAKEOPTS | group: Gentoo Wiki (Main) | wiki-title: MAKEOPTS -->
---
title: MAKEOPTS
url: https://wiki.gentoo.org/wiki/MAKEOPTS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-01"
fingerprint: "3a097614b62bbdec"
license: CC BY-SA 4.0
---

# MAKEOPTS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

`MAKEOPTS` is a variable that defines and limits how many parallel make jobs can be launched from Portage. It can be set in the [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf#MAKEOPTS) configuration file.

## MAKEOPTS on a system-wide basis

The parallel jobs entry ensures that, when make is invoked, it knows how many parallel sessions it is allowed to trigger (when parallel sessions are possible of course). This is completely within the scope of that make command and has no influence on parallel installations (which is triggered through emerge with --jobs=X option). The recommended value is the number of logical processors in the CPU.

On a system with one Intel Core i7 CPU the following command shows the numbering of the available logical CPUs.

`root #``lscpu`
Architecture:        x86\_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              8
On-line CPU(s) list: 0-8
Thread(s) per core:  2
Core(s) per socket:  4
Socket(s):           1

The make program creates parallel tasks based on the system's current load average and the --load-average limit. Since the load average is a damped value, the load-average jobs limitation can be sensibly set slightly above the number of available CPUs. For a machine with 4 physical cores with 2 threads per core, or 8 logical cores, `MAKEOPTS` *could* be set to:

**`/etc/portage/make.conf`**

```
MAKEOPTS="--jobs 8 --load-average 9"
```
Also, since Portage 3.0.31<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> and 3.0.53<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, --jobs and --load-average respectively default to the number of threads returned by nproc if `MAKEOPTS` is unset.

Jobs should also be limited by RAM. Recent gcc versions can take 1.5 GB to 2 GB of RAM per job for large C++ codebases. For systems with 8 logical CPUs and only 4 GB RAM, `MAKEOPTS` should be lowered to `-j2` so that the system can run the basics and also compile without hitting [swap](https://wiki.gentoo.org/wiki/Swap) very often slowing things down. To measure the time needed to compile the [app-misc/mc](https://packages.gentoo.org/packages/app-misc/mc) package with 4 jobs:

`root #``time MAKEOPTS="-j4" emerge app-misc/mc`
## MAKEOPTS on a per-package basis

Packages can be emerged with different `MAKEOPTS` values on a per-package basis. See [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env) for details.

## See also

- [EMERGE\_DEFAULT\_OPTS](https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS) — a variable for [Portage](https://wiki.gentoo.org/wiki/Portage) that defines entries to be appended to the emerge command line.
- [Knowledge Base:Emerge out of memory](https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_out_of_memory)
- [Portage niceness](https://wiki.gentoo.org/wiki/Portage_niceness) — describes some configuration options available for system administrators to help manage [Portage](https://wiki.gentoo.org/wiki/Portage)'s resource usage.
- [steve](https://wiki.gentoo.org/wiki/Steve) — a jobserver implementation for Gentoo
