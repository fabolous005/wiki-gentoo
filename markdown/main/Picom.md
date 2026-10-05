<!-- source: https://wiki.gentoo.org/wiki/Picom | group: Gentoo Wiki (Main) | wiki-title: Picom -->
---
title: picom
url: https://wiki.gentoo.org/wiki/Picom
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-25"
fingerprint: f608537f7cf75bcd
license: CC BY-SA 4.0
---

# picom

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**picom** is a lightweight compositor for [X](https://wiki.gentoo.org/wiki/Xorg). It was forked from Compton, which is no longer maintained<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

Picom allows for various effects including window transparency, background blur, rounded window corners, and rules which can be applied to specific windows or applications.

There are three different render backends available: *glx*, *xrender*, and *xr\_glx\_hybrid*, with the first one being the preferred and performant option <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.


## Installation


### USE flags


| [+doc](https://packages.gentoo.org/useflags/+doc) | Build documentation and man pages (requires app-text/asciidoc) | 
| [+drm](https://packages.gentoo.org/useflags/+drm) | Enable support for using drm for vsync | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [opengl](https://packages.gentoo.org/useflags/opengl) | Enable features that require opengl (opengl backend, and opengl vsync methods) | 
| [pcre](https://packages.gentoo.org/useflags/pcre) | Add support for Perl Compatible Regular Expressions | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 


### Emerge

`root #``emerge --ask x11-misc/picom`

## Configuration

By default, the configuration file is read from locations listed below. A configuration file can be specified by the `--config` parameter, although this is only necessary if the configuration file is in a non-standard location.

An on-change configuration autoload is supported.


### Files

If no `--config` parameter is provided the following files are searched for in the following order:

- $XDG\_CONFIG\_HOME/picom.conf - Local (per user) configuration file.
- $XDG\_CONFIG\_HOME/picom/picom.conf - Local (per user) configuration file.
- $XDG\_CONFIG\_DIRS/picom.conf - Global (system wide) configuration file.
- $XDG\_CONFIG\_DIRS/picom/picom.conf - Global (system wide) configuration file.

The [XDG/Base Directories](https://wiki.gentoo.org/wiki/XDG/Base_Directories) standard specifies that, by default, `XDG_CONFIG_HOME` is \~/.config, and `XDG_CONFIG_DIRS` is /etc/xdg.

`user $``mkdir --verbose --parents ~/.config/picom`
mkdir: created directory '/home/larry/.config/picom

`user $``bzcat --verbose /usr/share/doc/picom-*/picom.sample.conf.bz2 > ~/.config/picom/picom.conf`
/usr/share/doc/picom-13/picom.sample.conf.bz2: done


## Usage

Picom can be started as a background process:

`user $``picom --daemon`
To start Picom without an existing configuration file, the `--backend` parameter is mandatory:

`user $``picom --backend [ xrender | glx | xr_glx_hybrid ]`
The backend can also be specified in picom.conf:

**`picom.conf`**


## Tips


For those using [dwm](https://wiki.gentoo.org/wiki/Dwm) and [dmenu(1)](https://man.archlinux.org/man/dmenu.1.en)[, it's possible to exclude the status bar from rules. For example, to exclude the dwm status bar from having corner radius:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

**`~/.config/picom/picom.conf`**

Similarly, dmenu can also be matched with `"class_i = 'dmenu'"`.


### Screen tearing

X may produce noticeable screen tearing in some situations, which can be alleviated by launching picom with the `--vsync` option, such as in \~/.xinitrc:

**`~/.xinitrc`**

```
 --backend glx --vsync &
```
or in picom.conf:

**`picom.conf`**

However, enabling vsync this way may result in significant latency.


### Enabling triple buffering and ForceFullCompositionPipeline

Both triple buffering and ForceFullCompositionPipeline can provide lower latency (compared to picom's vsync), while also mitigating screen tearing effectively.

To enable triple buffering and ForceFullCompositionPipeline, first run:

`root #``nvidia-xconfig`
This will produce a config file at /etc/X11/xorg.conf. This should be moved to /etc/X11/xorg.conf.d/20-nvidia.conf:

`root #``mkdir -p /etc/X11/xorg.conf.d``root #``mv /etc/X11/xorg.conf /etc/X11/xorg.conf.d/20-nvidia.conf`
Alternatively, this file can be written manually. This may be desirable if other configuration changes are required.

Edit `Section "Device"` in /etc/X11/xorg.conf.d/20-nvidia.conf to add the following lines:

**`/etc/X11/xorg.conf.d/20-nvidia.conf`**

This `Section "Device"` is an example of how it may look once edited:

**`/etc/X11/xorg.conf.d/20-nvidia.conf`**

It may be necessary to run this command to load the new configuration:

`user $``nvidia-settings --load-config-only`
Or to include it in \~/.xinitrc:

**`~/.xinitrc`**

```
 --load-config-only &
```
Finally, run Picom with vsync disabled:

**`~/.xinitrc`**

```
 --no-vsync &
```
or in picom.conf:

**`picom.conf`**


## External Resources
