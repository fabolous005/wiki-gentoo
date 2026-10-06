<!-- source: https://wiki.gentoo.org/wiki/Dhcpcd-ui | group: Gentoo Wiki (Main) | wiki-title: Dhcpcd-ui -->
---
title: dhcpcd-ui
url: https://wiki.gentoo.org/wiki/Dhcpcd-ui
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-30"
fingerprint: c211055e9d8a596e
license: CC BY-SA 4.0
---

# dhcpcd-ui

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**dhcpcd-ui** is a Qt and GTK monitor and configuration graphical user interface for [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd).

## Installation

### USE flags

To get one of the graphical user interfaces enable the respective USE flag.


### USE flags for
            [net-misc/dhcpcd-ui](https://packages.gentoo.org/packages/net-misc/dhcpcd-ui)
            
            Desktop notification and configuration for dhcpcd

| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [libnotify](https://packages.gentoo.org/useflags/libnotify) | Enable desktop notification support | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support (console display library) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

Support for `qt4` has been removed with commit [43f5c510450633d249f596fcc4f74255df76bb73](https://gitweb.gentoo.org/repo/gentoo.git/commit/net-misc/dhcpcd-ui?id=43f5c510450633d249f596fcc4f74255df76bb73) [bug #630638](https://bugs.gentoo.org/show_bug.cgi?id=630638).

### Emerge

Install dhcpcd-ui:

`root #``emerge --ask net-misc/dhcpcd-ui`
### Building from source

Alternatively, for installing the bleeding edge<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> of dhcpcd-ui you can use the live ebuild from the bar overlay<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

`root #````
emerge --ask --noreplace app-eselect/eselect-repository dev-vcs/git
```
`root #````
eselect repository enable bar
```
`root #````
emerge --sync bar
```
`root #````
emerge --ask =net-misc/dhcpcd-ui-9999:bar
```
`root #````
emerge --ask --deselect net-misc/dhcpcd
```
## Configuration

Uncomment the `controlgroup` line in /etc/dhcpcd.conf:

FILE **`/etc/dhcpcd.conf`**

```
# Allow users of this group to interact with dhcpcd via the control socket.
controlgroup wheel
```
Change group and permissions of /etc/dhcpcd.conf in order to make it writable for the user interface:

`root #````
chgrp wheel /etc/dhcpcd.conf
```
`root #````
chmod g+w /etc/dhcpcd.conf
```


## Removal

Uninstall dhcpcd-ui:

`root #``emerge --ask --depclean --verbose net-misc/dhcpcd-ui`
