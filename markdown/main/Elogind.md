<!-- source: https://wiki.gentoo.org/wiki/Elogind | group: Gentoo Wiki (Main) | wiki-title: Elogind -->
---
title: elogind
url: https://wiki.gentoo.org/wiki/Elogind
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-29"
fingerprint: "8013fb5241a173c4"
license: CC BY-SA 4.0
---

# elogind

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**elogind** is  the [systemd](https://wiki.gentoo.org/wiki/Systemd) project's [*logind*](https://en.wikipedia.org/wiki/Systemd#logind), extracted to a standalone package. It's designed for users who prefer a non-systemd [init system](https://wiki.gentoo.org/wiki/Init_system), but still want to use popular software such as [KDE](https://wiki.gentoo.org/wiki/KDE) or [GNOME](https://wiki.gentoo.org/wiki/GNOME) that otherwise hard-depends on systemd.

## Installation

### Kernel

The following kernel options are recommended:

```
General setup  --->
    [*] Control Group support  --->
File systems  --->
    [*] Inotify support for userspace
```
In the unlikely (and not recommended) event that standard kernel features are enabled for manual configuration, elogind also requires `eventpoll`, `signalfd()` and `timerfd()` support. Most users can ignore this.

### USE flags


### USE flags for
            [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind)
            
            The systemd project's logind, extracted to a standalone package

| [+acl](https://packages.gentoo.org/useflags/+acl) | Add support for Access Control Lists | 
| [+pam](https://packages.gentoo.org/useflags/+pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [+policykit](https://packages.gentoo.org/useflags/+policykit) | Enable PolicyKit (polkit) authentication support | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [cgroup-hybrid](https://packages.gentoo.org/useflags/cgroup-hybrid) | Use hybrid cgroup hierarchy instead of unified (OpenRC's default). | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

There is a global `elogind` USE flag for enabling elogind support in other packages. It's also recommended to disable support for other session trackers (`systemd`) to avoid conflicts:

**`/etc/portage/make.conf`**

```
USE="elogind -systemd"
```
### Emerge

After updating the USE flags update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
## Configuration

### Service

elogind should be configured to start at boot time:

`root #``rc-update add elogind boot`
When [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) is installed with the `USE="elogind"` flag, starting elogind on boot triggers the dbus system daemon to load automatically.

Alternatively, elogind can be launched on-demand by the first program that requests it (like a compatible [display manager](https://wiki.gentoo.org/wiki/Display_manager)), through the system dbus service.

Additionally, if built with the [pam](https://packages.gentoo.org/useflags/pam) [USE flag](https://wiki.gentoo.org/wiki/USE_flag), elogind will be activated when the first user logs into the system.

### startx D-Bus integration

To have an elogind session created when using startx to start the X server (instead of a [display manager](https://wiki.gentoo.org/wiki/Display_manager)), add the following to the user's \~/.xinitrc file:

**`~/.xinitrc`**

```
exec dbus-run-session <WINDOW_MANAGER>
```
`WINDOW_MANAGER` in the above example needs to be replaced by a [window manager](https://wiki.gentoo.org/wiki/Window_manager) or a single application.

### Suspend/Hibernate Resume/Thaw hook scripts

With elogind the situation is much handier. Any suspend/resume and hibernate/thaw hook scripts need to be in the directory /etc/elogind/system-sleep/ and use the variables `$1` (`pre` or `post`) and `$2` (`suspend`, `hibernate`, or `hybrid-sleep`). For example, in the case of elogind a hook script could have the following format:

**`/etc/elogind/system-sleep/example.sh`**

**An example of elogind hook**

```
#!/bin/bash
case $1/$2 in
  pre/*)
    # Put here any commands expected to be run when suspending or hibernating.
    ;;
  post/*)
    # Put here any commands expected to be run when resuming from suspension or thawing from hibernation.
    ;;
esac
```
Do not forget to make the hook scripts executable:

`root #``chmod +x /etc/elogind/system-sleep/example.sh`
### elogind.conf

Other automatic actions can be configured through /etc/elogind/logind.conf. For example, to disable suspend on laptop lid close,

**`/etc/elogind/logind.conf.d/lid.conf`**

**Disable suspend on laptop lid close**

```
[Login]
HandleLidSwitch=ignore
```
Make sure to reload the loginctl configuration following changes.

`root #``loginctl reload`
## Usage

### loginctl

The command loginctl may be used to control and introspect the login manager. For example, to shut down or reboot the system:

`user $``loginctl poweroff``user $``loginctl reboot`
For example, to suspend, hibernate or hybrid-suspend the system:

`user $``loginctl suspend``user $``loginctl hibernate``user $``loginctl hybrid-sleep`
To suspend the system and then hibernate after a period of inactivity while the system is suspended:

`user $``loginctl suspend-then-hibernate`
where hibernation delay can be specified in /etc/elogind/logind.conf.

## Troubleshooting

### Confirmation of full functionality

Running loginctl itself will indicate ALL sessions/seats/users/tty's for which elogind has been fully activated. For example:

`user $``loginctl````
SESSION  UID USER      SEAT  TTY 
      1    0 root      seat0 tty1
      2 1000 larry     seat0 tty2
 
2 sessions listed.
```
Checking for the presence of XDG environment variables should produce similar results, even before a GUI is loaded. For example:

`user $``env | grep "XDG"` XDG\_CONFIG\_DIRS=/etc/xdg
XDG\_SEAT=seat0
XDG\_SESSION\_TYPE=tty
XDG\_SESSION\_CLASS=user
XDG\_VTNR=2
XDG\_SESSION\_ID=2
XDG\_RUNTIME\_DIR=/run/user/1000
XDG\_DATA\_DIRS=/usr/local/share:/usr/share

### Conflict when using hidepid in proc

When [procfs is mounted](https://wiki.gentoo.org/wiki/Procfs#Restricting_access_to_PID_directories) with `hidepid=2` and `gid=wheel`, there will be conflicts with elogind. In order to change this, the gid needs to be changed to `gid=polkitd`.

See also this forum post [https://forums.gentoo.org/viewtopic-t-1099870.html](https://forums.gentoo.org/viewtopic-t-1099870.html)

### PAM

If using [pam](https://packages.gentoo.org/useflags/pam)[, make sure there are no conflicting pending changes waiting to be written to /etc (run dispatch-conf to merge any /etc/pam.d conflicts).](https://wiki.gentoo.org/wiki/USE_flag)

Confirm these changes took place in these two /etc/pam.d files:

`user $``grep -r "elogind" /etc/pam.d/` /etc/pam.d/elogind-user: session optional pam\_elogind.so
/etc/pam.d/system-login: -session        optional        pam\_elogind.so

## External resources

- [bug #599470](https://bugs.gentoo.org/show_bug.cgi?id=599470) - [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) integration into Gentoo tracker bug.
- [News item - Desktop profile switching USE default to elogind](https://gentoo.org/support/news-items/2020-04-14-elogind-default.html)
