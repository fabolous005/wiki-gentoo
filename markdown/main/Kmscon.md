<!-- source: https://wiki.gentoo.org/wiki/Kmscon | group: Gentoo Wiki (Main) | wiki-title: Kmscon -->
---
title: Kmscon
url: https://wiki.gentoo.org/wiki/Kmscon
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-02"
fingerprint: f641521b09b529e2
license: CC BY-SA 4.0
---

# Kmscon

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Kmscon** is a simple terminal emulator based on Linux kernel mode setting (KMS). It is an attempt to replace the in-kernel VT implementation with a userspace console. Kmscon addresses the limitations of the built-in virtual console by better support for non-Latin characters, better font rendering with anti-aliasing, more fluent DRM backed presentation and better compatibility with HiDPI.

## Installation

### USE flags


| [+drm](https://packages.gentoo.org/useflags/+drm) | Enable Linux DRM for backend | 
| [+fbdev](https://packages.gentoo.org/useflags/+fbdev) | Enable Linux FBDev for backend | 
| [+gles2](https://packages.gentoo.org/useflags/+gles2) | Enable GLES 2.0 (OpenGL for Embedded Systems) support (independently of full OpenGL, see also: gles2-only) | 
| [+libseat](https://packages.gentoo.org/useflags/+libseat) | Enable seat management via sys-auth/seatd | 
| [+pango](https://packages.gentoo.org/useflags/+pango) | Enable pango font rendering | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [freetype](https://packages.gentoo.org/useflags/freetype) | Enable freetype2 renderer | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask sys-apps/kmscon`
## Configuration

It is recommended that the `ERASECHAR` line in /etc/login.defs be commented out for proper backspace functionality at the kmscon login prompt.  Refer to [this Github issue](https://github.com/dvdhrm/kmscon/issues/69#issuecomment-13827797) for details.

### OpenRC

The usage of kmscon on OpenRC is similar to agetty; the following script is based on /etc/init.d/agetty.

**`/etc/init.d/kmsconvt`**

```
#!/sbin/openrc-run
description="KMS System Console"
supervisor="supervise-daemon"
port="${RC_SVCNAME#*.}"
command=/usr/bin/kmscon
command_args="--vt=${port} --no-switchvt"
pidfile="/run/${RC_SVCNAME}.pid"
depend() {
        after local
}
start_pre() {
        if [ "$port" = "$RC_SVCNAME" ]; then
                eerror "${RC_SVCNAME} cannot be started directly. You must create"
                eerror "symbolic links to it for the ports you want to start"
                eerror "kmscon on and add those to the appropriate runlevels."
                return 1
        else
                export EINFO_QUIET="${quiet:-yes}"
        fi
}
stop_pre() {
        export EINFO_QUIET="${quiet:-yes}"
}
```
`root #````
cd /etc/init.d 
```
`root #````
chmod +x ./kmsconvt
```
`root #````
for n in $(seq 1 6); do ln -s kmsconvt kmsconvt.tty$n; rc-update add kmsconvt.tty$n default; done
```
### systemd

`root #``ln -s '/usr/lib/systemd/system/kmsconvt@.service' '/etc/systemd/system/autovt@.service'`
## Usage

Refer to the [kmscon(1)](https://man.archlinux.org/man/kmscon.1.en) [man page for general information about usage.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

GUI environments must be started via the kmscon-launch-gui script, e.g.:

`user $``kmscon-launch-gui startx`
## Removal

Prior to removing kmscon, the system must be reconfigured to return to using agetty; a LiveCD may need to be used for this. Kmscon can then safely be removed.

### OpenRC

#### openrc-init

If you are using [openrc-init](https://wiki.gentoo.org/wiki/OpenRC/openrc-init), you need：

`root #````
cd /etc/init.d
```
`root #``for n in $(seq 1 6); do rc-update del kmsconvt.tty${n} default; cp agetty agetty.tty${n} ; rc-update add agetty.tty${n} default ; done` #### sysvinit

TODO

### systemd

`root #````
systemctl disable autovt@
```
`root #````
ln -s '/usr/lib/systemd/system/getty@.service' '/etc/systemd/system/autovt@.service'
```
## Troubleshooting

### Can't start X / Wayland session on kmscon

An X / Wayland session can't be started directly on kmscon; a [Display manager](https://wiki.gentoo.org/wiki/Display_manager) or kmscon-launch-gui must be used instead, e.g.:

`user $``kmscon-launch-gui sway`
### Unable to switch between different TTYs

1. The familiar combination of `Ctrl`+`Alt`+`F?` from [Xorg](https://wiki.gentoo.org/wiki/Xorg), where *?* should be replaced with the desired TTY number, might work.
2. Check /etc/kmscon/kmscon.conf and verify that it includes:FILE**`/etc/kmscon/kmscon.conf and F? are the F keys`**

### display-manager startup exception after upgrading to kmscon v10.0.0

`kmscon-10.0.0` added and enabled the libseatd support by default, which may causes some exceptions. Refer to [this Github issue](https://github.com/dvdhrm/kmscon/issues/403) for details.

If, after updating kmscon, there are issues such as a `display-manager` service start error or `XDG_RUNTIME_DIR` not getting set, disable seat support by disabling the `libseat` USE flag, or edit /etc/kmscon/kmscon.conf to get the old 9.3.\* behaviour.

**`/etc/kmscon/kmscon.conf`**



## See also

- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).
