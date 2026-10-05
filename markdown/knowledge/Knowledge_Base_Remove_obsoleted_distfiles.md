<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Remove_obsoleted_distfiles | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Remove_obsoleted_distfiles -->
---
title: Knowledge Base:Remove obsoleted distfiles
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Remove_obsoleted_distfiles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-10-14"
fingerprint: "2f80395f949b032a"
license: CC BY-SA 4.0
---

# Knowledge Base:Remove obsoleted distfiles

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This knowledge base article provides instructions for system administrators on how to clean up Portage's `DISTDIR` directory.

## Synopsis

The `[DISTDIR](https://wiki.gentoo.org/wiki/DISTDIR)` location hosts all the source code archives downloaded by Portage. Unless the files are properly removed when no longer needed, the storage occupied by this location will continue to grow. Regular clean-up of this location is *not* performed automatically by Portage, therefore clean up is at the discretion of the system administrator.

## Environment

Any Gentoo Linux installation.

## Analysis

When Portage needs to download source code archives, it will store these archives in `[DISTDIR](https://wiki.gentoo.org/wiki/DISTDIR)` for later use. Portage will never clean up this location by itself; it will wait for a user with root privileges to do so.

Cleaning up the `DISTDIR` location means that the system administrator should check which source code archives to keep and which to delete.

- If the system is tight on space and there are no problems in re-downloading the source code archives when necessary, then the entire `DISTDIR` can be cleaned
- If the system is tight on space, and the user would rather not re-download source code of packages that are currently installed (in case a rebuild of software is needed the sources will be present on the system), then only clean out source code archives that are no longer used by any installed software

Of course, other clean-up strategies might exist as well. The above examples are provided as hypothetical situations.

## Resolution

The [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package provides a utility called eclean-dist which supports, among other strategies, the following clean-up activities.

Running eclean-dist will remove the source code archives that do not belong to any available ebuild anymore. This is a safe approach since these sources are very unlikely to be needed again (most of these archives are of old ebuilds that have since been removed from the Gentoo ebuild repository).

`root #``eclean-dist`
The `--deep` option can be added to make eclean-dist remove source code archives that do not belong to an *installed ebuild*. This will remove many more archives while providing a modest safety net, since the archives of installed ebuilds remain available in case rebuilds are needed.

`root #``eclean-dist --deep`
## A more drastic approach to save disk space

An other option is to clean the distfiles after every successful merged ebuild, for that you may set the following hook (or install [app-portage/darkelf-features](http://gpo.zugaina.org/Overlays/darkelf/app-portage/darkelf-features) from [darkelf](https://gitweb.gentoo.org/repo/user/darkelf.git/) overlay):

**`/etc/portage/bashrc`**

**Implementing function to clean distfiles for every merge**

```
# darkelf-features-0.1::darkelf
#
# This enables cleaning the distfiles after every emerge,
# to enable this feature set
# DARKELF_FEATURES="postmerge_distclean"
# in /etc/portage/make.conf or per command basis e.g.:
# DARKELF_FEATURES="postmerge_distclean" emerge ...
darkelf_postmerge_distclean(){
 echo cleaning distfiles for ${CATEGORY}/${PF}...
 if [ -z $DISTDIR ] ; then
  echo ERROR: DISTDIR is empty!
  exit 1
 fi
 for f in $DISTDIR/* ; do
  echo deleting "`readlink -f $f`"...
  rm -r "`readlink -f $f`"
 done
}
post_pkg_postinst(){
 if grep -q "postmerge_distclean" <<< $DARKELF_FEATURES ; then
  darkelf_postmerge_distclean
 fi
}
```
This clean feature can then be enabled by adding the following line to make.conf:

**`/etc/portage/make.conf`**

**Enabling function to clean distfiles for every merge**

```
DARKELF_FEATURES="postmerge_distclean"
```
## A few useful options

There are several useful options, so be sure to look at them. The `--help` option outputs lots of useful information. For example, `--pretend` functions as it does with emerge, showing what will happen without actually doing anything. Another nice option that can be used in conjunction with `--deep` is `--fetch-restricted`. Downloading Java from Oracle is annoying enough the first time; this will keep users from reliving the painful experience of re-downloading source code for their systems... at least until the next release.

## See also

- [Eclean](https://wiki.gentoo.org/wiki/Eclean) — a tool for cleaning repository source files and binary packages.
