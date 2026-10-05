<!-- source: https://wiki.gentoo.org/wiki/Wacom | group: Gentoo Wiki (Main) | wiki-title: Wacom -->
---
title: Wacom
url: https://wiki.gentoo.org/wiki/Wacom
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-24"
fingerprint: cda5c9563f85b98b
license: CC BY-SA 4.0
---

# Wacom

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides instructions for enabling touchscreen support for [Wacom](https://www.wacom.com/) devices such as laptops, tablets, and ultrabooks, and the like.

## Installation

### Kernel

The following option is likely necessary for proper tablet functionality, even if the device is not an Intuos/Graphire tablet as it says in the description.

**Kernel config**

Recent touchscreen models may need other options like I2C\_HID\_ACPI (for a Lenovo IdeaPad Flex 5 16ALC7 82RA):

**Kernel config**

Some tablet models may also require these options:

**Additional config**

### Userspace driver

After kernel is configured, update the `INPUT_DEVICES` Portage variable using the wacom driver:

**`/etc/portage/make.conf`**

**Set`INPUT_DEVICES`**

```
INPUT_DEVICES="wacom libinput"
```
After setting the `INPUT_DEVICES` variable remember to update the system using the following command so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`

You should also create Xorg config file so that Xorg knows which driver should be used for the tablet. Example:

**`/etc/X11/xorg.conf.d/42-libinput.conf`**

```
Section "InputClass"
        Identifier "Tablet"
        Driver "wacom"
        MatchIsTablet "on"
EndSection
```
#### Xinput2 multitouch

The default setting of the `wacom` xinput driver has "gesture emulation".

When *disabled*, **true multitouch events** will be emitted instead and it can be used with [Firefox multitouch](https://wiki.gentoo.org/wiki/Firefox#Enabling_multitouch).

```
Section "InputClass"
	Identifier "Wacom class"
	MatchProduct "Wacom|WACOM|Hanwang|PTK-540WL|ISDv4|ISD-V4|ISDV4"
	MatchDevicePath "/dev/input/event*"
	Driver "wacom"
	Option "Gesture" "off"
EndSection
```
## Configuration

To list detected devices in the terminal, try `xsetwacom`. This command also allows tweaking things such as stylus buttons and draw space, but keep in mind that more work is necessary to make the changes persist after exiting an X session (see [this discussion](https://askubuntu.com/questions/9242/how-do-i-change-xsetwacom-and-make-the-settings-stay-on-startup)).

`user $``xsetwacom --list devices`
Wacom One by Wacom S Pen stylus         id: 15  type: STYLUS    
Wacom One by Wacom S Pen eraser         id: 16  type: ERASER
