<!-- source: https://wiki.gentoo.org/wiki//etc/portage/package.use | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/package.use -->
---
title: "/etc/portage/package.use"
url: https://wiki.gentoo.org/wiki//etc/portage/package.use
hostname: gentoo.org
sitename: "/etc/portage/package.use"
date: "2025-06-23"
fingerprint: e0f79afc6eaf978d
license: CC BY-SA 4.0
---

# /etc/portage/package.use

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/etc/portage/package.use** provides a more fine grained **[per-package control](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/USE#Declaring_USE_flags_for_individual_packages) of [USE flags](https://wiki.gentoo.org/wiki/USE_flag)** than the `USE` variable in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE).

With the default `[USE_ORDER](https://wiki.gentoo.org/wiki/USE_ORDER)` setting, the /etc/portage/package.use file or directory will override individual package settings coming from all locations except for the `USE` environment variable.

## Example

**`/etc/portage/package.use`**

**Example with this location as a single file**

```
# Globally disable the unwanted USE flags which were enabled by the profile
*/* -bluetooth -dbus -ldap -libnotify -nls -udisks
 
# enable the offensive USE flag for app-admin/sudo
app-admin/sudo offensive
 
# disable mysql support for dev-lang/php
dev-lang/php -mysql 
 
# enable java and set the python interpreter version for libreoffice
app-office/libreoffice java PYTHON_SINGLE_TARGET: python3_11
```
**`/etc/portage/package.use/openrct`**

**Example with this location as a directory**

```
# Disable Vorbis support in OpenRCT2
games-simulation/openrct2 -vorbis
```
For more details see [the Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE#Declaring_USE_flags_for_individual_packages).

## Format

- One `DEPEND` atom per line with space-delimited [USE flags](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/USE).
- Comment lines begin with `#` (hash).

## Automatically generated content

emerge has the `--autounmask` option enabled by default (see man 1 emerge), so it can generate package.use settings as necessary to satisfy dependencies.

## Finding USE flags set

With all the will in the world, mistakes will happen so below are some tips to help find a USE flag that was set and can no longer be found.

In this example, the [lua](https://packages.gentoo.org/useflags/lua) [USE flag was set for](https://wiki.gentoo.org/wiki/USE_flag) [media-video/obs-studio](https://packages.gentoo.org/packages/media-video/obs-studio), but is no longer required.

`user $````
grep --recursive "lua" /etc/portage/
```
/etc/portage/package.use/obs:media-video/obs-studio nvenc browser speex fdk lua python qsv v4l vlc

/etc/portage/package.use/scummvm:games-engines/scummvm fluidsynth -fribidi lua mpeg2 sndio speech theora unsupported

/etc/portage/package.use/zz-automask:>=dev-lua/lgi-0.9.2-r100 lua_targets_luajit
It can be seen that the USE flag is in /etc/portage/package.use/obs and can be quickly added and removed.

## External resources

- [https://packages.gentoo.org/useflags](https://packages.gentoo.org/useflags) - USE flags on Gentoo Packages Database
- [Portage man page](https://dev.gentoo.org/~zmedico/portage/doc/man/portage.5.html)
- [Setting USE\_EXPAND flags in package.use](https://blog.cafarelli.fr/2016/02/setting-use_expand-flags-in-package-use/) - blog post by Bernard Cafarelli
- [Cleaning /etc/portage/package.\* from unused entries](https://wiki.gentoo.org/wiki/User:Tillschaefer/cleanup_package)
- [Find obsolete USE flags in /etc/portage/package.use](https://forums.gentoo.org/viewtopic-t-897206-start-11.html) - Gentoo forums thread
