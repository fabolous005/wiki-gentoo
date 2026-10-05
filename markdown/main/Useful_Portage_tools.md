<!-- source: https://wiki.gentoo.org/wiki/Useful_Portage_tools | group: Gentoo Wiki (Main) | wiki-title: Useful Portage tools -->
---
title: Useful Portage tools
url: https://wiki.gentoo.org/wiki/Useful_Portage_tools
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-02"
categories: ['Portage related tools']
fingerprint: b0115fd680e63fa2
license: CC BY-SA 4.0
---

# Useful Portage tools

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides a list of Gentoo-specific system management tools, notably for [Portage](https://wiki.gentoo.org/wiki/Portage), available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).

## Available software

| Name | Package | Homepage | Description | 
|---|---|---|---|
| command-not-found | [app-portage/command-not-found](https://packages.gentoo.org/packages/app-portage/command-not-found) | [https://github.com/Nowa-Ammerlaan/command-not-found-gentoo](https://github.com/Nowa-Ammerlaan/command-not-found-gentoo) | Suggests packages that provide a missing command. | 
| [eclean-kernel](https://wiki.gentoo.org/wiki/Kernel/Removal#Using_eclean-kernel) | [app-admin/eclean-kernel](https://packages.gentoo.org/packages/app-admin/eclean-kernel) | [https://github.com/projg2/eclean-kernel/](https://github.com/projg2/eclean-kernel/) | Remove old kernel versions, keeping either N newest kernels or only those which are referenced by a bootloader. | 
| [eix](https://wiki.gentoo.org/wiki/Eix) | [app-portage/eix](https://packages.gentoo.org/packages/app-portage/eix) | [https://github.com/vaeth/eix/](https://github.com/vaeth/eix/) | Command line tool for accessing information on installed packages, local settings, and local and external overlays. | 
| [elogv](https://wiki.gentoo.org/wiki/Elogv) | [app-portage/elogv](https://packages.gentoo.org/packages/app-portage/elogv) | [https://gitweb.gentoo.org/proj/elogv.git](https://gitweb.gentoo.org/proj/elogv.git) | Curses based utility to parse the contents of elogs created by Portage. | 
| [emlop](https://wiki.gentoo.org/wiki/Emlop) | [app-portage/emlop](https://packages.gentoo.org/packages/app-portage/emlop) | [https://github.com/vincentdephily/emlop](https://github.com/vincentdephily/emlop) | A fast, accurate, ergonomic emerge.log parser. | 
| [eselect](https://wiki.gentoo.org/wiki/Eselect) | [app-admin/eselect](https://packages.gentoo.org/packages/app-admin/eselect) | [Project:Eselect](https://wiki.gentoo.org/wiki/Project:Eselect) | Tool for administration and configuration on Gentoo systems.. | 
| [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) | [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) | [https://github.com/projg2/eselect-repository](https://github.com/projg2/eselect-repository) | A tool to configure Gentoo overlays. (Relies on emerge to synchronize them.) | 
| euses | [app-portage/euses](https://packages.gentoo.org/packages/app-portage/euses) | [https://rooversj.home.xs4all.nl/gentoo/](https://rooversj.home.xs4all.nl/gentoo/) | Look up USE flag descriptions fast. | 
| [genlop](https://wiki.gentoo.org/wiki/Genlop) | [app-portage/genlop](https://packages.gentoo.org/packages/app-portage/genlop) | [https://github.com/gentoo-perl/genlop](https://github.com/gentoo-perl/genlop) | A nice emerge.log parser. | 
| [gentoopmq](https://wiki.gentoo.org/wiki/Gentoopm) | [app-portage/gentoopm](https://packages.gentoo.org/packages/app-portage/gentoopm) | [https://github.com/projg2/gentoopm](https://github.com/projg2/gentoopm) | A tool that provides basic lookups into the package manager data. | 
| [pfl](https://wiki.gentoo.org/wiki/Pfl) | [app-portage/pfl](https://packages.gentoo.org/packages/app-portage/pfl) | [https://www.portagefilelist.de/](https://www.portagefilelist.de/) | Searchable online file/package database for Gentoo. | 
| [pkgcore](https://wiki.gentoo.org/wiki/Pkgcore) | [sys-apps/pkgcore](https://packages.gentoo.org/packages/sys-apps/pkgcore) | [https://github.com/pkgcore](https://github.com/pkgcore) | Package and repository utilities. | 
| [pybugz](https://wiki.gentoo.org/wiki/Pybugz) | [www-client/pybugz](https://packages.gentoo.org/packages/www-client/pybugz) | [https://github.com/williamh/pybugz](https://github.com/williamh/pybugz) | A command line interface to Gentoo Bugzilla. | 
| [q applets](https://wiki.gentoo.org/wiki/Q_applets) | [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) | [https://gitweb.gentoo.org/proj/portage-utils.git](https://gitweb.gentoo.org/proj/portage-utils.git) | Small and fast Portage helper tools written in C. | 
| smart-live-rebuild | [app-portage/smart-live-rebuild](https://packages.gentoo.org/packages/app-portage/smart-live-rebuild) | [https://github.com/projg2/smart-live-rebuild](https://github.com/projg2/smart-live-rebuild) | Check live packages for updates and emerge them as necessary. | 
| [ufed](https://wiki.gentoo.org/wiki/Ufed) | [app-portage/ufed](https://packages.gentoo.org/packages/app-portage/ufed) | [https://gitweb.gentoo.org/proj/ufed.git](https://gitweb.gentoo.org/proj/ufed.git) | Simple program designed to configure system USE flags. | 

### Gentoolkit

[Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) ([app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit)) contains several useful tools for users:

| Name | Description | 
|---|---|
| ebump | Ebuild revision bumper (more useful for developers). | 
| [eclean](https://wiki.gentoo.org/wiki/Eclean) | Tool for cleaning repository source files and binary packages. | 
| enalyze | Gentoo's installed packages analysis and repair tool. See man page, which states "CAUTION: This is beta software and is not yet feature complete". | 
| [epkginfo](https://wiki.gentoo.org/wiki/Epkginfo) | Wrapper to equery: display metadata about a given package. | 
| [equery](https://wiki.gentoo.org/wiki/Equery) | Gentoo package query tool. | 
| [eread](https://wiki.gentoo.org/wiki/Gentoolkit#eread) | Script to read portage log items from einfo, ewarn etc. | 
| eshowkw | Display keywords for specified package(s). | 
| [euse](https://wiki.gentoo.org/wiki/Euse) | Tool to see, set and unset USE flags at various places. | 
| imlate | Displays candidates for keywords for an architecture (more useful for developers?). | 
| revdep-rebuild | Reverse Dependency rebuilder. Generally not necessary to run this tool anymore. |
