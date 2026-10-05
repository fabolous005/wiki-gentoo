<!-- source: https://wiki.gentoo.org/wiki/Compose_key | group: Gentoo Wiki (Main) | wiki-title: Compose key -->
---
title: Compose key
url: https://wiki.gentoo.org/wiki/Compose_key
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-29"
fingerprint: f035d4d04b94e6f2
license: CC BY-SA 4.0
---

# Compose key

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Compose key**, also known as *Multi\_key* on the X Window System, allows one to combine several keys into one character (e.g. `compose o /` to form ø, or `compose = e` to form €).

## Configuration

Note: there is no compose key defined by default.

### Xorg

The user can specify it in `XkbOptions`:

**`/etc/X11/xorg.conf.d/keyboard.conf`**

…or through graphical front-ends such as [x11-misc/qxkb](https://packages.gentoo.org/packages/x11-misc/qxkb)

Note that not every key can be assigned as the Compose key.  To see a list of keys that **can** be used:

`user $``grep "compose:" /usr/share/X11/xkb/rules/base.lst`
**`/usr/share/X11/xkb/rules/base.lst`**

### Wayland

#### Sway

To set the compose key to **Right Control**, update your Sway config with:

**`~/.config/sway/config`**

### Key combinations

See the [Arch wiki](https://wiki.archlinux.org/title/Xorg/Keyboard_configuration#Key_combinations) for how to list and modify key combinations.
