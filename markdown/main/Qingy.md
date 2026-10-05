<!-- source: https://wiki.gentoo.org/wiki/Qingy | group: Gentoo Wiki (Main) | wiki-title: Qingy -->
---
title: qingy
url: https://wiki.gentoo.org/wiki/Qingy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-01-28"
fingerprint: be09b97959a33394
license: CC BY-SA 4.0
---

# qingy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Qingy** (**Q**ingy **I**s **N**ot **G**ett**Y**) is a replacement for getty. Written in C, it uses DirectFB to provide a GUI without the overhead of the X Windows System. It allows the user to log in and start the session of choice (text console, GNOME, KDE, wmaker, etc.).

## Installation

### USE flags


| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [crypt](https://packages.gentoo.org/useflags/crypt) | Add support for encryption -- using mcrypt or gpg where applicable | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [gpm](https://packages.gentoo.org/useflags/gpm) | Add support for sys-libs/gpm (Console-based mouse driver) | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 

### Emerge

`root #``emerge --ask qingy`
## Configuration

### Keypair

qingy requires keypairs to run. To generate keys:

`root #``qingy-keygen`
### inittab

After successful installation edit the /etc/inittab file and replace following section:

**`/etc/inittab`**

with following entries:

**`/etc/inittab`**

### Configuration file

This is default qingy's configuration as shipped with Gentoo:

**`/etc/qingy/settings`**

## Display managers

Remove xdm and display-manager from the default startup level, otherwise they will fight with qingy for screen control at system boot. This sometimes results in nasty results.

`root #``rc-update del xdm default``root #``rc-update del display-manager default`
## Starting qingy

Now either reboot the system or use following commands:

`root #````
init Q
```
`root #``killall agetty`
After successful authentication qingy will list contents of /etc/X11/Xsession/ directory:

```
 Welcome, ${LOGNAME}, please select a session...
 (a) dwm
 (b) fvwm
 (c) Your .xsession
 (d) Text: Console
 Your choice (just press ENTER for 'Text: Console'):
```
Different Xsessions can be started in each tty, which works fine. Use the `Ctrl`+`Alt`+`F1` through `F6` key combinations to switch between different X sessions.

## Troubleshooting

If qingy hangs making it impossible to login press `Ctrl`+`Alt`+`F6` to get the agetty spawned terminal and login from there.
