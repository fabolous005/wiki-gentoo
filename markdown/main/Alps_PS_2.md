<!-- source: https://wiki.gentoo.org/wiki/Alps_PS/2 | group: Gentoo Wiki (Main) | wiki-title: Alps PS/2 -->
---
title: Alps PS/2
url: https://wiki.gentoo.org/wiki/Alps_PS/2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-08-11"
fingerprint: bfd85b5a5f361126
license: CC BY-SA 4.0
---

# Alps PS/2

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

To get the [Alps touchpad](https://www.notebookcheck.net/fileadmin/_processed_/csm_DSC04158_43a71e3d78.jpg) working, the kernel option **ALPS PS/2 mouse protocol extension** ( `CONFIG_MOUSE_PS2_ALPS` ) needs to be activated.

KERNEL **linux-4.19**

```
Device Drivers  --->
    Input device support  --->
    -*- Generic input layer (needed for keyboard, mouse, ...)
    [*]   Mice  ---> 
        --- Mice
        <*>   PS/2 mouse
        [*]     ALPS PS/2 mouse protocol extension
        [ ]     BYD PS/2 mouse protocol extension
```
It can be operated using the [libinput](https://wiki.gentoo.org/wiki/Libinput) driver and the default configuration as set in /usr/share/X11/xorg.conf.d/40-libinput.conf should work sufficiently in most cases.  Customization can be done via /etc/X11/xorg.conf.d/30-touchpad.conf.

FILE **`/etc/X11/xorg.conf.d/30-touchpad.conf`**

```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
        Option "Tapping" "on"
        Option "Accel Speed" "1.0"
EndSection
```
For more detailed instruction/description, see [https://wiki.archlinux.org/index.php/Libinput#Common\_options](https://wiki.archlinux.org/index.php/Libinput#Common_options)

In dmesg it will appear like:

`user $``dmesg`
\[    2.282316\] input: AlpsPS/2 ALPS DualPoint Stick as /devices/platform/i8042/serio1/input/input14
\[    2.296204\] input: AlpsPS/2 ALPS DualPoint TouchPad as /devices/platform/i8042/serio1/input/input8

lsinput ([sys-apps/input-utils](https://packages.gentoo.org/packages/sys-apps/input-utils)) detects it as

`root #``lsinput`
12: 0002:0008 I8042  isa0060/serio1/i AlpsPS/2 ALPS DualPoint  KEY REL ABS     
13: 0002:0008 I8042  isa0060/serio1/i AlpsPS/2 ALPS DualPoint  KEY ABS

xinput and libinput list-devices can be used to show the touchpad's characteristics:

`root #``libinput list-devices | sed -n '/DualPoint/,/^$/p'`
Device:           AlpsPS/2 ALPS DualPoint Stick
Kernel:           /dev/input/event12
Group:            6
Seat:             seat0, default
Capabilities:     pointer 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      disabled
Nat.scrolling:    disabled
Middle emulation: disabled
Calibration:      n/a
Scroll methods:   \*button
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   flat \*adaptive
Rotation:         n/a
Device:           AlpsPS/2 ALPS DualPoint TouchPad
Kernel:           /dev/input/event13
Group:            6
Seat:             seat0, default
Size:             80x44mm
Capabilities:     pointer 
Tap-to-click:     disabled
Tap-and-drag:     enabled
Tap drag lock:    disabled
Left-handed:      disabled
Nat.scrolling:    disabled
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   \*two-finger edge 
Click methods:    none
Disable-w-typing: enabled
Accel profiles:   none
Rotation:         n/a

`user $``xinput`
⎡ Virtual core pointer                          id=2    \[master pointer  (3)\]
⎜   ↳ Virtual core XTEST pointer                id=4    \[slave  pointer  (2)\]
⎜   ↳ AlpsPS/2 ALPS DualPoint Stick             id=12   \[slave  pointer  (2)\]
⎜   ↳ AlpsPS/2 ALPS DualPoint TouchPad          id=13   \[slave  pointer  (2)\]

`user $``xinput list --long 13````
AlpsPS/2 ALPS DualPoint TouchPad                id=13   [slave  pointer  (2)]
        Reporting 7 classes:
                Class originated from: 13. Type: XIButtonClass
                Buttons supported: 7
                Button labels: "Button Left" "Button Middle" "Button Right" "Button Wheel Up" "Button Wheel Down" "Button Horiz Wheel Left" "Button Horiz Wheel Right"
                Button state:
                Class originated from: 13. Type: XIValuatorClass
                Detail for Valuator 0:
                  Label: Rel X
                  Range: -1.000000 - -1.000000
                  Resolution: 0 units/m
                  Mode: relative
                Class originated from: 13. Type: XIValuatorClass
                Detail for Valuator 1:
                  Label: Rel Y
                  Range: -1.000000 - -1.000000
                  Resolution: 0 units/m
                  Mode: relative
                Class originated from: 13. Type: XIValuatorClass
                Detail for Valuator 2:
                  Label: Rel Horiz Scroll
                  Range: -1.000000 - -1.000000
                  Resolution: 0 units/m
                  Mode: relative
                Class originated from: 13. Type: XIValuatorClass
                Detail for Valuator 3:
                  Label: Rel Vert Scroll
                  Range: -1.000000 - -1.000000
                  Resolution: 0 units/m
                  Mode: relative
                Class originated from: 13. Type: XIScrollClass
                Scroll info for Valuator 2
                  type: 2 (horizontal)
                  increment: 15.000000
                  flags: 0x0
                Class originated from: 13. Type: XIScrollClass
                Scroll info for Valuator 3
                  type: 1 (vertical)
                  increment: 15.000000
                  flags: 0x0
```
`user $``xinput list-props 13````
Device 'AlpsPS/2 ALPS DualPoint TouchPad':
        Device Enabled (125):   1
        Coordinate Transformation Matrix (127): 1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
        libinput Tapping Enabled (281): 0
        libinput Tapping Enabled Default (282): 0
        libinput Tapping Drag Enabled (283):    1
        libinput Tapping Drag Enabled Default (284):    1
        libinput Tapping Drag Lock Enabled (285):       0
        libinput Tapping Drag Lock Enabled Default (286):       0
        libinput Tapping Button Mapping Enabled (287):  1, 0
        libinput Tapping Button Mapping Default (288):  1, 0
        libinput Accel Speed (263):     0.000000
        libinput Accel Speed Default (264):     0.000000
        libinput Natural Scrolling Enabled (268):       0
        libinput Natural Scrolling Enabled Default (269):       0
        libinput Send Events Modes Available (248):     1, 1
        libinput Send Events Mode Enabled (249):        0, 0
        libinput Send Events Mode Enabled Default (250):        0, 0
        libinput Left Handed Enabled (270):     0
        libinput Left Handed Enabled Default (271):     0
        libinput Scroll Methods Available (272):        1, 1, 0
        libinput Scroll Method Enabled (273):   1, 0, 0
        libinput Scroll Method Enabled Default (274):   1, 0, 0
        libinput Disable While Typing Enabled (289):    1
        libinput Disable While Typing Enabled Default (290):    1
        Device Node (251):      "/dev/input/event13"
        Device Product ID (252):        2, 8
        libinput Drag Lock Buttons (279):       <no items>
        libinput Horizontal Scroll Enabled (280):       1
```
## Troubleshooting

- Due to [kernel bug 204541](https://bugzilla.kernel.org/show_bug.cgi?id=204541), some kernel versions from gentoo-sources:4.19.63 also needed **IBM Trackpoint PS/2 mouse protocol extension** ( `CONFIG_MOUSE_PS2_TRACKPOINT` ).  This is fixed since gentoo-sources:4.19.68
