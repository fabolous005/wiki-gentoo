<!-- source: https://wiki.gentoo.org/wiki/D-Bus | group: Gentoo Wiki (Main) | wiki-title: D-Bus -->
---
title: D-Bus
url: https://wiki.gentoo.org/wiki/D-Bus
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-19"
fingerprint: a498db3a0d89a984
license: CC BY-SA 4.0
---

# D-Bus

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**D-Bus** is an interprocess communication (IPC) system for software applications. Software makes use of D-Bus to communicate with services and other software.

There are two distinct D-Bus buses: the *system bus* and the *session bus*.

- The *system* bus is for messages related to the system as a whole, e.g. hardware connects and disconnects.
- The *session* bus is for messages related to a specific user session, e.g. an X or Wayland session.

As a result, there are distinct services providing each type of bus; this page describes the details.

For a brief introduction to D-Bus, refer to [D-Bus/background](https://wiki.gentoo.org/wiki/D-Bus/background).

For a list of "well-known bus names and interfaces", refer to [D-Bus/reference](https://wiki.gentoo.org/wiki/D-Bus/reference).

## Installation

### USE flags

The global [dbus](https://packages.gentoo.org/useflags/dbus) [USE flag enables support for D-Bus in packages, and pulls in the](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/dbus](https://packages.gentoo.org/packages/sys-apps/dbus) package. This flag is enabled by default on *desktop* [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>).


### USE flags for
            [sys-apps/dbus](https://packages.gentoo.org/packages/sys-apps/dbus)
            
            A message bus system, a simple way for applications to talk to each other

| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [apparmor](https://packages.gentoo.org/useflags/apparmor) | Enable support for the AppArmor application security system | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 

### Emerge

After enabling the [dbus](https://packages.gentoo.org/useflags/dbus) [global USE flag, be sure to update the system using the](https://wiki.gentoo.org/wiki/USE_flag) `--changed-use`/`-U` option:

`root #``emerge --ask --changed-use --deep @world`
## Configuration

**Todo:**

- This section needs information about systemd setups - whether configuration of either bus is ever required, and if so, the specifics of such configuration(s).

### Files

The main configuration files include:

- /usr/share/dbus-1/system.conf, which defines the "well-known" system bus; and
- /usr/share/dbus-1/session.conf, which defines the "well-known" session bus.

Both allow configuration of security policy, e.g. the method of authentication, which messages can be sent/received, and who can send/receive messages.

### The system bus

#### OpenRC

The OpenRC `dbus` system service provides the *system* bus. It does **not** provide a *session* bus. Depending on system configuration, a *session bus* may also need to be started to enable certain 'desktop' functionality; refer to [the "session bus" section](https://wiki.gentoo.org/wiki/D-Bus#The_session_bus) for details.

To start the D-Bus *system bus*:

`root #``rc-service dbus start`
To start the D-Bus system bus at boot, add it the `default` runlevel:

`root #``rc-update add dbus default`
### The session bus

If using a desktop environment such as [KDE](https://wiki.gentoo.org/wiki/KDE) or [GNOME](https://wiki.gentoo.org/wiki/GNOME), a session bus should be created automatically. However, this is not necessarily the case when using certain [window managers](https://wiki.gentoo.org/wiki/Window_manager) or [compositors](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland#Compositors).

To check whether a session bus is available within an [Xorg](https://wiki.gentoo.org/wiki/Xorg) or [Wayland](https://wiki.gentoo.org/wiki/Wayland) session, open a terminal in that session and run:

`user $``echo $DBUS_SESSION_BUS_ADDRESS`
This should output a string beginning with `unix:path=`, e.g.:

unix:path=/tmp/dbus-a77380e2b9,guid=90c8f55c7e7745be8f35a31b977085f

If no such string is output, there is no D-Bus session bus available to the session.

#### OpenRC

A `dbus` user service is available, in addition to the `dbus` system service.

To add the service to the `default` user runlevel:

`user $``rc-update --user add dbus default`
To start the service:

`user $``rc-service --user dbus start`
The `dbus` user service creates a session bus at the address `unix:path=${XDG_RUNTIME_DIR}/bus`.

Thus, for example, the `emacs` user service needs to be configured (e.g. via \~/.config/rc/conf.d/emacs) to set `DBUS_SESSION_BUS_ADDRESS`:

```
 export DBUS_SESSION_BUS_ADDRESS="unix:path=${XDG_RUNTIME_DIR}/bus"
```
#### Manual

In general, to manually start a D-Bus session bus, the window manager or compositor should be started via [dbus-run-session(1)](https://man.archlinux.org/man/dbus-run-session.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

The session bus thus created will *only* be visible to programs created as child processes of the GUI started by dbus-run-session. Consequently, any programs needing access to the session bus must be started via the GUI's configuration. Refer to the GUI's documentation for details.

For example, on [X](https://wiki.gentoo.org/wiki/X), if [i3](https://wiki.gentoo.org/wiki/I3) is started via \~/.xinitrc, then that file should be modified to have as its *last* line:

**`~/.xinitrc`**

```
exec dbus-run-session /usr/bin/i3
```
and any programs needing access to that session bus must be started via \~/.config/i3/config.

On [Wayland](https://wiki.gentoo.org/wiki/Wayland) systems, the compositor should be launched via dbus-run-session, e.g. in a start script:

**`~/.local/bin/start-sway`**

```
#!/bin/sh
dbus-run-session /usr/bin/sway
```
## Usage

Some basic commands include:

- dbus-monitor --system - To monitor activity in the system bus.
- dbus-monitor --session - To monitor activity in the session bus.
- dbus-send \<arguments> - To send a D-Bus message. Details below.

The following examples make use of [dbus-send(1)](https://man.archlinux.org/man/dbus-send.1.en)[, from the reference implementation. The messages used in the following examples are described more fully in](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [the "Message Bus Messages" section](https://dbus.freedesktop.org/doc/dbus-specification.html#message-bus-messages) of the specification.

To list available D-Bus services:

`user $``dbus-send --print-reply --dest=org.freedesktop.DBus /org/freedesktop/DBus org.freedesktop.DBus.ListNames`
This command says to send the message `org.freedesktop.DBus.ListNames` to the
`/org/freedesktop/DBus` object on `org.freedesktop.DBus`.

By default, dbus-send uses the session bus. Use `--system` to use the system bus.

To list all names that can be activated:

`user $``dbus-send --print-reply --dest=org.freedesktop.DBus /org/freedesktop/DBus org.freedesktop.DBus.ListActivatableNames`
To return a boolean indicating whether a name has an owner:

`user $``dbus-send --print-reply --dest=org.freedesktop.DBus /org/freedesktop/DBus org.freedesktop.DBus.NameHasOwner string:'org.freedesktop.Notifications'`
The above command passes a message argument: `string:'org.freedesktop.Notifications'`. The dbus-send interface requires that message arguments be specified in the form `<type>:<data>`.

To return the PID of the process that owns a name, if one exists:

`user $``dbus-send --print-reply --dest=org.freedesktop.DBus /org/freedesktop/DBus org.freedesktop.DBus.GetConnectionUnixProcessID string:'org.freedesktop.Notifications'`
To shut down and reboot as a regular user when using [elogind](https://wiki.gentoo.org/wiki/Elogind):

`user $``dbus-send --system --print-reply --dest=org.freedesktop.login1 /org/freedesktop/login1 org.freedesktop.login1.Manager.PowerOff boolean:false``user $``dbus-send --system --print-reply --dest=org.freedesktop.login1 /org/freedesktop/login1 org.freedesktop.login1.Manager.Reboot boolean:false`
Changing the last argument to `boolean:true` should make [polkit](https://wiki.gentoo.org/wiki/Polkit) interactively ask the user for authentication credentials if necessary.

The [dev-debug/d-spy](https://packages.gentoo.org/packages/dev-debug/d-spy) package provides a GUI to explore available D-Bus services and objects on the system and session buses, and to call methods of D-Bus interfaces.

A bus name can be monitored with [gdbus(1)](https://man.archlinux.org/man/gdbus.1.en)[, e.g.:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``gdbus monitor --session --dest 'org.freedesktop.Notifications' --object-path /org/freedesktop/Notifications`
Users of [systemd](https://wiki.gentoo.org/wiki/Systemd) or elogind can also use [busctl(1)](https://man.archlinux.org/man/busctl.1.en) [to list objects on a given bus, e.g.:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``busctl --user tree````
Service org.freedesktop.DBus:
└─/org/freedesktop/DBus
Service org.freedesktop.Notifications:
└─/org
  └─/org/freedesktop
    └─/org/freedesktop/Notifications
...
```
## See also

- [Eudev](https://wiki.gentoo.org/wiki/Eudev) — a fork of [udev](https://wiki.gentoo.org/wiki/Udev), [systemd](https://wiki.gentoo.org/wiki/Systemd)'s [device file](https://wiki.gentoo.org/wiki/Device_file) manager for the Linux kernel.
- [Udev](https://wiki.gentoo.org/wiki/Udev) — [systemd's](https://wiki.gentoo.org/wiki/Systemd) device manager for the Linux kernel.

## External resources

- [Introduction to D-Bus](https://www.freedesktop.org/wiki/IntroductionToDBus/) (freedesktop.org)
- [D-Bus tutorial](https://dbus.freedesktop.org/doc/dbus-tutorial.html) (freedesktop.org)
- [D-Bus](https://wiki.archlinux.org/index.php/D-Bus) (Arch Wiki)
