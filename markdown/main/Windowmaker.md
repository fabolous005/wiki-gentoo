<!-- source: https://wiki.gentoo.org/wiki/Windowmaker | group: Gentoo Wiki (Main) | wiki-title: Windowmaker -->
---
title: Windowmaker
url: https://wiki.gentoo.org/wiki/Windowmaker
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-05"
categories: ['x11-plugins']
fingerprint: ce031a5dd0b2f9dc
license: CC BY-SA 4.0
---

# Windowmaker

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Windowmaker** is a lightweight window manager for X11 that was originally designed for the GNUstep desktop environment. However, that does not stop Windowmaker from being used as a standalone window manager.

## Installation

### USE flags


### USE flags for
            [x11-wm/windowmaker](https://packages.gentoo.org/packages/x11-wm/windowmaker)
            
            The fast and light GNUstep window manager

| [+xpm](https://packages.gentoo.org/useflags/+xpm) | Add support for XPM graphics format | 
| [gif](https://packages.gentoo.org/useflags/gif) | Add GIF image support | 
| [imagemagick](https://packages.gentoo.org/useflags/imagemagick) | Enable optional support for the ImageMagick or GraphicsMagick image converter | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [modelock](https://packages.gentoo.org/useflags/modelock) | Enable XKB language status lock support. README says: "If you don't know what it is you probably don't need it." | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [png](https://packages.gentoo.org/useflags/png) | Add support for libpng (PNG images) | 
| [tiff](https://packages.gentoo.org/useflags/tiff) | Add support for the TIFF image format | 
| [webp](https://packages.gentoo.org/useflags/webp) | Add support for the WebP image format | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 
| [xrandr](https://packages.gentoo.org/useflags/xrandr) | Enable XRandR window extension support | 

### Emerge

A user would want to emerge Windowmaker with the following:

`root #``emerge --ask x11-wm/windowmaker`
## Configuration

### WPrefs

For regular configuration of Windowmaker, you can use **WPrefs**, which is designed to make configuring Windowmaker simple and straightforward.

Use the WPrefs icon to open the app, or right click on your desktop, and select *Configure Windowmaker* in the drop-down menu.

### Manual configuration

For sophisticated configuration, [Windowmaker's documentation](https://www.windowmaker.org/docs/chap4.html) gives another straightforward guide to configuring Windowmaker manually.

To be specific, Windowmaker uses a "defaults" database to store configuration. This is stored in the home directory under GNUstep and Defaults. (\~/GNUstep/Defaults), where you can look and edit any configuration to your desire.

## Tips

### Audio keys

If binding volume keys is not automatically supported on your Windowmaker install, you can add audio keys by emerging [xbindkeys](https://packages.gentoo.org/packages/x11-misc/xbindkeys) and binding your audio keys. The following \~/.xbindkeysrc configuration can achieve volume key support using ALSA:

**`/.xbindkeysrc`**

```
"amixer set Master 5%-"
   XF86AudioLowerVolume
"amixer set Master 5%+"
   XF86AudioRaiseVolume
"amixer set Master toggle"
   XF86AudioMute
```
This command will bind your audio keys if you use ALSA. Make sure to automatically start `xbindkeys` in \~/.xinitrc with **startx** or you will have to manually start it everytime.

If you use Pulseaudio or PipeWire you can simply replace the lines with **amixer** for whatever your sound server is.

### Running a program

Windowmaker includes it's own application launcher by default. You can access this by right clicking, and selecting *Run* from the drop-down menu.

### Pinning icons

If a user so wished to pin applications to their desktop, they can achieve this by running the following application, right clicking the icon it shows up (the icon will not be destroyed by default) then selecting the **Lock** attribute which prevents accidental deletion of icons.

To make it easier, you can right click a window title, select the **Attributes** button, go to **Advanced options**, then select **Emulate this app Icon** to add the icon to your desktop.

### Plugins

You can install both plugins specifically for Windowmaker, and general plugins under the [x11-plugins](https://packages.gentoo.org/categories/x11-plugins) category in Gentoo's package repository.

Emerge them, then launch them using any application launcher of your choice, then right click the icon and both select to start when Windowmaker starts, and to lock the icon. This prevents accidental deletion of the icon while still making it active on your desktop.
