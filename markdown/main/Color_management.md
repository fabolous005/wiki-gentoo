<!-- source: https://wiki.gentoo.org/wiki/Color_management | group: Gentoo Wiki (Main) | wiki-title: Color management -->
---
title: Color management
url: https://wiki.gentoo.org/wiki/Color_management
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-26"
fingerprint: c6211f3f2f77a3e4
license: CC BY-SA 4.0
---

# Color management

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Color management at its simplest definition is a computer technique for ensuring colors stay the same or at least as close as possible between devices. Colors may differ due to different physical characteristics between devices and even imperfections between displays of the same model and ageing of a display.

## Basic theory behind computer color

Despite the title of the section basic grasp of [RGB](https://en.wikipedia.org/wiki/RGB_color_model) is assumed until someone writes about the basics here.

### Shortcomings of RGB

More fundamentally RGB is a relative scale that most commonly has a mere 256 states per channel which is largely insufficient to represent the vividness of the real world. For better or worse our eyes see the world in a logarithmic fashion with greater distinction between darker tones. This can be exploited to avoid encoding many bright tones that seem roughly the same to humans. Nevertheless color management and using the correct gamma function are not the same thing although both are required for correct end results.

Another issue is the aforementioned relativity where the color space is defined by the display characteristics, meaning that #FF0000, #00FF00, #0000FF and by extension #FFFFFF are in fact different tones from device to device.

### sRGB

[sRGB](https://en.wikipedia.org/wiki/SRGB) is Microsoft's attempt at fixing shortcomings of RGB, chiefly lack of absolute reference points and clarifies which gamma curve to use. On the down side, that curve itself is non-linear, adding complexity to proper sRGB support. Despite this sRGB is the de facto standard for RGB24/32 images and if it lacks a color profile then sRGB is the safest bet for files produced in the past 15 or so years.

## Color management systems (CMS)

On Linux the two main solutions for display color management are Oyranos ([media-libs/oyranos](https://packages.gentoo.org/packages/media-libs/oyranos)) and colord ([x11-misc/colord](https://packages.gentoo.org/packages/x11-misc/colord)).

### KDE (Plasma 5)

Enable the `colord` USE flag and rebuild the [world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>). Emerge [kde-misc/colord-kde](https://packages.gentoo.org/packages/kde-misc/colord-kde) to provide interfaces and session daemon to colord. [media-gfx/displaycal-py3](https://packages.gentoo.org/packages/media-gfx/displaycal-py3) can be used to calibrate the devices instead of [GNOME](https://wiki.gentoo.org/wiki/Gnome) Color Management as noted in [KDE](https://wiki.gentoo.org/wiki/KDE) Plasma application System Settings -> Hardware -> Color Corrections.

### GNOME

By default, GNOME uses [gnome-base/gnome-settings-daemon](https://packages.gentoo.org/packages/gnome-base/gnome-settings-daemon) to communicate with colord. Enable the `colord` USE flag and rebuild the [world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>). For at least non-lite version of GNOME that should be all that is required.

### Others

Desktop environments like [LXDE](https://wiki.gentoo.org/wiki/LXDE), [Xfce](https://wiki.gentoo.org/wiki/Xfce), or [i3](https://wiki.gentoo.org/wiki/I3) do not feature native means of communication with colord. However, it is possible to use xiccd ([x11-misc/xiccd](https://packages.gentoo.org/packages/x11-misc/xiccd)) which acts as an independent alternative to GNOME/KDE daemons. The display profiles can be associated with devices using colord's colormgr tool.

First, import the display profile file and obtain its newly assigned `Profile ID`:

`user $``colormgr import-profile display_profile.icc | grep "Profile ID"`
Profile ID:    icc-d1c6fc06dd1a4fa72f5fe241aef755d7

Then get the `Device ID` of your device:

`user $``colormgr get-devices | grep "Device ID"`
Device ID:     xrandr-LVDS-1

Now it is possible to associate the profile with the device, followed by making the new profile default by:

`user $``colormgr device-add-profile xrandr-LVDS-1 icc-d1c6fc06dd1a4fa72f5fe241aef755d7``user $``colormgr device-make-profile-default xrandr-LVDS-1 icc-d1c6fc06dd1a4fa72f5fe241aef755d7`
Verify the association has been made correctly:

`user $``colormgr device-get-default-profile xrandr-LVDS-1`
Finally, make sure both colord and xiccd are set to start automatically and reboot the system.

## Application support

### GIMP

In theory [GIMP](https://wiki.gentoo.org/wiki/GIMP) has had color management for a bit longer than most applications, but if someone understands how to configure it to work properly with a CMS, go ahead, till then consider it broken.

### Firefox

Go to about:config and set `gfx.color_management.mode` to `1` from the default value of `2`. This will make [Firefox](https://wiki.gentoo.org/wiki/Firefox) treat untagged images as sRGB which generally is what you want.

Support for ICC v4 profiles<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> can be enabled by setting `gfx.color_management.enablev4` to `true`. The support can be verified by a dedicated [ICC's test page](https://www.color.org/version4html.xalter).

For best results also install a color management system or alternatively set the `gfx.color_management.display_profile` to the correct color profile file. Naturally, this will not work with two displays.

### mpv

When using [mpv](https://wiki.gentoo.org/wiki/Mpv)'s [OpenGL](https://en.wikipedia.org/wiki/Opengl) presentation driver (not the legacy one inherited from MPlayer!) it's possible to configure it to always output sRGB which usually is already the case but could not be the case for nowadays more exotic or old content. Alternatively when built with [LittleCMS](https://en.wikipedia.org/wiki/Little_CMS) (`lcms` USE flag) support the same OpenGL presentation driver can also do full color management including with profile set in window properties but currently it will not tag the surface appropriately so care should be taken that the display color manager does not apply the display color profile again.

For detailed description see [the article on mpv](https://wiki.gentoo.org/wiki/Mpv#Example_user_mpv.conf).

### Darktable

By default [Darktable](https://wiki.gentoo.org/wiki/Darktable) uses the `_ICC_PROFILE` X atom<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> to obtain the color profile. The `colord` USE flag enables additional support for colord. Darktable can be set to use one of those methods or both.

The package bundles a standalone binary utility darktable-cmstest<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> which provides a convenient overview of system CMS' configuration.

## External resources

## TODO

- Color calibration section or even an outright article as it's probably complicated enough on its own on Linux.
- More applications such as \[\[Inkscape, Blender and other graphics processing tools.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Version 4 ICC Specification](https://www.color.org/v4spec.xalter), ICC. Retrieved on September 19, 2022
2. [↑](https://wiki.gentoo.org#cite_ref-2) [ICC Profiles In X Specification](http://www.burtonini.com/computing/x-icc-profiles-spec-0.2.html) (20 Feb 2007, Ross Burton)
3. [↑](https://wiki.gentoo.org#cite_ref-3) [darktable 3.4 user manual - darktable-cmstest](https://www.darktable.org/usermanual/en/special-topics/program-invocation/darktable-cmstest/)
