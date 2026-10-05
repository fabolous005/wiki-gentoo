<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_out_of_memory | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Emerge_out_of_memory -->
---
title: Knowledge Base:Emerge out of memory
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_out_of_memory
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-18"
fingerprint: "3abd74a0b423bdec"
license: CC BY-SA 4.0
---

# Knowledge Base:Emerge out of memory

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Symptom

- Building packages hits an "out of memory" error while using [emerge](https://wiki.gentoo.org/wiki/Emerge).
- The system becomes extremely slow because of [swap](https://wiki.gentoo.org/wiki/Swap) usage while using emerge.

## Solutions

### Decrease number of parallel compiler processes for some ebuilds

The main idea is to decrease number of parallel compiler processes for some ebuilds.

Check the current number of parallel jobs in [MAKEOPTS](https://wiki.gentoo.org/wiki/MAKEOPTS):

`user $````
portageq envvar MAKEOPTS
```
Also check [EMERGE\_DEFAULT\_OPTS](https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS):

`user $````
portageq envvar EMERGE_DEFAULT_OPTS
```
It is OK if `EMERGE_DEFAULT_OPTS` does not exist.

`EMERGE_DEFAULT_OPTS` cannot be set in /etc/portage/package.env. To override the defaults, specify new values on the command line instead.

Try to lower number of parallel jobs for some packages which usually requires more RAM to compile:

**`/etc/portage/env/j1.conf`**

```
MAKEOPTS="-j1"
```
**`/etc/portage/env/j2.conf`**

```
MAKEOPTS="-j2"
```
#### Special cases

##### Chromium

### Trade off for the GNU linker: use less memory and more IO

The linker GNU ld/bfd (***not*** the lld command) can use less memory at the expense of IO. The IO of the linker can be prioritized by configuring emerge to run with ionice -c3 by setting `PORTAGE_IONICE_COMMAND="ionice -c 3 -p \${PID}"` in [make.conf](https://wiki.gentoo.org/wiki/Make.conf). Swapping has always a high IO priority.

**`/etc/portage/env/j1.conf`**

```
MAKEOPTS="-j1"
LDFLAGS="${LDFLAGS} -Wl,--no-keep-memory"
```
### Very low memory (single-board computers etc.)

It is not advised to use parallel jobs in either `MAKEOPTS` nor `EMERGE_DEFAULT_OPTS` on systems that do not have much RAM (Raspberry Pi's with 512 MB of RAM, old desktop computers, etc.).

## See also

- [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env) — can contain files to be called during the installation of specific packages, or files used to set Portage's environment variables on a per-package basis.
- [EMERGE\_DEFAULT\_OPTS](https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS) — a variable for [Portage](https://wiki.gentoo.org/wiki/Portage) that defines entries to be appended to the emerge command line.
- [Knowledge Base:No space left on device while there is plenty of space available](https://wiki.gentoo.org/wiki/Knowledge_Base:No_space_left_on_device_while_there_is_plenty_of_space_available)
- [MAKEOPTS](https://wiki.gentoo.org/wiki/MAKEOPTS) — a variable that defines and limits how many parallel make jobs can be launched from Portage.
- [Portage niceness](https://wiki.gentoo.org/wiki/Portage_niceness) — describes some configuration options available for system administrators to help manage [Portage](https://wiki.gentoo.org/wiki/Portage)'s resource usage.
- [Portage TMPDIR on tmpfs](https://wiki.gentoo.org/wiki/Portage_TMPDIR_on_tmpfs) — It is unlikely that tmpfs will provide any performance gain for modern systems — another example: how to not use tmpfs for big packages (large packages list included)
- [steve](https://wiki.gentoo.org/wiki/Steve) — a jobserver implementation for Gentoo

## TODO

**Todo:**

- use `--load-average` for both MAKEOPTS and EMERGE\_DEFAULT\_OPTS?
