<!-- source: https://wiki.gentoo.org/wiki/Keyd | group: Gentoo Wiki (Main) | wiki-title: Keyd -->
---
title: keyd
url: https://wiki.gentoo.org/wiki/Keyd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-19"
fingerprint: "9ec1d159af85b9c0"
license: CC BY-SA 4.0
---

# keyd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**keyd** is a key remapping daemon that allows the user to map keys both per-application and globally, with support for both [Xorg](https://wiki.gentoo.org/wiki/Xorg) and [Wayland](https://wiki.gentoo.org/wiki/Wayland).

## Installation

### Kernel

keyd relies on the `uinput` kernel module to create virtual input devices in userspace.

**Enable support for keyd**

```
Device Drivers --->
  Input device support --->
    [*] Miscellaneous devices 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT_MISC</code> to find this item.
      <M|*> User level driver support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT_UINPUT</code> to find this item.
### Emerge

keyd is currently only available in the [GURU](https://wiki.gentoo.org/wiki/GURU) [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository). If the overlay is not already enabled, run:

`root #````
eselect repository enable guru
```
`root #````
emaint sync -r guru
```
After activating the overlay, [app-misc/keyd](https://packages.gentoo.org/packages/app-misc/keyd) can be installed:

`root #``emerge --ask app-misc/keyd`
## Configuration

keyd is distributed with a script, keyd-application-mapper, for usage in GUI environments: it remaps keys when window focus changes.

Any user wanting to use this script will need access to /var/run/keyd.socket, which can be provided by adding that user (e.g. `larry`) to the `keyd` group:

`root #``usermod -aG keyd larry`
### System-wide configuration

The system-wide configuration file, /etc/keyd/default.conf, defines "layers", as described by the [keyd(1)](https://man.archlinux.org/man/keyd.1.en) [man page:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

A layer is a collection of bindings, each of which specifies the behavior of a particular key. Multiple layers may be active at any given time, forming a stack of occluding keymaps consulted in activation order. The default layer is called `main` and is where common bindings should be defined.


### Per-application configuration

For per-application configuration, use keyd-application-mapper and its associated configuration file, \~/.config/keyd/app.conf. Configuration specified in this file will be added to that specified by the system-wide configuration file, possibly overriding parts of it. Bindings in this file generally take the format required by the `bind` command, `<layer>.<key>`; refer to the [keyd(1)](https://man.archlinux.org/man/keyd.1.en) [and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [keyd-application-mapper(1)](https://man.archlinux.org/man/keyd-application-mapper.1.en) [man pages for details.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

### Chords

Chords - that is, groups of keys which are treated as a unit when simultaneously depressed - are configured by using the `+` operator on the left-hand side of a binding. For example, to make `j`+`k` produce `Esc`:

**`/etc/keyd/default.conf`**

```
j+k = esc
```
### Multiple actions per binding

If multiple actions are required in response to the use of a binding, use the `macro()` action to specify several expressions separated by spaces.

If a macro call is needed before a call to an internal action, e.g. `clear()`, use the `*m()` form of that internal action. For example, `clearm()` can be used to call a macro before calling the `clear()` action.

### Two modifiers with a key

Each modifier key has its own layer<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

control = layer(control)
meta = layer(meta)
shift = layer(shift)
leftalt = layer(alt)
rightalt = layer(altgr)

Thus, to use two modifiers with a particular key, specify a combined layer using the `+` operator, e.g.:

**`~/.config/keyd/app.conf`**

```
alt+shift.v = C-end
```
### Clear other layer

**Todo:**

- Clarify what is meant by this sentence.

Only "clearm()" working for current stable version, that breaks all layers.

### Switching between virtual consoles

When switching between [virtual consoles](https://wiki.gentoo.org/wiki/Terminal_emulator#Virtual_consoles_and_switching) and wanting to reset bindings to their defaults, use the `command()` macro together with [chvt(1)](https://man.archlinux.org/man/chvt.1.en)[, provided by](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [sys-apps/kbd](https://packages.gentoo.org/packages/sys-apps/kbd) (part of the **@system** set):

**`/etc/keyd/default.conf`**

```
[control+alt]
f6 = command(keyd bind reset && chvt 6)
```
### Examples

#### Running keyd-application-mapper with DEBUG in Sway with doas

**`/etc/doas.conf`**

```
permit setenv { WAYLAND_DISPLAY XDG_RUNTIME_DIR KEYD_DEBUG } nopass larry cmd /usr/bin/keyd-application-mapper
```
**`/home/larry/.config/sway/config`**

```
exec /bin/bash -c 'KEYD_DEBUG=True doas /usr/bin/keyd-application-mapper' >> /home/larry/.config/keyd/app.log &
```
#### Emacs keys for Firefox

**`/etc/keyd/default.conf`**

```
[ids]
*
 
# - Firefox copy text
[control+alt]
w = C-w
 
[alt+shift]
dot = C-end
comma = C-home
 
[control_x]
h = C-a
```
**`~/.config/keyd/app.conf`**

```
[firefox-esr|*]
# Firefox
alt.x = C-l
alt.a = C-pageup
alt.e = C-pagedown
control.s = C-f
# - Firefox built-in:
# control.[ - back
# control.] - forward
# F3 - search forward
# shift.F3 - search backward
 
# - enter, escape
control.g = clearm(escape)
control.m = enter
# - begin, end of string
control.a = home
control.e = end
# - edit
control.h = backspace
control.d = delete
control.l = left
control.f = right
control.n = down
control.k = up
alt.l = C-left
alt.f = C-right
control.slash = C-z
# - scroll
alt.v = pageup
control.v = pagedown
alt.z = up
control.z = down
# - copy, paste
control.w = clearm(C-x)
alt.w = clearm(C-c)
control.y = clearm(C-v)
alt.c = C-v
# - selection mode - remember: C+h and C+d don't work with
control.space = toggle(shift)
control.c = clearm(C-c)
 
# - select all
control.x = oneshot(control_x)
```
### Service

The system-wide configuration is enabled via the `keyd` service.

#### OpenRC

Add the `keyd` service to the `default` runlevel.

`root #``rc-update add keyd default`
Start the service:

`root #``rc-service keyd start`
## Usage

### Security concerns

When a user is made a member of the `keyd` group so as to be able to access /var/run/keyd.socket, it allows them to send keys system-wide. This creates a vulnerability that might be exploited by any malicious program within that user's environment. This vulnerability affects all uinput-based applications that allow arbitrary keys to be sent from userspace.

#### Wayland

This vulnerability may be closed if we run script as root with Wayland environment variables:

`user $````
sudo --preserve-env=WAYLAND_DISPLAY,XDG_RUNTIME_DIR /usr/bin/keyd-application-mapper
```
For this command add sudo permission for current user:

**`/etc/sudoers`**

```
larry ALL=(root) SETENV:NOPASSWD:/usr/bin/keyd-application-mapper
```
Copy user configuration to root folder:

`root #````
mkdir -p /root/.config/keyd
```
`root #````
cp /home/larry/.config/keyd/app.conf /root/.config/keyd/app.conf
```
Withdraw permissions by removing user from `keyd` group:

`root #``gpasswd -d larry keyd`
## External resources

- [Key remapper for X Window System on Linux](https://github.com/kawao/x-set-keys)
- [Pythonic API to Linux uinput module](https://github.com/pyinput/python-uinput)
- [\[wip](https://github.com/acro5piano/sway-remap) keyboard remapper tool for Sway window manager\]
