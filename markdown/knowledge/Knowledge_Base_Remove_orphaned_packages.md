<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Remove_orphaned_packages | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Remove_orphaned_packages -->
---
title: Knowledge Base:Remove orphaned packages
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Remove_orphaned_packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-12"
fingerprint: "7331727aa847970d"
license: CC BY-SA 4.0
---

# Knowledge Base:Remove orphaned packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

During regular system maintenance jobs, software packages will be pulled in as dependencies of other packages. When the other packages are removed from the system, these dependencies will remain and become *orphaned packages*. It is recommended to remove orphaned packages so that no unnecessary space is used. This action should be taken for security reasons because orphaned packages are not updated during regular @system or @world update sessions.

## Environment

Any Gentoo Linux environment.

## Analysis

When a user installs software, the software title itself is registered in the user's [world file](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>). During the installation, the necessary dependencies are also pulled in and installed. These packages however are *not* registered in the world file.

When software is removed from the system, its entry in the world file (if applicable) is removed as well. The dependencies of that software however remain on the system since it is possible that the dependencies are also used by other (installed) software titles.

To support clean-up of orphaned packages (software packages that are not in the dependency tree of any installed software title(s)) Gentoo's Portage supports a `--depclean` option. This option will scan the installed software and all dependency trees, identify the orphaned packages, and remove them.

## Resolution

First, make sure that the [system is up to date](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#Updating_packages) and that no open installation jobs are pending:

`root #``emerge --ask --verbose --update --deep --newuse @world`
Then, invoke emerge --ask --depclean to remove orphaned packages (it will ask for confirmation):

`root #``emerge --ask --depclean`
Review changes carefully. If emerge --depclean has not been run in a while, it may try to remove many packages - caution is advised.

### Keep specific packages

If emerge --depclean would remove packages that should not be removed, the `--exclude` option could temporally be used, or they can be added to the world set (preferred), for example:

`root #``emerge --noreplace <atom>`
### Check for "obsolete" packages

To test if any obsolete packages are still found on the system, use the [eix](https://wiki.gentoo.org/wiki/Eix) utility, which comes with the following useful command:

`root #``eix-test-obsolete`
