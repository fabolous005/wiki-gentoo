<!-- source: https://wiki.gentoo.org/wiki/Gentoolkit | group: Gentoo Wiki (Main) | wiki-title: Gentoolkit -->
---
title: gentoolkit
url: https://wiki.gentoo.org/wiki/Gentoolkit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-15"
fingerprint: b7513a6e82e3bf9a
license: CC BY-SA 4.0
---

# gentoolkit

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


*Not to be confused with[Genkernel](https://wiki.gentoo.org/wiki/Genkernel).*

**gentoolkit** is a suite of tools to ease the administration of a Gentoo system, and [Portage](https://wiki.gentoo.org/wiki/Portage) in particular.

Gentoo is a very unique distribution, with certain specifics that are not present in other systems. Several tools developed to help with Gentoo usage have been contributed, and are grouped in the [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package.

gentoolkit contains tools to help users manage packages and keep track of what is going on in their systems. Most users - particularly those who update systems often - will benefit from having gentoolkit installed.

The gentoolkit commands have [man pages](https://wiki.gentoo.org/wiki/Man_page), type man \<command> for each command for full documentation.

## Installation

### USE flags


### USE flags for
            [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit)
            
            Collection of administration scripts for Gentoo

| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Install gentoolkit:

`root #``emerge --ask app-portage/gentoolkit`
## eclean

**[eclean](https://wiki.gentoo.org/wiki/Eclean)** is a tool to remove old source files and old binary packages from the system.

When building and installing packages, the source files are downloaded and preserved in `DISTDIR`, usually /var/cache/distfiles. This can accumulate several gigabytes of material over time if it is not cleaned periodically. Users should run eclean-dist to clean source files from `DISTDIR`.

It is possible to create archives of installed packages by using quickpkg or `FEATURES="buildpkg"`. These archived packages are kept in `PKGDIR`, usually /var/cache/binpkgs. When they are no longer needed, or if they are too old, eclean-pkg can be run to remove them from `PKGDIR`. It is a good way to ensure that any binary packages on the system are only the latest versions.

For more information on eclean and tips on maintaining a cruft-free system, please read man eclean or check the [eclean](https://wiki.gentoo.org/wiki/Eclean) article.

## equery

**[equery](https://wiki.gentoo.org/wiki/Equery)** is a tool designed to make asking portage for information easier. Among other operations, it can display package dependencies, metadata, and installed files.

### epkginfo

**[epkginfo](https://wiki.gentoo.org/wiki/Epkginfo)** is an alias for equery meta command.

### eshowkw

**eshowkw** is an alias for equery keywords command.

## eread

eread is a simple utility to display elog files produced by Portage since version 2.1. The saving of elog files can be enabled by setting a couple of variables in /etc/portage/make.conf:

**`make.conf`**

**Enabling elog**

```
PORTAGE_ELOG_CLASSES="log"
PORTAGE_ELOG_SYSTEM="save"
```
Once elog has been set up to satisfaction, run eread to view the log files.

`user $``eread`
This is a list of portage log items. Choose a number to view that file or type
q to quit.
  
1) app-portage:gentoolkit-0.2.4\_pre2:20070320-000256.log
2) app-portage:gentoolkit-0.2.4\_pre2:20070320-000258.log
3) app-portage:gentoolkit-0.2.4\_pre2:20070320-000319.log
4) app-portage:gentoolkit-0.2.3:20070320-000408.log
Choice?

Select a number and the file will be displayed using the paging program specified in the `PAGER` environment variable. If `PAGER` is not set, it will use less. The `PAGER` environmental variable can be set using eselect (module `pager`).

After displaying the elog item, the user will be prompted to delete the file.

## euse

**[euse](https://wiki.gentoo.org/wiki/Euse)** provides functionality to set (disable/enable) and obtain information about [USE flags](https://wiki.gentoo.org/wiki/USE_flag) in [make.conf](https://wiki.gentoo.org/wiki/Make.conf), without having to edit the file directly.

## revdep-rebuild

This tool is Gentoo's **rev**erse **dep**endency rebuilder. It will scan the installed ebuilds to find packages that have become broken as a result of an upgrade of a package they depend on. It can emerge those packages for users automatically but it can also happen that a given package does not work with the currently installed dependencies, in which case the broken package should be upgraded to a more recent version.

revdep-rebuild will pass flags to emerge which lets the `--pretend` flag pass through, to see what is going to be emerged again before going any further.

`user $``revdep-rebuild -p`
\* Configuring search environment for revdep-rebuild
  
 \* Checking reverse dependencies
 \* Packages containing binaries and libraries broken by a package update
 \* will be emerged.
  
 \* Collecting system binaries and libraries
 \* Generated new 1\_files.rr
 \* Collecting complete LD\_LIBRARY\_PATH
 \* Generated new 2\_ldpath.rr
 \* Checking dynamic linking consistency
\[ 48% \]  \*   broken /usr/lib/gstreamer-0.10/libgsttaglib.la (requires /usr/lib/libtag.la)
\[ 64% \]  \*   broken /usr/lib/libgdkglext-x11-1.0.la (requires /usr/lib/libGLU.la)
\[ 67% \]  \*   broken /usr/lib/libgtkglext-x11-1.0.la (requires /usr/lib/libGLU.la)
\[ 85% \]  \*   broken /usr/lib/python2.6/site-packages/gtk-2.0/gtk/gdkgl/\_gdkgl.la (requires /usr/lib/libGLU.la)
 \*   broken /usr/lib/python2.6/site-packages/gtk-2.0/gtk/gtkgl/\_gtkgl.la (requires /usr/lib/libGLU.la)
\[ 97% \]  \*   broken /usr/qt/3/lib/libqt-mt.la (requires -lpng)
\[ 100% \]
 \* Generated new 3\_broken.rr
 \* Assigning files to packages
 \*   /usr/lib/gstreamer-0.10/libgsttaglib.la -> media-plugins/gst-plugins-taglib
 \*   /usr/lib/libgdkglext-x11-1.0.la -> x11-libs/gtkglext
 \*   /usr/lib/libgtkglext-x11-1.0.la -> x11-libs/gtkglext
 \*   /usr/lib/python2.6/site-packages/gtk-2.0/gtk/gdkgl/\_gdkgl.la -> dev-python/pygtkglext
 \*   /usr/lib/python2.6/site-packages/gtk-2.0/gtk/gtkgl/\_gtkgl.la -> dev-python/pygtkglext
 \*   /usr/qt/3/lib/libqt-mt.la -> x11-libs/qt
 \* Generated new 4\_raw.rr and 4\_owners.rr
 \* Cleaning list of packages to rebuild
 \* Generated new 4\_pkgs.rr
 \* Assigning packages to ebuilds
 \* Generated new 4\_ebuilds.rr
 \* Evaluating package order
 \* Generated new 5\_order.rr
 \* All prepared. Starting rebuild
emerge --oneshot --pretend  dev-python/pygtkglext:0
media-plugins/gst-plugins-taglib:0.10
x11-libs/gtkglext:0
x11-libs/qt:3
  
These are the packages that would be merged, in order:
  
Calculating dependencies... done!
\[ebuild   R   \] media-plugins/gst-plugins-taglib-0.10.17
\[ebuild   R   \] x11-libs/gtkglext-1.2.0
\[ebuild   R   \] x11-libs/qt-3.3.8b-r2
\[ebuild   R   \] dev-python/pygtkglext-1.1.0
 \* Now you can remove -p (or --pretend) from arguments and re-run revdep-rebuild.

To rebuild some packages run revdep-rebuild without the `-p` flag and the listed packages will be emerged again.

`root #``revdep-rebuild`
## Other tools

gentoolkit also provides other tools:

| Name | Description | 
|---|---|
| ebump | Ebuild revision bumper (more useful for developers). | 
| enalyze | Gentoo's installed packages analysis and repair tool. See man page, which states "CAUTION: This is beta software and is not yet feature complete". | 
| imlate | Displays candidates for keywords for an architecture (more useful for developers?). | 

See the [man pages](https://wiki.gentoo.org/wiki/Man_page) for each of these tools for more info.

## See also

- [Q applets](https://wiki.gentoo.org/wiki/Q_applets) — a collection of small, fast [Portage](https://wiki.gentoo.org/wiki/Portage) **q**uery utilities written in C.
- [Useful Portage tools](https://wiki.gentoo.org/wiki/Useful_Portage_tools) — provides a list of Gentoo-specific system management tools, notably for [Portage](https://wiki.gentoo.org/wiki/Portage), available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).
