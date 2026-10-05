<!-- source: https://wiki.gentoo.org/wiki/Guildmaster | group: Gentoo Wiki (Main) | wiki-title: Guildmaster -->
---
title: guildmaster
url: https://wiki.gentoo.org/wiki/Guildmaster
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-19"
fingerprint: "7355f457a9b5a9ad"
license: CC BY-SA 4.0
---

# guildmaster

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**guildmaster** is a simple jobserver implementation.

Its implementation is much simpler than [Steve](https://wiki.gentoo.org/wiki/Steve)'s and hence has easier to understand behavior without heuristics, at the cost of fewer features.

[GNU make](https://wiki.gentoo.org/wiki/Make) and other supporting clients support requesting tokens from a [jobserver](https://www.gnu.org/software/make/manual/html_node/POSIX-Jobserver.html) for coordination of parallelism across different make (and friends) instances.

This allows capping the total number of parallel jobs across different emerge jobs or calls.

## Installation

`root #``emerge --ask dev-build/guildmaster`
guildmaster should then be enabled via the init system, i.e.

`root #``systemctl enable --now guildmaster`
or

`root #``rc-update add guildmaster default`
guildmaster itself has a README in /usr/share/doc/guildmaster\*.

## Configuration

Clients are configured via `MAKEFLAGS="--jobserver-auth=fifo:/dev/guild"`.

Note that while the the *--jobserver-auth* flag is GNU Make-specific, non-GNU Make clients usually only check `MAKEFLAGS` and not `GNUMAKEFLAGS`.

### guildmaster daemon

guildmaster itself can be configured via the systemd unit (systemctl edit guildmaster) or OpenRC init script (/etc/conf.d/guildmaster).

It will hand out a maximum of `$(nproc)` tokens.

## Usage

### Packages

For the jobserver to be used by packages, the package manager must be told how to communicate this to build systems: Example /etc/portage/make.conf snippet:

**`/etc/portage/make.conf`**

```
# Replace 32 by the value of $(nproc)
MAKEOPTS="-j32 -l32"
NINJAOPTS="-l32"
MAKEFLAGS="-l32 --jobserver-auth=fifo:/dev/guild"
```
It is important that *-jN* is **not** passed to make or other clients, as they interpret this as a directive to not use the jobserver. Portage will defer to `MAKEFLAGS` if both `MAKEOPTS` and `MAKEFLAGS` are set.

On the other hand, `MAKEOPTS` should still be set because some packages using less popular build systems (not involving make or ninja will extract *-jN* from it to use an appropriate level of parallelism.

### Portage

Portage itself will soon gain support for claiming a jobserver token per job in emerge --jobs. This is important because of the 'implicit slot' problem. See [bug #966879](https://bugs.gentoo.org/show_bug.cgi?id=966879).

It will be controlled by `FEATURES="jobserver-token"` and can be tested using this [Portage patch](https://github.com/gentoo/portage/pull/1528).



## See also

- [Steve](https://wiki.gentoo.org/wiki/Steve) — a jobserver implementation for Gentoo
- [Make](https://wiki.gentoo.org/wiki/Make) — a [tool to *build*](https://wiki.gentoo.org/wiki/Build_automation#Available_software) software from source code (which usually includes compiling)
