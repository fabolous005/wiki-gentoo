<!-- source: https://wiki.gentoo.org/wiki/SpaceNavigator | group: Gentoo Wiki (Main) | wiki-title: SpaceNavigator -->
---
title: SpaceNavigator
url: https://wiki.gentoo.org/wiki/SpaceNavigator
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-12-23"
fingerprint: e023105e5dcff9af
license: CC BY-SA 4.0
---

# SpaceNavigator

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![](https://wiki.gentoo.org/images/thumb/0/01/3dconnexion_space_navigator_1024x1024.jpg/300px-3dconnexion_space_navigator_1024x1024.jpg)

This article is written for the SpaceNavigator but it should work with other 3D mouse in the Space-series produced by 3Dconnexion. Only applications that support 3D mouse device can work properly with this driver. For now, only [blender](https://wiki.gentoo.org/wiki/Blender) support it in the portage tree.

## Kernel configuration

To be able to use spacenavd used in this article you need to compile your kernel with input evdev support as well as with support for Logitech HID devices and Physical Interface Device support.

## Pitfalls

The following troubles are rather easy to run into (and to get out of).

### No response

The device cannot be managed by *both* Xorg and the spacenavd daemon
at the same time, see [modes of operation](https://wiki.gentoo.org#Modes_of_operation).
If you get no reaction with Xorg verify that spacenavd is *not* running.
Respectively, if the device doesn't work in Blender make sure the daemon
*is* running and xinput is *not* listing the device.

### Orientation / inverted axes

The place where the cable leaves the body marks the top orientation of the device as with a normal 2D mouse. If you position the device in a different position inverting certain axes may be necessary to obtain expected behavior.

## Modes of operation

SpaceNavigator can be operated in two mutually exclusive modes:

## Xorg xinput device

### xf86-input-evdev

You can use your SpaceNavigator as a mouse without any additional drivers except for [Kernel Event Support](https://wiki.gentoo.org#Kernel_configuration) and xf86-input-evdev with

**`/etc/X11/xorg.conf.d/20-spacenavigator.conf`**

```
Section "InputClass"
        Identifier "evdev 3Dconnexion"
        MatchProduct "3Dconnexion|SpaceNavigator|Space Navigator"
        Driver "evdev"
        Option "Mode" "Relative"
        Option "ConstantDeceleration" "20"  # decrease initial velocity to usable value
EndSection
```
Once you restart Xorg and unplug-replug the device you can list supported properties and current values:

`root #``emerge --ask x11-apps/xinput``user $``xinput --list-props '3Dconnexion Space Navigator'````
Device '3Dconnexion Space Navigator':
        Device Enabled (132):
        Coordinate Transformation Matrix (134):        1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
        Device Accel Profile (256):        0
        Device Accel Constant Deceleration (257):        10.000000
        Device Accel Adaptive Deceleration (258):        1.000000
        Device Accel Velocity Scaling (259):        10.000000
        Evdev Axis Inversion (260):        1, 1
        Evdev Axes Swap (262):        0
        Axis Labels (263):        "Rel X" (142), "Rel Y" (143), "Rel Z" (275), "Rel Rotary X" (276), "Rel Rotary Y" (277), "Rel Rotary Z" (278)
        Button Labels (264):        "Button 0" (273), "Button 1" (274), "Button Unknown" (250), "Button Wheel Up" (138), "Button Wheel Down" (139)
        Evdev Middle Button Emulation (265):        0
        Evdev Middle Button Timeout (266):        50
        Evdev Wheel Emulation (267):        0
        Evdev Wheel Emulation Axes (268):        0, 0, 4, 5
        Evdev Wheel Emulation Inertia (269):        10
        Evdev Wheel Emulation Timeout (270):        200
        Evdev Wheel Emulation Button (271):        4
        Evdev Drag Lock Buttons (272):        0
```
These keys can be used to alter the current configuration, for example:

`user $````
xinput --set-prop '3Dconnexion Space Navigator' 'Device Accel Constant Deceleration' 10
```
`user $````
xinput --set-prop '3Dconnexion Space Navigator' 'Evdev Axis Inversion' 1 1
```
### xf86-input-joystick

The evdev approach above provides an absolute pointer that always re-centers on the screen. For a relative pointer that works more like a pointing stick, use xf86-input-joystick instead.

**`/etc/X11/xorg.conf.d/20-spacenavigator.conf`**

```
Section "InputClass"
        Identifier "SpaceNavigator"
        MatchProduct "3Dconnexion|SpaceNavigator|Space Navigator"
	Driver "joystick"
	# X: move left/right - horizontal scroll
	Option "MapAxis1"   "mode=relative axis=+5zx deadzone=15000"
	# Y: move up/down - vertical scroll
	Option "MapAxis2"   "mode=relative axis=+5zy deadzone=15000"
	# Z: pull up / push down - right / left click
	Option "MapAxis3"   "mode=relative keyhigh=251 keylow=252 deadzone=2000"
	# RY: tilt up/down - move pointer
	Option "MapAxis4"   "mode=relative axis=+5y deadzone=500"
	# RX: tilt left/right (inverted) - move pointer
	Option "MapAxis5"   "mode=relative axis=-5x deadzone=500"
	# RZ: twist left/right - horizontal scroll
	Option "MapAxis6"   "mode=relative axis=-10zy deadzone=5000"
	# Button 2 - right click
	Option "MapButton2" "button=3"
EndSection
```
The mouse buttons on the Z axis are mapped to unused keycodes which can be configured through Xkb as mouse keys:

```
    key <I251> {         [     XF86User1KB ] };
    key <I252> {         [     XF86User2KB ] };
    interpret XF86User1KB+AnyOfOrNone(all) {
        action= PtrBtn(button=1);
    };
    interpret XF86User2KB+AnyOfOrNone(all) {
        action= PtrBtn(button=3);
    };
```
Mouse keys then can be enabled using xkbset:

`user $````
xkbset mousekeys
```
`user $````
xkbset exp \=mousekeys
```
With the above configuration it is quite usable as a mouse, only thing missing is middle click, not sure where to map that.

#### Firefox

- Ctrl + left click opens links in a new tab as an alternative to middle click.
- Shift + scroll navigates the history
- Ctrl + scroll zooms the page

## Supported applications

### Blender

To enable SpaceNavigator support for [Blender](https://wiki.gentoo.org/wiki/Blender) you need to enable use flag "ndof"

**`/etc/portage/package.use`**

or

**`/etc/portage/package.use/media-gfx-blender`**

To get 3D mouse working properly you will need spacenavd.

`root #``emerge --ask app-misc/spacenavd`
After installing spacenavd you could check out the following file before starting anything, normally default values works for everyone.

**`/etc/spnavrc`**

When finished you can start the spacenavd with

`root #``/etc/init.d/spacenavd start`
or with systemd

`root #``systemctl start spacenavd`
And add it to start up as default with

`root #``/sbin/rc-update add spacenavd default`
or with systemd

`root #``systemctl enable spacenavd`
You could also make [udev](https://wiki.gentoo.org/wiki/Udev) start the daemon for you when a SpaceNavigator is plugged in:

`root #``ln -s 99-space-navigator.rules.ignored /etc/udev/rules.d/99-space-navigator.rules`
spacenavd runs as the root user and must connect to the X server. Authentication is done via the MIT-MAGIC-COOKIE mechanism, consequently spacenavd must know about the cookie stored in the *user's* \~/.Xauthority file. One way to do this is via the xauth command:

`root #``xauth -f /home/ta/.Xauthority extract - :0 | xauth -f /root/.Xauthority merge -`
Also see the # 10 in the [spacenavd FAQ](http://spacenav.sourceforge.net/faq.html#faq10) :

If everything worked correctly you should now have a working device in blender (it must be emerge with **ndof** useflag, or **3dmouse** for older version).

### Alternative x11 events

By default, the spacenavd daemon use the uevent mechanism but can alternatively use x11 socket to handle events. Most of the time, uevents should be enough to use the 3D mouse in supported applications but Xevents has one advantage, it's capable of handle proprietary driver protocol Magellan. But if your application doesn't run on top of X proprietary driver is useless. More information on *What's wrong with the proprietary driver ?* or *What is provided by the spacenav project?*, read the official FAQ (see links following).

If you want to use X events support, you must enable it with the X use flag on build time and the spacenavd daemon must be notified of this new protocol with

`root #` `spnavd_ctl x11 start` To start generating Spacenav X events by default you should add this command in your user startup scripts such as \~/.gnomerc or \~/.xinitrc .

### Extra

Optionally you can add the following ebuild to configure dynamically the spacenavd.

`root #``emerge --ask x11-misc/spnavcfg``root #``/etc/init.d/spacenavd start``user $``spnavcfg`
![](https://wiki.gentoo.org/images/b/bc/Spnavcfg.png)

The changes are immediately transmit to the spacenavd daemon that modify its behavior as requested.

## External resources

- [Website of spacenav tools](http://spacenav.sourceforge.net/) at SourceForge (libspnav, spacenavd, spnavcfg and other)
- [Official spacenav FAQ](http://spacenav.sourceforge.net/faq.html)
