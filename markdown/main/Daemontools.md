<!-- source: https://wiki.gentoo.org/wiki/Daemontools | group: Gentoo Wiki (Main) | wiki-title: Daemontools -->
---
title: daemontools
url: https://wiki.gentoo.org/wiki/Daemontools
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-29"
fingerprint: "1b1a5ddb28950992"
license: CC BY-SA 4.0
---

# daemontools

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Daniel J. Bernstein's daemontools package, described by him as "*a collection of tools for managing UNIX services*", is the pioneer of what some people call today *process supervision suites*, i.e. packages that provide tools for performing process supervision[\[1\]](https://wiki.gentoo.org#cite_note-1)[\[2\]](https://wiki.gentoo.org#cite_note-2)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. There are no further releases of daemontools after 0.76 (released in 2001), but other software packages have been inspired by its design principles, notably [runit](https://wiki.gentoo.org/wiki/Runit), [s6](https://wiki.gentoo.org/wiki/S6), [perp](http://b0llix.net/perp/site.cgi?page=about), [nosh](https://jdebp.uk/Softwares/nosh/), and an enhanced succesor, [daemontools-encore](https://wiki.gentoo.org/wiki/Daemontools-encore) <sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.

## Installation

### USE flags


### Emerge

`root #``emerge --ask sys-process/daemontools`
## Configuration

### Files

- /service - Location of the scan directory when using [OpenRC](https://wiki.gentoo.org/wiki/OpenRC), svscanboot, or svscan-add-to-inittab from [sys-process/supervise-scripts](https://packages.gentoo.org/packages/sys-process/supervise-scripts).

### Service

#### OpenRC

See [here](https://wiki.gentoo.org/wiki/Daemontools-encore#openrclaunch) for details.

## Usage

See [daemontools-encore](https://wiki.gentoo.org/wiki/Daemontools-encore#Usage).

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sys-process/daemontools`
The same extra steps after [removing daemontools-encore](https://wiki.gentoo.org/wiki/Daemontools-encore#Removal) apply here.

## See also

- [Runit](https://wiki.gentoo.org/wiki/Runit) — lightweight process supervision suite, originally inspired by [daemontools] that offers fast and reliable service management.
- [S6](https://wiki.gentoo.org/wiki/S6) — a package that provides a [daemontools-inspired] process supervision suite, a notification framework, a UNIX domain super-server, and tools for file descriptor holding and suidless privilege gain.
- [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) — a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system
- [Systemd](https://wiki.gentoo.org/wiki/Systemd) — a modern SysV-style init and [rc](https://wiki.gentoo.org/wiki/Rc) replacement for Linux systems.

## External resources

- [https://cr.yp.to/daemontools/install.html](https://cr.yp.to/daemontools/install.html) - A very short guide on how to install daemontools.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) D. J. Bernstein, [daemontools FAQ](https://cr.yp.to/daemontools/faq/create.html#why), which includes one about the benefits of process supervision. Retrieved on April 23rd, 2017.
2. [↑](https://wiki.gentoo.org#cite_ref-2) Gerrit Pape, [runit benefits](http://smarden.org/runit/benefits.html), which includes a short description of process supervision in general. Retrieved on April 23rd, 2017.
3. [↑](https://wiki.gentoo.org#cite_ref-3) Laurent Bercot, [s6 overview](https://www.skarnet.org/software/s6/overview.html), which contains an introduction to process supervision. Retrieved on April 23rd, 2017.
4. [↑](https://wiki.gentoo.org#cite_ref-4) Jonathan de Boyne Pollard, [The daemontools family](https://jdebp.uk/FGA/daemontools-family.html). Retrieved on May 16th, 2017.
