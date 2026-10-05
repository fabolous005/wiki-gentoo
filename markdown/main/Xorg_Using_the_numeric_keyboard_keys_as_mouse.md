<!-- source: https://wiki.gentoo.org/wiki/Xorg/Using_the_numeric_keyboard_keys_as_mouse | group: Gentoo Wiki (Main) | wiki-title: Xorg/Using the numeric keyboard keys as mouse -->
---
title: Xorg/Using the numeric keyboard keys as mouse
url: https://wiki.gentoo.org/wiki/Xorg/Using_the_numeric_keyboard_keys_as_mouse
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-28"
fingerprint: d43e8c3bf999e360
license: CC BY-SA 4.0
---

# Xorg/Using the numeric keyboard keys as mouse

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

XOrg comes with built-in mouse emulation using the keyboards numeric keypad. The trick is to use `Shift`+`NumLock` to toggle the use of the numeric pad as a keyboard mouse on and off, after enabling this feature in the X configuration.

This feature comes in handy when someone funny removes the ball from the mouse, or when the touchpad controller has crashed.

## How to enable keyboard mouse keys

Mouse emulation with the number pad keys used to be a default standard feature of [Xorg/X11](https://wiki.gentoo.org/wiki/X_server). Most of the modern GNU/Linux distributions have disabled this still built-in feature. It can be manually (re)enabled either by creating or editing a configuration file in the directory [/etc/X11/xorg.conf.d/](https://wiki.gentoo.org/wiki/Xorg.conf#xorg.conf.d.2C_xorg.conf) (create it if it does not exist).

The `XkbOptions` with the value `keypad:pointerkeys`. Add or append the following line to the /etc/X11/xorg.conf.d/30-keyboard.conf file.

`Option "XkbOptions" "keypad:pointerkeys"`

Like the directory above, the file may need created if it does not already exist. For example:

**`/etc/X11/xorg.conf.d/30-keyboard.conf`**

```
Section "InputClass"
  Identifier "system-keyboard"
  MatchIsKeyboard "on"
  Option "XkbOptions" "keypad:pointerkeys"
EndSection
```
This feature can also be activated in a currently running instance of X by issuing the following command in a terminal:

`user $``setxkbmap -option keypad:pointerkeys`
## Controlling the keyboard "mouse"

Hold the `Shift` key and press `NumLock` to enable keyboard mouse emulation. A short beep may be emitted depending on computer and kernel configuration.

To move the mouse pointer:

- `7`, `8`, `9` are the up direction.
- `4`, `6` are left and right.
- `1`, `2`, `3` are the down direction.


Clicking using the emulated mouse buttons is slightly tricky, *the trick* is:

- `/` *selects* the left mouse button.
- `*` *selects* the middle mouse button.
- `-` *selects* the right mouse button.


Do note that `/`, `*` and `-` will *only select and make a mouse button active*. To actually use the active mouse button, press:

- `5` to do the actual mouse-click (press it twice for a double click; and thrice for a triple click),
- `+` to do a double mouse-click,
- `0` / `Ins` to press and hold the selected mouse button (e.g. for dragging something),
- `.` / `Del` to release the currently selected mouse button.


The numpad emulated mouse is easy to use as long as the mouse button is first selected for (ab)use by pressing `/` for left mouse button, `-` for the right button or `*` for the middle, and then pressing `5` to do the actual click (or `Ins` / `Del` for drag-and-drop).

Example: To do a mouse-click with the middle button, first press the `*` key and then `5` to click.
