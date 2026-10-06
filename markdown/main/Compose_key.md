<!-- source: https://wiki.gentoo.org/wiki/Compose_key | group: Gentoo Wiki (Main) | wiki-title: Compose key -->
---
title: Compose key
url: https://wiki.gentoo.org/wiki/Compose_key
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-29"
fingerprint: "30b594d5689c66f6"
license: CC BY-SA 4.0
---

# Compose key

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The **Compose key**, also known as *Multi\_key* on the X Window System, allows one to combine several keys into one character (e.g. `compose o /` to form ø, or `compose = e` to form €).

## Configuration

Note: there is no compose key defined by default.

### Xorg

The user can specify it in `XkbOptions`:

FILE **`/etc/X11/xorg.conf.d/keyboard.conf`**

```
Section "InputClass"
        Identifier          "Keyboard0"
        MatchIsKeyboard     "on"
        Option "XkbLayout"  "us,ru"
        Option "XkbOptions" "grp:alt_shift_toggle,compose:rctrl"
EndSection
```
…or through graphical front-ends such as [x11-misc/qxkb](https://packages.gentoo.org/packages/x11-misc/qxkb)

Note that not every key can be assigned as the Compose key.  To see a list of keys that **can** be used:

`user $``grep "compose:" /usr/share/X11/xkb/rules/base.lst`
FILE **`/usr/share/X11/xkb/rules/base.lst`**

```
  compose:ralt         Right Alt
  compose:lwin         Left Win
  compose:lwin-altgr   3rd level of Left Win
  compose:rwin         Right Win
  compose:rwin-altgr   3rd level of Right Win
  compose:menu         Menu
  compose:menu-altgr   3rd level of Menu
  compose:lctrl        Left Ctrl
  compose:lctrl-altgr  3rd level of Left Ctrl
  compose:rctrl        Right Ctrl
  compose:rctrl-altgr  3rd level of Right Ctrl
  compose:caps         Caps Lock
  compose:caps-altgr   3rd level of Caps Lock
  compose:102          The "< >" key
  compose:102-altgr    3rd level of the "< >" key
  compose:paus         Pause
  compose:ins          Insert
  compose:prsc         PrtSc
  compose:sclk         Scroll Lock
```
### Wayland

#### Sway

To set the compose key to **Right Control**, update your Sway config with:

FILE **`~/.config/sway/config`**

```
...
input * xkb_options compose:rctrl
...
```
### Key combinations

See the [Arch wiki](https://wiki.archlinux.org/title/Xorg/Keyboard_configuration#Key_combinations) for how to list and modify key combinations.
