<!-- source: https://wiki.gentoo.org/wiki/Keyd | group: Gentoo Wiki (Main) | wiki-title: Keyd -->
---
title: keyd
url: https://wiki.gentoo.org/wiki/Keyd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-29"
fingerprint: bfc194712d85f9cc
license: CC BY-SA 4.0
---

# keyd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**keyd** is a key remapping daemon that allows the user to map keys per application and globally with support for both [Xorg](https://wiki.gentoo.org/wiki/Xorg) and [Wayland](https://wiki.gentoo.org/wiki/Wayland).

## Installation

### Kernel

keyd relies on the uinput kernel module to create virtual input devices in userspace.

**Enable support for keyd**

```
Device Drivers --->
  Input device support --->
    -*- Generic input layer (needed for keyboard, mouse, ...) 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CONFIG_INPUT</code> to find this item. --->
      [*] Miscellaneous devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT_MISC</code> to find this item.
        <M/*> User level driver support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT_UINPUT</code> to find this item.
### Emerge

keyd is currently only available on the [GURU](https://wiki.gentoo.org/wiki/GURU) [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).

If the overlay is not enabled, run:

`root #````
eselect repository enable guru
```
`root #````
emaint sync -r guru
```
After activating the overlay, keyd can be installed:

`root #``emerge --ask app-misc/keyd`
## Usage

There is global config file at /etc/keyd/default.conf that define "layers", are packs of keys, that may be activated sequentially or at the same time with keys that activate layer.

For per application usage there is separate config at \~/.config/keyd/app.conf that define layers per applications. In this layers you define additional keys to global. Key here may be double in form of \<layer>.\<key>.

Global configuration activated with "keyd service".

When using OpenRC add the service to the default runlevel.

`root #````
rc-update add keyd default
```
`root #````
rc-service keyd start
```
keyd distributed with "keyd-application-mapper" script for usage in window environment. Permission to group "keyd" should be granted with following command for user "larry":

`root #````
usermod -aG keyd larry
```
## Configuration cases for per application usage

### Three keys like Alt+Shift+v

There are two predefined layers here *alt* and *shift*. You should define global combined layer "\[alt+shift\]", after that you may use this as the name of the layer.

**`.config/keyd/app.conf`**

```
alt+shift.v = C-end
```
### Two action at one key

If you need several keys to be pressed you may use "macro" action with several expressions separated by space.

If you need some internal action like "clear()" you should look for "clearm()" action that execute expression before "clear()" call.

### Chords

Implemented with "oneshot()" action for example "Ctrl+x key":

**`/etc/keyd/default.conf`**

### Clear other layer

Only "clearm()" working for current stable version, that breaks all layers.

### Switching between virtual consoles

We need to reset key's bindings of keyd-application-mapper script and made action of `Ctrl`+`Alt`+`Fn` keys. To switch between terminals we use chvt command from [sys-apps/busybox](https://packages.gentoo.org/packages/sys-apps/busybox).

`root #``emerge --ask busybox`
**`/etc/keyd/default.conf`**

**Global**

```
[control+alt]
f6 = command(keyd bind reset && chvt 6)
```
## Security concerns

When a user is granted access to the keyd group, and thereby to /run/keyd.socket, which allows them to send keys system-wide, it creates a breach that may be exploited by any malicious program within that user's environment. This could potentially lead to elevated root access. This vulnerability affects all uinput-based applications that allow arbitrary keys to be sent from user space.

### Wayland security

This vulnerability may be closed if we run script as root with Wayland environment variables:

`user $````
sudo --preserve-env=WAYLAND_DISPLAY,XDG_RUNTIME_DIR /usr/bin/keyd-application-mapper
```
For this command add sudo permission for current user:

Copy user configuration to root folder:

`root #````
mkdir -p /root/.config/keyd
```
`root #````
cp /home/larry/.config/keyd/app.conf /root/.config/keyd/app.conf
```
Withdraw permits by removing user from "keyd" group:

`root #````
gpasswd -d larry keyd
```
## Example of running keyd-application-mapper with DEBUG in Wayland Sway with doas

**`/etc/doas.conf`**

```
permit setenv { WAYLAND_DISPLAY XDG_RUNTIME_DIR KEYD_DEBUG } nopass larry cmd /usr/bin/keyd-application-mapper
```
**`/home/larry/.config/sway/config`**

```
exec /bin/bash -c 'KEYD_DEBUG=True doas /usr/bin/keyd-application-mapper' >> /home/larry/.config/keyd/app.log &
```
## Example of configuration of Emacs keys for Firefox

**`.config/keyd/app.conf`**

**For Firefox**

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
**`/etc/keyd/default.conf`**

**Global**

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
