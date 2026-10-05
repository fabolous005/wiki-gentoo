<!-- source: https://wiki.gentoo.org/wiki/Window_manager | group: Gentoo Wiki (Main) | wiki-title: Window manager -->
---
title: Window manager
url: https://wiki.gentoo.org/wiki/Window_manager
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-05"
categories: ['x11-wm']
fingerprint: "40d76a5f139516dc"
license: CC BY-SA 4.0
---

# Window manager

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A **window manager** (WM) manages the creation, manipulation, and destruction of on-screen windows and window decorations in a GUI environment.

When using [X](https://wiki.gentoo.org/wiki/X), a window manager is usually wanted. This might be provided by a [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment), or as standalone software.

When using the [Wayland](https://wiki.gentoo.org/wiki/Wayland) protocol, a WM is typically part of a [Wayland compositor](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland#Compositors), which requires no 'server'. However, as of 2026-10-05, the [River](https://wiki.gentoo.org/wiki/River) project is working on [a framework for separating a Wayland WM from the compositor functionality](https://isaacfreund.com/blog/river-window-management/).

## Classification

Windows managers can generally be *dynamic*, *stacking*, or *tiling* in their behavior.

- Stacking (or floating) window managers have windows analogous to pieces of paper on a physical desktop, which can be stacked each on top of the others, with the one with which the user interacts on top of the stack, and totally visible.
- Tiling window managers represent windows as tiles, or split views, with windows displayed next to one another, but with none of the windows overlapping.
- Dynamic window managers can dynamically switch between the other two paradigms.

Windows managers can integrate a compositor, for buffering graphics before showing them, allowing visual effects, anti-flicker and other facilities.

## Available software

This is a partial selection of window managers that work with X11 and are available in Gentoo. See [x11-wm](https://packages.gentoo.org/categories/x11-wm) on packages.gentoo.org, or use [eix](https://wiki.gentoo.org/wiki/Eix) ([app-portage/eix](https://packages.gentoo.org/packages/app-portage/eix)).

| Name | Package | Homepage | Description | 
|---|---|---|---|
| aewm | [x11-wm/aewm](https://packages.gentoo.org/packages/x11-wm/aewm) | 404 ( [bug #708484](https://bugs.gentoo.org/show_bug.cgi?id=708484)) | Minimalistic, dynamic X11 window manager. | 
| aewm++ | [x11-wm/aewm++](https://packages.gentoo.org/packages/x11-wm/aewm++) | [https://github.com/frankhale/aewmpp](https://github.com/frankhale/aewmpp) | Dynamic window manager with more modern features than aewm but with the same look and feel. | 
| amiwm | [x11-wm/amiwm](https://packages.gentoo.org/packages/x11-wm/amiwm) | [https://www.lysator.liu.se/\~marcus/amiwm.html](https://www.lysator.liu.se/~marcus/amiwm.html) | Stacking window manager that resembles the Amiga Workbench user interface. | 
| [Awesome](https://wiki.gentoo.org/wiki/Awesome) | [x11-wm/awesome](https://packages.gentoo.org/packages/x11-wm/awesome) | [https://awesomewm.org/](https://awesomewm.org/) | Highly configurable, next generation, dynamic window manager for X. | 
| [blackbox](https://wiki.gentoo.org/wiki/Blackbox) | [x11-wm/blackbox](https://packages.gentoo.org/packages/x11-wm/blackbox) | [https://github.com/bradleythughes/blackbox](https://github.com/bradleythughes/blackbox) | Open-source stacking window manager written in C++ and licensed under the MIT License. | 
| [bspwm](https://wiki.gentoo.org/wiki/Bspwm) | [x11-wm/bspwm](https://packages.gentoo.org/packages/x11-wm/bspwm) | [https://github.com/baskerville/bspwm](https://github.com/baskerville/bspwm) | Lightweight, tiling, minimalist window manager that is written in C and represents its windows as leaves on a binary tree. | 
| CTWM | [x11-wm/ctwm](https://packages.gentoo.org/packages/x11-wm/ctwm) | [https://www.ctwm.org/index.html](https://www.ctwm.org/index.html) | Lightweight, stacking window manager. | 
| cwm | [x11-wm/cwm](https://packages.gentoo.org/packages/x11-wm/cwm) | [https://github.com/leahneukirchen/cwm](https://github.com/leahneukirchen/cwm) | Lightweight, stacking window manager originally developed for OpenBSD. | 
| [dwm](https://wiki.gentoo.org/wiki/Dwm) | [x11-wm/dwm](https://packages.gentoo.org/packages/x11-wm/dwm) | [https://dwm.suckless.org/](https://dwm.suckless.org/) | Dynamic window manager for X11. | 
| echinus | [x11-wm/echinus](https://packages.gentoo.org/packages/x11-wm/echinus) | [https://plhk.ru/](https://plhk.ru/) | Lightweight tiling and floating window manager forked from dwm. | 
| [Enlightenment](https://wiki.gentoo.org/wiki/Enlightenment) | [x11-wm/enlightenment](https://packages.gentoo.org/packages/x11-wm/enlightenment) | [https://www.enlightenment.org/](https://www.enlightenment.org/) | Eye-candy, compositing and stacking window manager that is released under the permissive BSD License. | 
| evilwm | [x11-wm/evilwm](https://packages.gentoo.org/packages/x11-wm/evilwm) | [https://www.6809.org.uk/evilwm/](https://www.6809.org.uk/evilwm/) | Lightweight, stacking window manager. | 
| [fluxbox](https://wiki.gentoo.org/wiki/Fluxbox) | [x11-wm/fluxbox](https://packages.gentoo.org/packages/x11-wm/fluxbox) | [http://fluxbox.org/](http://fluxbox.org/) | Open-source stacking window manager for X11 that was originally forked from Blackbox. | 
| [FVWM](https://wiki.gentoo.org/wiki/FVWM) | [x11-wm/fvwm3](https://packages.gentoo.org/packages/x11-wm/fvwm3) | [http://www.fvwm.org/](http://www.fvwm.org/) | Stacking window manager for X11. | 
| goomwwm | [x11-wm/goomwwm](https://packages.gentoo.org/packages/x11-wm/goomwwm) | [https://github.com/seanpringle/goomwwm](https://github.com/seanpringle/goomwwm) | Get out of my way, Window Manager! | 
| [herbstluftwm](https://wiki.gentoo.org/wiki/Herbstluftwm) | [x11-wm/herbstluftwm](https://packages.gentoo.org/packages/x11-wm/herbstluftwm) | [https://herbstluftwm.org/](https://herbstluftwm.org/) | Manual tiling window manager for X11 using Xlib and Glib. | 
| [JWM](https://wiki.gentoo.org/wiki/JWM) | [x11-wm/jwm](https://packages.gentoo.org/packages/x11-wm/jwm) | [https://github.com/joewing/jwm](https://github.com/joewing/jwm) | Extremely lightweight window manager for the X window system. | 
| [i3](https://wiki.gentoo.org/wiki/I3) | [x11-wm/i3](https://packages.gentoo.org/packages/x11-wm/i3) | [https://i3wm.org/](https://i3wm.org/) | Minimalist tiling window manager, completely written from scratch. | 
| [IceWM](https://wiki.gentoo.org/wiki/IceWM) | [x11-wm/icewm](https://packages.gentoo.org/packages/x11-wm/icewm) | [https://ice-wm.org/](https://ice-wm.org/) | Free and open-source, lightweight, stacking window manager for X11. | 
| KWin | [kde-plasma/kwin](https://packages.gentoo.org/packages/kde-plasma/kwin) | [https://userbase.kde.org/KWin](https://userbase.kde.org/KWin) | [KDE](https://wiki.gentoo.org/wiki/KDE)'s compositing window manager. | 
| larswm | [x11-wm/larswm](https://packages.gentoo.org/packages/x11-wm/larswm) | [http://porneia.free.fr/larswm/larswm.html](http://porneia.free.fr/larswm/larswm.html) | Tiling window manager for X11, based on 9wm. | 
| lwm | [x11-wm/lwm](https://packages.gentoo.org/packages/x11-wm/lwm) | [http://www.jfc.org.uk/software/lwm.html](http://www.jfc.org.uk/software/lwm.html) | Lightweight, stacking window manager. | 
| Marco | [x11-wm/marco](https://packages.gentoo.org/packages/x11-wm/marco) | [https://github.com/mate-desktop/marco](https://github.com/mate-desktop/marco) | [MATE](https://wiki.gentoo.org/wiki/MATE)'s window manager, forked from Metacity, the window manager of GNOME 2. | 
| matwm2 | [x11-wm/matwm2](https://packages.gentoo.org/packages/x11-wm/matwm2) | [https://github.com/segin/matwm2](https://github.com/segin/matwm2) | Simple EWMH compatible window manager with titlebars and frames. | 
| Muffin | [x11-wm/muffin](https://packages.gentoo.org/packages/x11-wm/muffin) | [https://github.com/linuxmint/muffin](https://github.com/linuxmint/muffin) | [Cinnamon](https://wiki.gentoo.org/wiki/Cinnamon)'s compositing window manager. | 
| Musca | [x11-wm/musca](https://packages.gentoo.org/packages/x11-wm/musca) | [https://launchpad.net/musca](https://launchpad.net/musca) | Simple dynamic window manager, with features nicked from ratpoison and dwm. | 
| Mutter | [x11-wm/mutter](https://packages.gentoo.org/packages/x11-wm/mutter) | [https://gitlab.gnome.org/GNOME/mutter/](https://gitlab.gnome.org/GNOME/mutter/) | [GNOME](https://wiki.gentoo.org/wiki/GNOME)'s compositing window manager. | 
| Notion | [x11-wm/notion](https://packages.gentoo.org/packages/x11-wm/notion) | [http://notion.sourceforge.net/](http://notion.sourceforge.net/) | Tiling, tabbed window manager for X11. | 
| [Openbox](https://wiki.gentoo.org/wiki/Openbox) | [x11-wm/openbox](https://packages.gentoo.org/packages/x11-wm/openbox) | [http://openbox.org/](http://openbox.org/) | Highly configurable, next generation, stacking window manager for X11 with extensive standards support. | 
| oroborus | [x11-wm/oroborus](https://packages.gentoo.org/packages/x11-wm/oroborus) | [https://www.oroborus.org/](https://www.oroborus.org/) (link seems wrong, as of 2022-11) | Small and fast window manager. | 
| page | [x11-wm/page](https://packages.gentoo.org/packages/x11-wm/page) | [https://github.com/gschwind/page](https://github.com/gschwind/page) | Mouse-friendly tiling window manager. | 
| PekWM | [x11-wm/pekwm](https://packages.gentoo.org/packages/x11-wm/pekwm) | [https://www.pekwm.se/](https://www.pekwm.se/) | Lightweight, dynamic window manager originally forked from aewm++. | 
| [Qtile](https://wiki.gentoo.org/wiki/Qtile) | [x11-wm/qtile](https://packages.gentoo.org/packages/x11-wm/qtile) | [http://www.qtile.org/](http://www.qtile.org/) | Open-source, tiling window manager that is written in and extended with the Python programming language. | 
| [ratpoison](https://wiki.gentoo.org/wiki/Ratpoison) | [x11-wm/ratpoison](https://packages.gentoo.org/packages/x11-wm/ratpoison) | [https://nongnu.org/ratpoison/](https://nongnu.org/ratpoison/) | Tiling window manager modeled after screen. | 
| Sith WM | [x11-wm/sithwm](https://packages.gentoo.org/packages/x11-wm/sithwm) | [https://sithwm.darkside.no/](https://sithwm.darkside.no/) | Minimalist window manager for X11. | 
| spectrwm | [x11-wm/spectrwm](https://packages.gentoo.org/packages/x11-wm/spectrwm) | [http://srobb.net/spectrwm.html](http://srobb.net/spectrwm.html) | Small dynamic tiling window manager for X11. | 
| StumpWM | [x11-wm/stumpwm](https://packages.gentoo.org/packages/x11-wm/stumpwm) | [https://stumpwm.github.io/](https://stumpwm.github.io/) | Tiling window manager written entirely in Common Lisp. | 
| twm | [x11-wm/twm](https://packages.gentoo.org/packages/x11-wm/twm) | [https://gitlab.freedesktop.org/xorg/app/twm](https://gitlab.freedesktop.org/xorg/app/twm) | Simple stacking window manager started written in C. | 
| WindowLab | [x11-wm/windowlab](https://packages.gentoo.org/packages/x11-wm/windowlab) | [https://github.com/nickgravgaard/windowlab](https://github.com/nickgravgaard/windowlab) | Small and simple window manager of novel design. | 
| Window Maker | [x11-wm/windowmaker](https://packages.gentoo.org/packages/x11-wm/windowmaker) | [http://www.windowmaker.org/](http://www.windowmaker.org/) | Fast and light GNUstep window manager. | 
| [wm2](https://wiki.gentoo.org/wiki/Wm2) | [x11-wm/wm2](https://packages.gentoo.org/packages/x11-wm/wm2) | [https://www.all-day-breakfast.com/wm2/](https://www.all-day-breakfast.com/wm2/) | Minimalist window manager for X11. | 
| Xfwm | [xfce-base/xfwm4](https://packages.gentoo.org/packages/xfce-base/xfwm4) | [https://docs.xfce.org/xfce/xfwm4/start](https://docs.xfce.org/xfce/xfwm4/start) | [Xfce](https://wiki.gentoo.org/wiki/Xfce)'s compositing window manager. | 
| [xmonad](https://wiki.gentoo.org/wiki/Xmonad) | [x11-wm/xmonad](https://packages.gentoo.org/packages/x11-wm/xmonad) | [https://xmonad.org/](https://xmonad.org/) | Fast and lightweight tiling window manager for X11. | 

## See also

- [Desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) — provides a list of desktop environments available in Gentoo.
- [Display manager](https://wiki.gentoo.org/wiki/Display_manager) — presents the user with a graphical login screen to start a GUI session, either [X](https://wiki.gentoo.org/wiki/Xorg) or [Wayland](https://wiki.gentoo.org/wiki/Wayland).

## External resources

- [Comparison of X window managers](https://en.wikipedia.org/wiki/Comparison_of_X_window_managers) (Wikipedia)
