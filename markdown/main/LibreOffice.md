<!-- source: https://wiki.gentoo.org/wiki/LibreOffice | group: Gentoo Wiki (Main) | wiki-title: LibreOffice -->
---
title: LibreOffice
url: https://wiki.gentoo.org/wiki/LibreOffice
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-19"
fingerprint: "6e00061c488e39c4"
license: CC BY-SA 4.0
---

# LibreOffice

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**LibreOffice** is a full office productivity suite. It's a successor to OpenOffice.org and strives to be a better and less bloated office suite.[\[1\]](https://wiki.gentoo.org#cite_note-1)[\[2\]](https://wiki.gentoo.org#cite_note-2)


| [+branding](https://packages.gentoo.org/useflags/+branding) | Enable Gentoo specific branding | 
| [+cups](https://packages.gentoo.org/useflags/+cups) | Add support for CUPS (Common Unix Printing System) | 
| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [+gtk3](https://packages.gentoo.org/useflags/+gtk3) | Enable support for x11-libs/gtk+:3 | 
| [+mariadb](https://packages.gentoo.org/useflags/+mariadb) | Prefer mariadb connector over mysql connector | 
| [accessibility](https://packages.gentoo.org/useflags/accessibility) | Add support for accessibility (eg 'at-spi' library) | 
| [base](https://packages.gentoo.org/useflags/base) | Enable full support for LibreOffice Base databases (involves additional bundled libs) | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [coinmp](https://packages.gentoo.org/useflags/coinmp) | Use sci-libs/coinor-mp as alternative solver | 
| [custom-cflags](https://packages.gentoo.org/useflags/custom-cflags) | Build with user-specified CFLAGS (unsupported) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [eds](https://packages.gentoo.org/useflags/eds) | Enable support for Evolution-Data-Server (EDS) | 
| [googledrive](https://packages.gentoo.org/useflags/googledrive) | Enable support for remote files on Google Drive | 
| [gstreamer](https://packages.gentoo.org/useflags/gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [gtk4](https://packages.gentoo.org/useflags/gtk4) | Enable support for gui-libs/gtk:4 | 
| [java](https://packages.gentoo.org/useflags/java) | Add support for Java | 
| [kde](https://packages.gentoo.org/useflags/kde) | Add support for software made by KDE, a free software community | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [odk](https://packages.gentoo.org/useflags/odk) | Build the Office Development Kit | 
| [pdfimport](https://packages.gentoo.org/useflags/pdfimport) | Enable PDF import via the Poppler library | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [vulkan](https://packages.gentoo.org/useflags/vulkan) | Enable Vulkan usage via the skia library (clang recommended) | 

LibreOffice does not start without a database enabled. Most users should choose one to enable.

LibreOffice can be built to support multiple GUI frameworks, specifically [GTK3](https://wiki.gentoo.org/wiki/GTK#GTK_3), [GTK4](https://wiki.gentoo.org/wiki/GTK#GTK_4), and [Qt](https://wiki.gentoo.org/wiki/Qt). It can also be built without these.

It is possible to build with support for one, or multiple, of these frameworks.

- `USE="gtk4"` will enable GTK4 support.
- `USE="gtk3"` will enable GTK3 support.
- `USE="qt6"` will enable Qt6 support, and is most useful for [KDE](https://wiki.gentoo.org/wiki/KDE) and other Qt-based desktop environments.
- `USE="-gtk -gtk3 -gtk4 -qt6"` will disable all of them if they are not required, but this will prevent theming the software with GTK or Qt themes.

Install [app-office/libreoffice](https://packages.gentoo.org/packages/app-office/libreoffice):

`root #``emerge --ask --verbose app-office/libreoffice`
Since LibreOffice is such a large package to compile, it is also offered in pre-compiled binaries in [app-office/libreoffice-bin](https://packages.gentoo.org/packages/app-office/libreoffice-bin).

`root #``emerge --ask --verbose app-office/libreoffice-bin`
The binary packages are compiled such that they fit to the libraries of a stable Gentoo system. Attempting to use them on an \~arch system may result in difficulties; this is not supported.

The [app-office/libreoffice-bin](https://packages.gentoo.org/packages/app-office/libreoffice-bin) may not be compatible with the use of the *Base* application. In this case it may be necessary to use a source build and enable the [java](https://packages.gentoo.org/useflags/java) [and](https://wiki.gentoo.org/wiki/USE_flag) [firebird](https://packages.gentoo.org/useflags/firebird) [USE flags](https://wiki.gentoo.org/wiki/USE_flag).

By default, LibreOffice uses [app-text/hunspell](https://packages.gentoo.org/packages/app-text/hunspell) for spell checking. To enable spell checking for a specific language, enable the corresponding language via the [L10N](https://wiki.gentoo.org/wiki/Localization) `USE_EXPAND` variable, e.g.:

**`/etc/portage/package.use/00local`**

**en-GB localization example**

A package specific to the Finnish language is also available: [app-office/libreoffice-voikko](https://packages.gentoo.org/packages/app-office/libreoffice-voikko).

The `SAL_USE_VCLPLUGIN` environment variable might need to be set to the desired toolkit, e.g. `qt6`. On [LXQt](https://wiki.gentoo.org/wiki/LXQt) this can be done in the LXQt Session settings. Failure to do so may result in a LibreOffice UI with a tiny font and incorrect background color.
