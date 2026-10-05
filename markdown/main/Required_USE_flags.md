<!-- source: https://wiki.gentoo.org/wiki/Required_USE_flags | group: Gentoo Wiki (Main) | wiki-title: Required USE flags -->
---
title: Required USE flags
url: https://wiki.gentoo.org/wiki/Required_USE_flags
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-30"
fingerprint: e73789f9a0bbd501
license: CC BY-SA 4.0
---

# Required USE flags

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Certain packages require being built with specific [USE flags](https://wiki.gentoo.org/wiki/USE_flag), these are called **required USE flags**. When trying to [emerge](https://wiki.gentoo.org/wiki/Emerge) such a package, [Portage](https://wiki.gentoo.org/wiki/Portage) will sometimes halt, and output a message stating that a dependency will require specific USE flags to be set or unset.

These USE flag requirements must be met for Portage to be able to proceed with installation, and it is up to the user to set up USE flags in a way that satisfies the requirements. USE flag requirements cannot be set automatically, as they imply a choice presented to the user.

For some packages, USE flag requirements vary according to what other USE flags are active or not. An example could be a package that has a choice of audio backends, where only one may be active at once.

## Portage output

When USE flags are required, Portage outputs a message, such as:

The following REQUIRED\_USE flag constraints are unsatisfied:
  foo? ( bar )
The above constraints are a subset of the following complete expression:
  foo? ( bar ) baz? ( ... ) ...

There are two parts to the constraint message:

1. The *unsatisfied constraints* - this is the part to pay attention to, as it shows what changes are required to proceed.
2. The *full USE constraints* for the package - this is just to give the surrounding context for the above, and should not be used to pick out USE flag changes. If it's not listed in the first section, then the constraints are already satisfied.

In the previous example, the active USE flag that imposes a constraint ("foo") comes first and is followed by an operator ("?"), then by the required flag ("bar"). Requirements can be chained, for example if flag X requires flag Y, that in turn requires flag Z.

As well as requiring the activation of certain flags, USE flag constraints can sometimes require the *deactivation* of a flag or flags. In this case, the flag that must be deactivated to satisfy the constraint is preceded by an exclamation mark ("!").

## Required USE operators

Operators indicate what is specifically needed to satisfy the requirement:

| Name | Operator | Example | Meaning | 
|---|---|---|---|
| if | ? | wayland? ( gles2 ) | Wayland support requires GLES2. Either turn on gles2 or disable wayland for this package. | 
| at-most-one-of | ?? | ?? ( libedit readline ) | libedit support is mutually exclusive with readline support. Either enable neither or just one. Not both. | 
| any-of | \|\| | \|\| ( ncurses wayland X ) | Enable at least one of ncurses, wayland, or X for this package. 1 is okay, more than 1 is okay, 0 is not okay. | 
| exactly-one-of | ^^ | ^^ ( elogind systemd ) | The package needs either elogind or systemd. Ensure exactly one is enabled (not two, not zero!) | 
| negation/disable/off | ! | !static-libs | The package or another USE flag needs the static-libs flag to be disabled/turned off. | 

## Examples

### Example 1

Here's a typical example of a required USE flag message:

`root #``emerge -p -uvDU @world````
[...]
!!! The ebuild selected to satisfy "media-libs/libsdl2" has unmet requirements.
- media-libs/libsdl2-2.0.16-r1::gentoo USE="X alsa dbus joystick opengl pulseaudio sound threads udev video wayland (-aqua) (-custom-cflags) -doc -fcitx4 -gles1 -gles2 -haptic -ibus -jack -kms -libsamplerate -nas -oss -pipewire -sndio -static-libs -vulkan -xinerama -xscreensaver" ABI_X86="(64) -32 (-x32)" CPU_FLAGS_X86="mmx sse sse2 -3dnow" VIDEO_CARDS="(-vc4)"
  The following REQUIRED_USE flag constraints are unsatisfied:
    wayland? ( gles2 )
  The above constraints are a subset of the following complete expression:
    alsa? ( sound ) fcitx4? ( dbus ) gles1? ( video ) gles2? ( video ) haptic? ( joystick ) ibus? ( dbus ) jack? ( sound ) nas? ( sound ) opengl? ( video ) pulseaudio? ( sound ) sndio? ( sound ) vulkan? ( video ) wayland? ( gles2 ) xinerama? ( X ) xscreensaver? ( X )
(dependency required by "dev-libs/efl-1.25.1-r13::gentoo[sdl]" [installed])
(dependency required by "x11-wm/enlightenment-0.24.2-r2::gentoo[wayland]" [installed])
(dependency required by "@selected" [set])
(dependency required by "@world" [argument])
[...]
```
The key part is:

The following REQUIRED\_USE flag constraints are unsatisfied:
   wayland? ( gles2 )

This means: if the [wayland](https://packages.gentoo.org/useflags/wayland) [flag is enabled for](https://wiki.gentoo.org/wiki/USE_flag) [media-libs/libsdl2](https://packages.gentoo.org/packages/media-libs/libsdl2), the [gles2](https://packages.gentoo.org/useflags/gles2) [flag is required to also be enabled.](https://wiki.gentoo.org/wiki/USE_flag)

This leaves the user with two options, of which they must choose one:

- Enable [gles2](https://packages.gentoo.org/useflags/gles2)[media-libs/libsdl2](https://packages.gentoo.org/packages/media-libs/libsdl2)
- Disable [wayland](https://packages.gentoo.org/useflags/wayland)[media-libs/libsdl2](https://packages.gentoo.org/packages/media-libs/libsdl2)

### Example 2

Here's a slightly more complex example:

`root #``emerge -p -uvDU @world`
\[...\]
!!! The ebuild selected to satisfy "sys-fs/lvm2\[static-libs(+)\]" has unmet requirements.
- sys-fs/lvm2-2.03.14-r1::gentoo USE="device-mapper-only lvm2create-initrd readline sanlock static-libs systemd thin udev (-selinux) -static" ABI\_X86="(64)"
The following REQUIRED\_USE flag constraints are unsatisfied:
device-mapper-only? ( !lvm2create-initrd !sanlock !thin ) static-libs? ( static !udev )
The above constraints are a subset of the following complete expression:
device-mapper-only? ( !lvm2create-initrd !sanlock !thin ) static? ( !systemd !udev ) static-libs? ( static !udev ) systemd? ( udev )

The key part is:

The following REQUIRED\_USE flag constraints are unsatisfied:
   device-mapper-only? ( !lvm2create-initrd !sanlock !thin ) static-libs? ( static !udev )

This example contains two parts:

1. If the [device-mapper-only](https://packages.gentoo.org/useflags/device-mapper-only)[sys-fs/lvm2](https://packages.gentoo.org/packages/sys-fs/lvm2), the three flags [lvm2create-initrd](https://packages.gentoo.org/useflags/lvm2create-initrd)[sanlock](https://packages.gentoo.org/useflags/sanlock)[thin](https://packages.gentoo.org/useflags/thin)2. And, if the [static-libs](https://packages.gentoo.org/useflags/static-libs)[static](https://packages.gentoo.org/useflags/static)[udev](https://packages.gentoo.org/useflags/udev)

The user must choose a combination of flags that meet these requirements in order to proceed.

## External resources

## See also

[/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) — provides a more fine grained **[per-package control](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/USE#Declaring_USE_flags_for_individual_packages) of [USE flags](https://wiki.gentoo.org/wiki/USE_flag)** than the `USE` variable in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE)
