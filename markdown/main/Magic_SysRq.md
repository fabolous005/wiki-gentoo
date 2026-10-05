<!-- source: https://wiki.gentoo.org/wiki/Magic_SysRq | group: Gentoo Wiki (Main) | wiki-title: Magic SysRq -->
---
title: Magic SysRq
url: https://wiki.gentoo.org/wiki/Magic_SysRq
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-17"
fingerprint: bc13bb0c0ca913a9
license: CC BY-SA 4.0
---

# Magic SysRq

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Magic SysRq** (Magic System Request) is a kernel hack that enables the kernel to listen to specific key presses and respond by calling a specific kernel function. Magic SysRq is activated via input from the keyboard or a serial line.

## Kernel

A basic Magic SysRq configuration:

**Enable Magic SysRq (`CONFIG_MAGIC_SYSRQ` and `CONFIG_MAGIC_SYSRQ_DEFAULT_ENABLE` respectively)**

## Usage

### Invocation

On **amd64** and **x86** systems the key combination of `Alt`+`SysRq`+\<command key> will result in Magic SysRQ invocation. See the following table for some possible options:

| Command key | Description | 
|---|---|
| `b` | Immediately reboot the system without syncing or unmounting the disks. | 
| `e` | Send a SIGTERM to all processes, except for init. | 
| `f` | Calls the OOM killer to kill a memory hog process; does not panic if nothing can be killed. | 
| `s` | Attempts to sync all mounted filesystems. | 
| `u` | Attempts to remount all mounted filesystems read-only. | 

More information can be found in the [official Magic SysRQ Linux Kernel documentation](https://www.kernel.org/doc/html/latest/admin-guide/sysrq.html).

## External resources

- [Magic SysRq Key](http://www.linuxhowtos.org/Tips%20and%20Tricks/sysrq.htm) at LinuxHowtos.org.
- [Wikipedia:System request](https://en.wikipedia.org/wiki/System_request) - History on the system request key.
