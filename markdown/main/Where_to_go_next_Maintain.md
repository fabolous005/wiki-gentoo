<!-- source: https://wiki.gentoo.org/wiki/Where_to_go_next/Maintain | group: Gentoo Wiki (Main) | wiki-title: Where to go next/Maintain -->
---
title: Where to go next/Maintain
url: https://wiki.gentoo.org/wiki/Where_to_go_next/Maintain
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-13"
fingerprint: b5315e4e0ac3ab15
license: CC BY-SA 4.0
---

# Where to go next/Maintain

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

So Gentoo is installed, but what is next? Here is a list of tools to configure the system ranging from desktop to server usage.

## Maintaining Gentoo

Before starting getting deeper into Gentoo, it would be wise get familiar with some of the tools that will be worked with.

### Dealing with common issues

There are a number of error messages that a Gentoo user will be confronted with regularly when maintaining their system, users should familiarize themselves with common errors and how to solve them.

#### Circular dependencies

Circular dependencies arise when two packages depend on each other to be merged. This is usually due to a set of USE flags set in the system's configuration.

The wiki has a dedicated page on dealing with [Portage/Help/Circular\_dependencies](https://wiki.gentoo.org/wiki/Portage/Help/Circular_dependencies)

#### Masked packages

Users may encounter portage telling them that a package is \`masked\` and are therefore unable to install said package. Packages may be masked for a number of reasons, a common example being the keyword of a package.

`root #``emerge --ask media-libs/nvidia-vaapi-driver`
These are the packages that would be merged, in order:
Calculating dependencies... done!
Dependency resolution took 0.70 s (backtrack: 0/20).
!!! All ebuilds that could satisfy "media-libs/nvidia-vaapi-driver" have been masked.
!!! One of the following masked packages is required to complete your request:
- media-libs/nvidia-vaapi-driver-0.0.12::gentoo (masked by: \~amd64 keyword)
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.

A package being masked means that it can't be installed with your current system configuration. The \`masked by\` part is what tells you why. If a package is masked by a keyword, that means that keyword isn't allowed by your configuration. In the preceding example the user is running a stable system with the \`amd64\` keyword accepted globally, but wishes to install a package that does not have an ebuild with a stable keyword available.

A package may be masked for other reasons, such as the license, or being explicitly masked by your profile. Action should be taken in one of the configuration files below, depending on the reason of the mask.

### /etc/portage/package.use

USE flags can look daunting at first, however it is just enabling a switch to turn a feature on and off. The Handbook install process had already given some basic insight on this.

More information and best practices for enabling them can be found at [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use).

### /etc/portage/package.accept\_keywords

Gentoo supports mixing Stable and Testing packages on the user's system effortlessly. This means a user can use a newer package version with a newer feature then the one guaranteed to work. A common example is the Linux kernel, which Gentoo uses the current Long Term Support (LTS) release as it's stable package, however some systems need wireless drivers which are only included in the mainline release.

For more information see [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords).

### /etc/portage/package.license

By default Gentoo only accepts free software licenses, but allows the user to accept more restrictive (in terms of freedom) licenses as either a system-wide or per package need as required,

See [/etc/portage/package.license](https://wiki.gentoo.org/wiki//etc/portage/package.license) on how to set these for any need.

### Administrative tools

Gentoo provides a number of administrative tools that aid in administrative tasks, such as viewing USE flag information, estimated build times, dependencies, which package provided a certain file, rebuilding of reverse dependencies and more. Most users will benefit from having them installed.

#### dispatch-conf

dispatch-conf is configuration file update when a package installs a new config file. It has handy features such as being able to quickly see the changes as a diff and rollback options if a new file is selected by mistake.

See [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) for more.

#### gentoolkit

gentoolkit provides a suite of tools that aid in the administration and maintenance of your Gentoo system.

For more information, see the [Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) wiki page.

##### eclean

Portage by default preserves copies of downloaded files, on local storage: source tarballs in /var/cache/distfiles and binary packages in /var/cache/binhost/gentoo. If an update downloads a newer version of these files, the earlier versions are still preserved.

It's good practice on a Gentoo system to keep the current versions of these source tarballs and binary packages, to be able to recover systems, in case of file corruption for example (after a certain amount of time, there isn't always a guarantee that these files will still be available for download).

Keeping the older, non current, versions of these files however can needlessly consume storage space, adding up over time, and potentially wasting space, if the older versions are not required.

#### q applets

The q applets are meant to provide a more limited, but faster, alternative to their gentoolkit counterpart.

For more information, see the [Q\_applets](https://wiki.gentoo.org/wiki/Q_applets) wiki page.

### Basic emerge usage

#### Installing a package

During the install process, the [app-portage/mirrorselect](https://packages.gentoo.org/packages/app-portage/mirrorselect) was used as an example of how to install a package.

If this wasn't done, lets now emerge it with:

`root #``emerge --ask app-portage/mirrorselect`
This now has installed the package [app-portage/mirrorselect](https://packages.gentoo.org/packages/app-portage/mirrorselect) to the system and Portage has been told that that user wants this package updated every time a new update comes out by adding it to the [World file](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>).

If the user only needs to use this program and never again the switch `--oneshot` can be added. This tell Portage to install the package, but not to update it and remove it on the next `--depclean`.

`root #``emerge --ask --verbose --oneshot app-portage/mirrorselect`
#### Removing a package

If a package is added to the world file then it must be first removed by using `--deselect`

`root #``emerge --deselect app-portage/mirrorselect`
It is now possible to ask Portage to check all packages on the system to see which can be safely removed as they are no longer required by the user and no other package depends on them.

`root #``emerge --ask --verbose --depclean`
#### Updating system

First update [Ebuild](https://wiki.gentoo.org/wiki/Ebuild) repositories.

`root #``emaint --auto sync`
With updated ebuild files actual upgrade can be performed.

`root #``emerge --ask --verbose --update --deep --newuse @world`
For in-depth upgrade guide consult [Upgrading Gentoo](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) page.
