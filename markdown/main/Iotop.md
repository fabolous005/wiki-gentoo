<!-- source: https://wiki.gentoo.org/wiki/Iotop | group: Gentoo Wiki (Main) | wiki-title: Iotop -->
---
title: iotop
url: https://wiki.gentoo.org/wiki/Iotop
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-01"
fingerprint: "8e153c19e3b12bf8"
license: CC BY-SA 4.0
---

# iotop

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**iotop** is a top-like utility that monitors system **i**nput/**o**utput.

## Installation

### Kernel

The `CONFIG_TASK_IO_ACCOUNTING` variable must be enabled in the Linux kernel for the iotop command to work properly. It can be found at this location:

**Kernel configuration for iotop**

General setup -> CPU/Task time and stats accounting

\[\*\]   Enable per-task delay accounting [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_TASK\_DELAY\_ACCT\</code> to find this item.
  \[\*\]   Enable extended accounting over taskstats [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_TASK\_XACCT\</code> to find this item.
  \[\*\]     Enable per-task storage I/O accounting [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_TASK\_IO\_ACCOUNTING\</code> to find this item.
  \[\*\] Enable VM event counters for /proc/vmstat (EXPERT)

Additionally, "delayacct" must be added to the kernel command-line such as `GRUB_CMDLINE_LINUX` for grub.

### Emerge

Install the package:

`root #``emerge --ask sys-process/iotop`
## Usage

### Invocation

`root #``iotop --help`
Usage: iotop \[OPTIONS\]
DISK READ and DISK WRITE are the block I/O bandwidth used during the sampling
period. SWAPIN and IO are the percentages of time the thread spent respectively
while swapping in and waiting on I/O more generally. PRIO is the I/O priority at
which the thread is running (set using the ionice command).
Controls: left and right arrows to change the sorting column, r to invert the
sorting order, o to toggle the --only option, p to toggle the --processes
option, a to toggle the --accumulated option, i to change I/O priority, q to
quit, any other key to force a refresh.
Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -o, --only            only show processes or threads actually doing I/O
  -b, --batch           non-interactive mode
  -n NUM, --iter=NUM    number of iterations before ending \[infinite\]
  -d SEC, --delay=SEC   delay between iterations \[1 second\]
  -p PID, --pid=PID     processes/threads to monitor \[all\]
  -u USER, --user=USER  users to monitor \[all\]
  -P, --processes       only show processes, not all threads
  -a, --accumulated     show accumulated I/O instead of bandwidth
  -k, --kilobytes       use kilobytes instead of a human friendly unit
  -t, --time            add a timestamp on each line (implies --batch)
  -q, --quiet           suppress some lines of header (implies --batch)

## See also

- [top](https://wiki.gentoo.org/index.php?title=Top&action=edit&redlink=1) - A utility that displays Linux processes.
- [atop](https://wiki.gentoo.org/index.php?title=Atop&action=edit&redlink=1) - A top-like interactive monitor that displays load on the Linux system.
- [htop](https://wiki.gentoo.org/wiki/Htop) - Another top-like clone that displays Linux processes.
- [Nload](https://wiki.gentoo.org/wiki/Nload) — a super simple, command-line network interface monitoring tool.
