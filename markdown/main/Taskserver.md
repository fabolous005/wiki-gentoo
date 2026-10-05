<!-- source: https://wiki.gentoo.org/wiki/Taskserver | group: Gentoo Wiki (Main) | wiki-title: Taskserver -->
---
title: Taskserver
url: https://wiki.gentoo.org/wiki/Taskserver
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-08"
fingerprint: "8a148955a8663af2"
license: CC BY-SA 4.0
---

# Taskserver

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

[Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

TLDR:

**Do not use this article!**

**Resources**

**Taskserver** (**taskd**) is a back-end service for providing to-do list synchronization between [Taskwarrior](https://wiki.gentoo.org/wiki/Taskwarrior) and clients or remote devices. There are several possible configurations but some of the most common are:

- Running taskd on a server which synchronizes multiple user's tasks to a centralized location for ease of [backup](https://wiki.gentoo.org/wiki/Backup) purposes.
- Providing synchronization with mobile devices via applications such as [Foreground](https://github.com/bgregos/foreground) (Android) or [Taskwarrior Reminders](https://github.com/blampe/taskwarrior-reminders) (iOS), among others.
- As part of a hosted private cloud infrastructure for an individual or groups.

There have been occasional attempts at providing "Taskwarrior as a service" using a public facing taskd instance. At present this usage is discouraged as the tasd daemon is not yet seen as robust enough to handle the rigors imposed by commercial use by large numbers of simultaneous users. It is expected that taskd will eventually support such a setup.

## Installation

### USE flags

*app-misc/taskd*correct?

### Emerge

`root #``emerge --ask app-misc/taskd`
### Additional software

A large ecosystem of Taskwarrior and Taskserver related tools, clients, and smartphone applications exist. The list is far too large to be listed here, see [Taskwarrior Tools](https://taskwarrior.org/tools/) for a large number of options.

## Configuration

### Environment variables

- `$TASKDATA` — sets the location of the Taskserver's data root directory if it differs from the default. The command line option --data overrides this value.

### Files

Directly editing this file is officially discouraged in favor of the task config command. Further, the fine details of the configuration file itself are in the package's documentation. Please consult man taskrc before making a configuration change.

- /var/lib/taskd/ — the default data directory for Taskserver.
- /var/lib/taskd/config — the Taskserver daemon configuration file. Again, use task config if at all possible.

### Service

#### OpenRC

To add taskd to system startup under the default OpenRC profile:

`root #``rc-update add taskd default`
To start the service manually without requiring a reboot:

`root #``rc-update add taskd start`
#### systemd

To add taskd to system startup under the default systemd profile:

`root #``systemctl enable taskd.service``root #``systemctl start taskd.service`
## Usage

### Invocation

`user $``taskd --help````
Usage: taskd -v|--version
       taskd -h|--help
       taskd diagnostics
       taskd validate <JSON | file>
       taskd help [<command>]
Commands run only on server:
       taskd add     [options] org   <org>
       taskd add     [options] group <org> <group>
       taskd add     [options] user  <org> <user>
       taskd config  [options] [--force] [<name> [<value>]]
       taskd init    [options]
       taskd remove  [options] org   <org>
       taskd remove  [options] group <org> <group>
       taskd remove  [options] user  <org> <user>
       taskd resume  [options] org   <org>
       taskd resume  [options] group <org> <group>
       taskd resume  [options] user  <org> <user>
       taskd server  [options] [--daemon]
       taskd status  [options]
       taskd suspend [options] org   <org>
       taskd suspend [options] group <org> <group>
       taskd suspend [options] user  <org> <user>
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-misc/taskd`
## See also

- [Taskwarrior](https://wiki.gentoo.org/wiki/Taskwarrior) — to-do list manager for the command line
- [Timewarrior](https://wiki.gentoo.org/wiki/Timewarrior) — a time management tool for the terminal.
- [Visual Interactive Taskwarrior](https://wiki.gentoo.org/wiki/Visual_Interactive_Taskwarrior) — curses-based front-end for [Taskwarrior](https://wiki.gentoo.org/wiki/Taskwarrior) with [vim](https://wiki.gentoo.org/wiki/Vim)-like keybindings.
