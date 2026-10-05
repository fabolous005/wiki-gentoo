<!-- source: https://wiki.gentoo.org/wiki/GTK_themes_in_Qt_applications | group: Gentoo Wiki (Main) | wiki-title: GTK themes in Qt applications -->
---
title: GTK themes in Qt applications
url: https://wiki.gentoo.org/wiki/GTK_themes_in_Qt_applications
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-04-21"
fingerprint: "981d53f9bcb79dd9"
license: CC BY-SA 4.0
---

# GTK themes in Qt applications

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide will explain how to set up Qt to adopt the same style as GTK.

## Setup

The GTK style for Qt can be built by setting the `gtk` `USE` flag for [dev-qt/qtwidgets](https://packages.gentoo.org/packages/dev-qt/qtwidgets).

Set the `gtk` `USE` flag:

**`/etc/portage/package.use`**

Now rebuild the package with its new `USE` flag:

`root #``emerge --ask --changed-use --oneshot dev-qt/qtwidgets`
## Configuration

### Qt5

Qt5 will try to inherit the configuration of the current desktop environment.

In some desktop environments, Qt5 does not pick up the configuration. This can sometimes be fixed by using the GNOME or KDE desktop preference utility and setting the `XDG_CURRENT_DESKTOP` environment variable to `KDE` or `GNOME` respectively.

Alternatively you could use the [x11-misc/qt5ct](https://packages.gentoo.org/packages/x11-misc/qt5ct) application and set the `QT_QPA_PLATFORMTHEME` environment variable to `qt5ct`. The "oxygen" icon packs may sometimes be necessary.

### Qt3 / Qt4

Reselecting the preferred theme may be necessary for Qt applications using qtconfig (from [dev-qt/qt3support](https://packages.gentoo.org/packages/dev-qt/qt3support)):

`user $``qtconfig`
Alternatively, the previous configuration files may be removed:

`user $``rm -r ~/.config/Trolltech*`
Select the **Default** theme to use the system settings or set it to use the **GTK** style explicitly by selecting **GTK** theme.

### Tips

- Individual applications might have their own configuration settings for their GUI, e.g. in VLC this is located in Tools → Preferences.

## GTK 2 alternative

An alternative for users of GTK 2 and Qt5 is using the QtCurve cross-toolkit theme:

`root #``emerge --ask x11-themes/qtcurve`
The downsides of this method are that it's not available for GTK 3 yet, and currently the only configuration GUI needs Qt5 and KDE Frameworks 5.

## External resources

- [*Uniform look for Qt and GTK applications*](https://wiki.archlinux.org/index.php/Uniform_look_for_Qt_and_GTK_applications) on Archlinux's wiki
- [*Configuration of Qt5 apps under environments other than KDE Plasma*](https://wiki.archlinux.org/index.php/qt#Configuration_of_Qt5_apps_under_environments_other_than_KDE_Plasma) on Archlinux's wiki
