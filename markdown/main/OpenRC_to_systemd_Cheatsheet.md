<!-- source: https://wiki.gentoo.org/wiki/OpenRC_to_systemd_Cheatsheet | group: Gentoo Wiki (Main) | wiki-title: OpenRC to systemd Cheatsheet -->
---
title: OpenRC to systemd Cheatsheet
url: https://wiki.gentoo.org/wiki/OpenRC_to_systemd_Cheatsheet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-09"
fingerprint: "9768e1c5b14480e7"
license: CC BY-SA 4.0
---

# OpenRC to systemd Cheatsheet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is for users that have recently converted from [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) to [systemd](https://wiki.gentoo.org/wiki/Systemd). It contains a list of commands commonly used in OpenRC and its equivalent systemd command.

| Command | OpenRC | systemd | Comments | 
|---|---|---|---|
| Start a service | /etc/init.d/\<service> start rc-service \<service> start | systemctl start \<service> |  | 
| Stop a service | /etc/init.d/\<service> stop rc-service \<service> stop | systemctl stop \<service> |  | 
| Restart a service | /etc/init.d/\<service> restart rc-service \<service> restart | systemctl restart \<service> |  | 
| Get service status | /etc/init.d/\<service> status rc-service \<service> status | systemctl status \<service> |  | 
| Show known startup scripts | rc-status rc-update show | systemctl list-units | Shows scripts that exist in runlevels | 
| Show all startup scripts | ls /etc/init.d/ rc-update -v show | systemctl list-unit-files --type=service | Shows all installed scripts | 
| Enable service at startup | rc-update add \<service> \<runlevel> | systemctl enable \<service> |  | 
| Disable service at startup | rc-update del \<service> \<runlevel> | systemctl disable \<service> |  | 
| Disable service altogether | chmod 444 /etc/init.d/\<service> | systemctl mask \<service> | The service cannot be run until *unmasked*. | 
| Cancels 'masking' service | chmod 555 /etc/init.d/\<service> | systemctl unmask \<service> |  | 

The following table is a list of useful systemd commands that have no OpenRC equivalent:

| Command | Syntax | Comments | 
|---|---|---|
| To be run after creating or modifying services | systemctl daemon-reload |  | 
| Kill all processes related to service | systemctl kill \<service> |  | 
| Show logs events that happened today, most recent events first | journalctl -r --since=today |  | 
| Show log events for a specific service | journalctl -u \<service> |  | 

## Runlevels

In systemd, what correpsponds to runlevels are called "targets", but the latter is more general. Services can be set to run before and after targets. In particular, `runlevel<N>.target` (N = 0...6) roughly mimicks runlevels.

See [Systemd#Targets](https://wiki.archlinux.org/title/Systemd#Targets) in Arch Wiki for introduction. All pre-defined targets are found in the man page [systemd.special (7)](https://www.man7.org/linux/man-pages/man7/systemd.special.7.html).

## /etc/local.d

In OpenRC, scripts [`/etc/local.d/*start`](https://wiki.gentoo.org/index.php?title=OpenRC_to_systemd_Cheatsheet/etc/local.d&action=edit&redlink=1) are started at system startup and `*stop` are run at shutdown. To port them to systemd with minimal changes, one can create the service `local.d.start`:

**`/etc/systemd/system/local.d.start.service`**

Here we chose runlevel3 as the target, but one can change this. This service will run the script `/for/bar/baz`, which should look like:

**`/foo/bar/baz`**

The path of /foo/bar/baz is not standardized. Some suggest `/usr/local/bin`. /usr/local is [for sysadmin installing something locally](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04s09.html), according to FHS.  (The contributor of this section personally thinks it is too general, and would like to suggest something like `/usr/local/libexec/systemd/` for example.)

Similarly one can create a \*stop service. They have to be enabled to take effect.

- [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) — a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system
- [Systemd](https://wiki.gentoo.org/wiki/Systemd) — a modern SysV-style init and [rc](https://wiki.gentoo.org/wiki/Rc) replacement for Linux systems.
