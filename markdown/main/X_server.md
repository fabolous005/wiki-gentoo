<!-- source: https://wiki.gentoo.org/wiki/X_server | group: Gentoo Wiki (Main) | wiki-title: X server -->
---
title: X server
url: https://wiki.gentoo.org/wiki/X_server
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-04"
fingerprint: ce52f65fa8c2bd86
license: CC BY-SA 4.0
---

# X server

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **X.Org server**, part of the X.Org releases, is the main component of the X Window system which abstracts the hardware and provides the foundation for most graphical user interfaces, like [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) or [window managers](https://wiki.gentoo.org/wiki/Window_manager), and their applications.

Functionality of the X.Org server is handled by [Xwayland](https://wiki.gentoo.org/wiki/Wayland#X-native_app_support_.28XWayland.29) on systems running the [Wayland](https://wiki.gentoo.org/wiki/Wayland) protocol.

## Installation

Installing xorg-server is much lighter than emerging the entire xorg package, and has all the necessary components to have a fully functional GUI such as plasma for example.

### USE flags

### mesa

[Correctly setting](https://wiki.gentoo.org/wiki//etc/portage/make.conf#VIDEO_CARDS) make.conf `VIDEO_CARDS`[Category:Video cards](https://wiki.gentoo.org/wiki/Category:Video_cards) affects the USE expand for [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) (see [Wikipedia](<https://en.wikipedia.org/wiki/Mesa_3D_(OpenGL)>)). Mesa is a graphic library that provides a generic OpenGL implementation and may already be automatically pulled in by graphics card/driver settings in make.conf and if a graphical profile is set. Issue the command emerge --search mesa to see if mesa is already installed prior to emerging.

### xorg-drivers

Portage knows the `X` USE flag for enabling support for X in other packages (default in all *desktop* [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>)). Make sure this USE flag is added to the USE flag list to ensure X compatibility system wide:

**`/etc/portage/make.conf`**

[x11-base/xorg-drivers](https://packages.gentoo.org/packages/x11-base/xorg-drivers) is a meta package to pull in the wanted drivers (note that these driver can be automatically pulled in if the graphics card/drivers info is set in make.conf and using a graphical profile) Issue the command emerge --search xorg-drivers to see if xorg-drivers is already installed prior to emerging.

Make sure to follow [Input devices](https://wiki.gentoo.org/wiki/Category:Input_devices) and [INPUT\_DEVICES](https://wiki.gentoo.org/wiki//etc/portage/make.conf#INPUT_DEVICES).

It is recommended to issue the `--verbose` option when emerging xorg-server because xorg-drivers or mesa may be pulled in as dependencies if they are not already installed. Using `--verbose` will show more information on USE flags and dependencies before package installation. If xorg-drivers and/or the mesa packages are emerged directly (IE without the `--one-shot` option) they will be recorded in the world file and could cause future package upgrade conflicts when Portage is upgrading dependencies. It is a best practice to allow them to be merged into the system as dependencies by setting USE flags or using a graphical [profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>).

### xorg-server

Now install [x11-base/xorg-server](https://packages.gentoo.org/packages/x11-base/xorg-server).


| [+elogind](https://packages.gentoo.org/useflags/+elogind) | Use elogind to get control over framebuffer when running as regular user | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Install a very minimal build (disables, for example, plugins, fonts, most drivers, non-critical features) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [suid](https://packages.gentoo.org/useflags/suid) | Enable setuid root program(s) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Enable libunwind usage for backtraces | 
| [xcsecurity](https://packages.gentoo.org/useflags/xcsecurity) | Build Security extension | 
| [xephyr](https://packages.gentoo.org/useflags/xephyr) | Build the Xephyr server | 
| [xnest](https://packages.gentoo.org/useflags/xnest) | Build the Xnest server | 
| [xorg](https://packages.gentoo.org/useflags/xorg) | Build the Xorg X server (HIGHLY RECOMMENDED) | 
| [xvfb](https://packages.gentoo.org/useflags/xvfb) | Build the Xvfb server | 

`root #``emerge --ask x11-base/xorg-server`
## Configuration

### Permissions

If the [`acl`](https://packages.gentoo.org/useflags/acl) USE flag is enabled globally and [`elogind`](https://packages.gentoo.org/useflags/elogind) is being used (default for desktop profiles) permissions to video cards will be handled automatically. It is possible to check the permissions using getfacl:

`user $``getfacl /dev/dri/card0 | grep larry``user:`**larry**:rw-
A broader solution is to add the user(s) needing access the video card to the video group:

`root #``gpasswd -a larry video`
Note that users will be able to run X without permission to the DRI subsystem, but hardware acceleration will be disabled.

### xorg.conf

The X server is designed to work out-of-the-box, with no need to manually edit Xorg's configuration files. It should detect and configure devices such as displays, keyboards, and mice.

However, the main configuration file of the X server is the [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf).

### Boot service

Usually the X server is started by starting a [display manager](https://wiki.gentoo.org/wiki/Display_manager) automatically on boot.

## See also

- [Non root Xorg](https://wiki.gentoo.org/wiki/Non_root_Xorg) — describes how an unprivileged user can run [Xorg](https://wiki.gentoo.org/wiki/Xorg) without using suid.
- [Xorg](https://wiki.gentoo.org/wiki/Xorg) — an open source implementation of the [X server].
- [Xorg/Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) — explains what Xorg is, how to install it, and the various configuration options.
- [Xrandr](https://wiki.gentoo.org/wiki/Xrandr) — [X](https://wiki.gentoo.org/wiki/X) protocol extension and its CLI tool xrandr are used to manage screen resolutions, rotation and screens with multiply displays in X
