<!-- source: https://wiki.gentoo.org/wiki/Logging | group: Gentoo Wiki (Main) | wiki-title: Logging -->
---
title: Logging
url: https://wiki.gentoo.org/wiki/Logging
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-19"
fingerprint: a018f83a06878f8c
license: CC BY-SA 4.0
---

# Logging

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Logging** is the process by which systems record notable events to provide users and administrators with a view of what is happening while a machine is running. Many programs/daemons generate logs according to the [syslog](https://en.wikipedia.org/wiki/Syslog) specification that can be marshaled by a system wide logging tool - other software may manage it's own log files.

Gentoo provides a choice of logging systems, to be installed by the administrator. System logs can be used to monitor activity, or to alert users of particular events, depending on what logger is chosen, and how it is set up.

System logs are often a vital tool for system security.

## Available software

This is just a partial selection of logging tools available in Gentoo.

| Name | Package | Description | 
|---|---|---|
| [Metalog](https://wiki.gentoo.org/wiki/Metalog) | [app-admin/metalog](https://packages.gentoo.org/packages/app-admin/metalog) | Highly configurable replacement for syslogd/klogd. | 
| [Rsyslog](https://wiki.gentoo.org/wiki/Rsyslog) | [app-admin/rsyslog](https://packages.gentoo.org/packages/app-admin/rsyslog) | Open-source system for high performance log processing. | 
| [Sysklogd](https://wiki.gentoo.org/wiki/Sysklogd) | [app-admin/sysklogd](https://packages.gentoo.org/packages/app-admin/sysklogd) | Standard log daemons. | 
| [Syslog-ng](https://wiki.gentoo.org/wiki/Syslog-ng) | [app-admin/syslog-ng](https://packages.gentoo.org/packages/app-admin/syslog-ng) | Powerful, highly configurable monitoring and logging daemon. | 
| [Systemd](https://wiki.gentoo.org/wiki/Systemd#Handling_of_log_files) | [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) | Init system that has its own way of handling log files. | 

## Linux system log files

Generally, all system logs are found in the /var/log/ directory. Logging file names may vary, example files of interest:

- /var/log/auth.log : user connection log
- /var/log/dmesg : diagnostic messages, see also [dmesg](https://wiki.gentoo.org/wiki/Util-linux)
- /var/log/emerge.log : events when managing packages with [Portage](https://wiki.gentoo.org/wiki/Portage), see also [Portage log](https://wiki.gentoo.org/wiki/Portage_log)
- /var/log/kern.log : kernel event log
- /var/log/syslog : all system messages
- /var/log/Xorg.1.log : [xorg](https://wiki.gentoo.org/wiki/Xorg) log, useful when diagnosing xorg startup issues

## See also

- [logcheck](https://wiki.gentoo.org/wiki/Logcheck) — tool to analyze the system logs.
- [Logrotate](https://wiki.gentoo.org/wiki/Logrotate) — a tool to periodically rotate (archive), delete, and optionally compress and/or mail historic log files.
- [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) — a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system -- OpenRC generates some of the first output seen on the screen during boot - this can be logged to a file.
- [Portage log](https://wiki.gentoo.org/wiki/Portage_log) — provides information when installing, updating, or removing packages.
