<!-- source: https://wiki.gentoo.org/wiki/Xrandr | group: Gentoo Wiki (Main) | wiki-title: Xrandr -->
---
title: Xrandr
url: https://wiki.gentoo.org/wiki/Xrandr
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-11"
fingerprint: "6478ed47863fd8d"
license: CC BY-SA 4.0
---

# Xrandr

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **RandR** (**R**esize **and R**otate)[\[1\]](https://wiki.gentoo.org#cite_note-1)[X](https://wiki.gentoo.org/wiki/X) protocol extension and its CLI tool xrandr are used to manage screen resolutions, rotation and screens with multiply displays in X.

With version 1.4 support for multiply graphics cards was introduced, future versions (>1.5) will also support monitors attached via USB (this will probably be merged with xorg-server 1.13 with support for [DisplayLink](https://wiki.gentoo.org/wiki/DisplayLink)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>).

## Installation

The [x11-apps/xrandr](https://packages.gentoo.org/packages/x11-apps/xrandr) package can be installed with the following command:

`root #``emerge --ask x11-apps/xrandr`
### GUI tools

There are several GUI tools to use xrandr. Here is a list of tools supported in Gentoo:

- [x11-misc/arandr](https://packages.gentoo.org/packages/x11-misc/arandr) – Another XRandR GUI (an independent GUI tool)
- [lxde-base/lxrandr](https://packages.gentoo.org/packages/lxde-base/lxrandr) – [LXDE](https://wiki.gentoo.org/wiki/LXDE) GUI interface to RandR extension

## Configuration

A common way to execute [X](https://wiki.gentoo.org/wiki/Xorg) related scripts is the use of \~/.xinitrc or \~/.xprofile files in a user's home directory. Put the line of code for xrandr in one of these files and it will be executed on every X startup to make settings permanent. For example, for [i3](https://wiki.gentoo.org/wiki/I3):

**`~/.xinitrc`**

```
 &
exec dbus-run-session i3
```
## Usage

xrandr uses the monitors *Extended Display Identification Data* ([EDID](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data)) to identify its capabilities. Faulty hardware might report wrong or bad EDIDs, in those cases it is still possible to use custom made setups by trial and error.

### Screen query

Running xrandr without any arguments will list all available display output interfaces and display devices along with their current state and capabilities:

`user $``xrandr`
Screen 0: minimum 320 x 200, current 1440 x 900, maximum 8192 x 8192
VGA-1 disconnected (normal left inverted right x axis y axis)
LVDS-1 connected 1440x900+0+0 (normal left inverted right x axis y axis) 304mm x 190mm
   1440x900       60.1\*+
   1024x768       60.0
   800x600        60.3
   640x480        59.9

A xrandr query result contains all available interfaces labeled as `<interface_name>-<index>`. Each shows its connection status and the reported screen modes. The connected device's current mode is hinted by `*`, the preferred mode is hinted by `+`.

The *RandR* naming scheme for common display interfaces:

- `LVDS` - [Laptop screen](https://en.wikipedia.org/wiki/FPD-Link)
- `HDMI` - [High-Definition Multimedia Interface](https://en.wikipedia.org/wiki/HDMI)
- `DVI` - [Digital Visual Interface](https://en.wikipedia.org/wiki/Digital_Visual_Interface)
- `DP` - [DisplayPort](https://en.wikipedia.org/wiki/DisplayPort)
- `VGA` - [Video Graphics Array](https://en.wikipedia.org/wiki/Video_Graphics_Array)
- `TV` - [Composite Video](https://en.wikipedia.org/wiki/Composite_video)
- `S-video` - [S-Video](https://en.wikipedia.org/wiki/S-Video)

### Screen manipulation

It is possible to manipulate output interfaces discovered via a xrandr query. Examples of common tasks:

Set the primary display if more than one device is attached:

`user $``xrandr --output LVDS-1 --primary`
Extend to an external display attached on a known interface (*left-of*, *right-of*, *above*, *below*, *same-as*):

`user $``xrandr --output DVI-1 --auto --left-of LVDS-1`
Rotate a display (*normal*, *left*, *right*, *inverted*):

`user $``xrandr --output LVDS-1 --rotate left`
Turn off the device:

`user $``xrandr --output LVDS-1 --off`
### Screen modes

A mode always consists of a resolution and a refresh-rate. With a xrandr query the hardware reports which modes are supported. It is possible to define own modes in case the hardware reports wrong information. To change the mode for an attached device:

`user $``xrandr --output LVDS-1 --mode 1024x768`
It will use the highest refresh-rate in that mode, to also change the refresh-rate (in Hz):

`user $``xrandr --output LVDS-1 --mode 1024x768 --rate 75`
The changes apply immediately and on the hardware listed above it might either produce an error or the screen goes blank, because 75Hz is not supported. This will last until the next session, using `Ctrl`+`Alt`+`Backspace` will terminate the session and restart [X](https://wiki.gentoo.org/wiki/X) in case something goes wrong.

To add a 75Hz mode, the tool cvt ([x11-libs/libxcvt](https://packages.gentoo.org/packages/x11-libs/libxcvt)) can be used to get a proper modeline:

`user $``cvt 1024 768 75`
Everything after *Modeline* needs to be appended to the following command:

`user $``xrandr --newmode "1024x768_75.00"  109.00  1280 1368 1496 1712  1024 1027 1034 1063 -hsync +vsync`
Running the query again should list the newly added mode.

## See also

- [Non root Xorg](https://wiki.gentoo.org/wiki/Non_root_Xorg) — describes how an unprivileged user can run [Xorg](https://wiki.gentoo.org/wiki/Xorg) without using suid.
- [Xorg](https://wiki.gentoo.org/wiki/Xorg) — an open source implementation of the [X server](https://wiki.gentoo.org/wiki/X_server).
- [Xorg/Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) — explains what Xorg is, how to install it, and the various configuration options.
- [X server](https://wiki.gentoo.org/wiki/X_server) — the main component of the X Window system which abstracts the hardware and provides the foundation for most graphical user interfaces, like [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) or [window managers](https://wiki.gentoo.org/wiki/Window_manager), and their applications.
