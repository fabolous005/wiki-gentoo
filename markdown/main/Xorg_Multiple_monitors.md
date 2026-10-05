<!-- source: https://wiki.gentoo.org/wiki/Xorg/Multiple_monitors | group: Gentoo Wiki (Main) | wiki-title: Xorg/Multiple monitors -->
---
title: Xorg/Multiple monitors
url: https://wiki.gentoo.org/wiki/Xorg/Multiple_monitors
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-10-31"
fingerprint: "36d5087cb23dcbdd"
license: CC BY-SA 4.0
---

# Xorg/Multiple monitors

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describes the setup of multiple monitors under Xorg. Multihead support in [X](https://wiki.gentoo.org/wiki/X) has luckily progressed in recent
versions, so it is pretty easy to set up. Usually it is as simple as defining monitors in Xorg configuration files.

## Installation

### USE flags

Be aware, most desktop environments, window managers, and graphical toolkit require the [xinerama](https://packages.gentoo.org/useflags/xinerama)

**`/etc/portage/make.conf`**

```
USE="xinerama"
```
### Emerge

Next rebuild the [@world set](https://wiki.gentoo.org/wiki/Package_sets#System_defining_sets).

`root #``emerge --ask --update --deep --newuse @world`
## Configuration

### Simple

The basic procedure is defining one "Monitor" section per Monitor and then tying everything together in a "Device" section that specifies the video chip driving the monitor(s). For the following examples, Xorg's configuration is presumed to be stored in /etc/X11/xorg.conf.d (no /etc/X11/xorg.conf file will be used). Alternatively, if the user desires, all files defined in the xorg.conf.d folder can be combined together to form an xorg.conf. The following example is the basic version for two monitors of the same resolution.

**`/etc/X11/xorg.conf.d/41-dvi-monitor.conf`**

**Right monitor**

```
Section "Monitor"
    Identifier  "DVI"
EndSection
```
**`/etc/X11/xorg.conf.d/42-hdmi-monitor.conf`**

**Left monitor**

```
Section "Monitor"
    Identifier  "DP"
    Option      "LeftOf"        "DVI"
EndSection
```
Each "Monitor" section must contain an Identifier, everything else is optional. The identifier may be freely chosen. The placement of the monitors is done relatively with one of the keywords `RightOf`, `LeftOf`, `Above`, or `Below`. Additionally there is a `Relative` keyword for fancy cases, however this article avoids using it. Each monitor can receive custom options (e.g. resolution) in its configuration.

Next the monitors need to be mapped to the outputs. This is done in the `Device` section describing the video chip and driver. The Device section of the two monitor example (using an [Intel](https://wiki.gentoo.org/wiki/Intel) video card) would look like the following.

**`/etc/X11/xorg.conf.d/50-video.conf`**

**Device section**

```
Section "Device"
    Identifier  "Device0"
    Option      "Monitor-DVI2" "DVI"
    Option      "Monitor-DP1" "DP"
    Driver      "intel"
EndSection
```
The names of the monitors are in the form "Monitor-\<port>". The port each monitor was on was determined by running the `xrandr -q` command. This should be everything needed for a basic setup.

### More complex cases

Sometimes more complex layouts will be needed. A third example of a monitor file, with resolution options could look like the following.

**`/etc/X11/xorg.conf.d/43-lvds-monitor.conf`**

**Advanced configuration with resolution defined**

```
Section "Monitor"
    Modeline    "1280x800x60" 71.00 1280 1328 1360 1440 800 803 809 823
    Option      "PreferredMode" "1280x800"
    Identifier  "LVDS"
    Option      "RightOf"       "DVI"
    VendorName  "CMO"
EndSection
```
It is possible to see in the example above that manufacturer names can be used to get a single config file attached to a single device.

Next a more complete example of the video configuration for [x11-drivers/xf86-video-intel](https://packages.gentoo.org/packages/x11-drivers/xf86-video-intel) package will be provided:

**`/etc/X11/xorg.conf.d/50-video.conf`**

**Advanced intel example**

```
Section "Device"
    Option      "Monitor-DVI2" "DVI"
    Option      "Monitor-DP1" "DP"
    Option      "Monitor-LVDS2" "LVDS"
    Option      "AccelMethod" "sna"
    Option      "TearFree" "true"
    Option      "DRI" "True"
    Option      "XvMCSurfaces" "6"
    Identifier  "GMA-X4500"
    Driver      "intel"
EndSection
```
The following was made for the [x11-drivers/xf86-video-ati](https://packages.gentoo.org/packages/x11-drivers/xf86-video-ati) radeon driver. Notice the change of the video output name on the same laptop.

**`/etc/X11/xorg.conf.d/50-video.conf`**

**Radeon driver example**

```
Section "Device"
    Option      "Monitor-DVI-0" "DVI"
    Option      "Monitor-HDMI-0" "DP"
    Option      "Monitor-LVDS" "LVDS"
    Option      "AccelMethod" "EXA"
    Option      "ClockGating" "On"
    Option      "ForceLowPowerMode" "On"
    Option      "VGAAccess" "On"
    Identifier  "HD3450"
    Driver      "radeon"
    Option      "MonitorLayout" "LVDS, AUTO"
EndSection
```
### Different resolutions

[Xrandr](https://wiki.gentoo.org/wiki/Xrandr)-1.3 is a little deficient in this regard. X can precisely probe the screen devices, but xrandr will get/display the minimum resolution fitted to the smallest screen attached to the machine. This is annoying because it creates a debug situation. Xorg.0.log has to be read in order to get possible monitor resolutions, define a few modes with xrandr, and then choose a video mode with xrandr. There is nothing user friendly about the process. The process usually takes matter of minutes, but is annoying when a user desires to attach a bigger screen to a computer for a simple event like watching a movie. The latter can be achieved, somewhat, with the following example.

**`/etc/X11/xorg.conf.d/44-vga-monitor.conf`**

**Preferred video modes example**

```
Section "Monitor"
    Option      "SyncOnGreen" "True"
    Modeline    "1280x1024x85" 157.50 1280 1344 1504 1728 1024 1025 1028 1072
    Modeline    "1600x1200x85" 229.50 1600 1664 1856 2160 1200 1201 1204 1250
    Modeline    "1600x1200x75" 202.50 1600 1664 1856 2160 1200 1201 1204 1250
    Option      "PreferredMode" "1600x1200"
    Identifier  "VGA"
    VendorName  "GSM"
EndSection
```
## Troubleshooting

**Bug:**
Anyone can notice the level of fine probing X server does (or rather [x11-drivers/xf86-input-evdev](https://packages.gentoo.org/packages/x11-drivers/xf86-input-evdev)). Numerous handy details are provided (numerous screen resolutions, gamma values, and other obscure values). One can pick any defined video mode on the list and X server display it. Now, whenever anyone desires to change the video mode of a particular display xrandr will complain about non-existant modes. What? The video modes vanished somewhere? The only that remain are the ones that can fit to the biggest resolution on the smallest display. To get more video modes copy/paste some preferred video modes like in the example above.
