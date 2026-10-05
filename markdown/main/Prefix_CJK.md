<!-- source: https://wiki.gentoo.org/wiki/Prefix/CJK | group: Gentoo Wiki (Main) | wiki-title: Prefix/CJK -->
---
title: Prefix/CJK
url: https://wiki.gentoo.org/wiki/Prefix/CJK
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-04-16"
fingerprint: c6b4df4cfa81ebc2
license: CC BY-SA 4.0
---

# Prefix/CJK

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Prefix is mostly an independent system from the host. Therefore it is possible to get full CJK features within Prefix despite of the host.

In this guide, a solution of [app-emacs/scim-bridge-el](https://packages.gentoo.org/packages/app-emacs/scim-bridge-el) in [app-editors/emacs](https://packages.gentoo.org/packages/app-editors/emacs) running on [x11-wm/xpra](https://packages.gentoo.org/packages/x11-wm/xpra) is given.

## The remote X server

[x11-wm/xpra](https://packages.gentoo.org/packages/x11-wm/xpra) is the "screen for X", it allows you to run X programs, usually on a remote host and direct their display to your local machine.

In most of the cases, Prefix does not have access to the real input/output devices such as the keyboard and display on the host. It is preferrable to only use dummy drivers.

**`${EPREFIX}/etc/portage/make.conf`**

```
VIDEO_CARDS="dummy"
INPUT_DEVICES="void"
```
Install xpra,

`root #``emerge --ask x11-wm/xpra`
Next, install standard X and CJK fonts:

`root #``emerge --ask media-fonts/font-misc-misc media-fonts/efont-unicode media-fonts/wqy-bitmapfont media-fonts/font-adobe-75dpi media-fonts/font-adobe-100dpi`
### Setup fonts for Xpra

Tell Xpra to use Xorg instead of Xvfb.

**`${EPREFIX}/etc/xpra/xpra.conf`**

The location of the fonts needs to be told to the Xorg of Xpra. Fonts from the host can be used, too.

**`${EPREFIX}/etc/xpra/xorg.conf`**

```
Section "Files"
  FontPath "/home/benda/gnto/usr/share/fonts/wqy-bitmapfont"
  FontPath "catalogue:/etc/X11/fontpath.d"
EndSection
```
The wqy-bitmapfont line are quite straight forward, while the second line is special to a RHEL-like host.

### Start Xpra

Start Xpra and verify the fonts are installed:

`user $``xpra start :10` `user $``export DISPLAY=:10` `user $``xlsfonts | grep '9x15\|wenquanyi'`
...
-wenquanyi-wenquanyi bitmap song-medium-r-normal--13-130-75-75-p-80-iso10646-1
-wenquanyi-wenquanyi bitmap song-medium-r-normal--15-150-75-75-p-80-iso10646-1
-wenquanyi-wenquanyi bitmap song-medium-r-normal--16-160-75-75-p-80-iso10646-1
9x15
...

Consult the [xpra homepage](http://xpra.org) for its basic usage.

## Emacs and SCIM

[app-emacs/scim-bridge-el](https://packages.gentoo.org/packages/app-emacs/scim-bridge-el) will pull in [app-editor/emacs](https://packages.gentoo.org/packages/app-editor/emacs) and [app-i18n/scim](https://packages.gentoo.org/packages/app-i18n/scim).

`root #``emerge --ask app-emacs/scim-bridge-el`
Follow the tips in the [Emacs wiki](http://www.emacswiki.org/emacs/ScimBridge) to have your scim-bridge-el setup in Emacs.
