<!-- source: https://wiki.gentoo.org/wiki/Package_sets | group: Gentoo Wiki (Main) | wiki-title: Package sets -->
---
title: Package sets
url: https://wiki.gentoo.org/wiki/Package_sets
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-08"
fingerprint: "2b39fb2e83539700"
license: CC BY-SA 4.0
---

# Package sets

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Package sets** are lists of packages used by [Portage](https://wiki.gentoo.org/wiki/Portage). They define the packages that make up the base system and those that are installed based on system administrator action. This article describes package sets in high detail and includes a list of all typically available sets on a Gentoo system.

## Configuration

### Static sets

All Gentoo systems have six static sets, defined in the /usr/share/portage/config/sets/portage.conf file maintained by Portage, which specify the packages that make up the system. Three sets include "**@selected-**" as a prefix in their name and refer to the member sets and their packages which the system administrator has *selected* to be installed.

The sets titled **@system** and **@profile** define a static list of packages which are required for the base system to operate as intended. These two sets are defined by the Gentoo development team.

The *@world* set is a superset consisting of the sum of the members of the previously mentioned sets.

#### @selected-packages

The **selected-packages** set is the list of packages found in the [world file](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) (/var/lib/portage/world). It is a static list of packages that have been explicitly installed by the system administrator. Adding a package to this set can be avoided by passing the `--oneshot` option to emerge.

#### @selected-sets

The **@selected-sets** set is a [superset](https://en.wikipedia.org/wiki/superset): a union of any and all custom sets emerged by the system administrator. It includes a list of every package that been explicitly installed onto the system by a result of inclusion of an installed set (except if `--oneshot` was used - see below). The members of this set are listed in the /var/lib/portage/world\_sets file. The file will be empty if the system administrator has not (yet) emerged any custom sets.

If the `--oneshot` option was passed when installing a custom set then the packages included in the set will not be listed, nor will the custom set be recorded in the world\_sets file.

#### @selected

The **@selected** set is a [superset](https://en.wikipedia.org/wiki/superset): a union of **@selected-packages** and **@selected-sets**. That is to say, the output of this set includes all packages intentionally installed by the system administrator.

Take careful note that packages which are listed in the **@selected** set may or may not include packages which are *also* defined in the *@system* or *@profile* sets by Gentoo developers. It is unwise to blindly uninstall packages from this set. Portage will provide a warning when attemping to unmerge a package which is defined in the *@system* or *@profile* set.

#### @system

The **@system** set (along with the following **@profile** set) defines the base set of packages on a Gentoo system. The **@system** set is maintained by the Gentoo development team.

As of 2026-03-04, the **@system** set contains:

#### @profile

The **profile** set (along with **@system** defined in the preceding section) define the base set of packages on a Gentoo system. The differences between the two is described in the man pages of Portage but the distinction does not usually matter to the system administrator.  The packages that make up this set are determined by the system's profile and/or computer's architecture.

#### @world

The **@world** set is the largest [superset](https://en.wikipedia.org/wiki/superset) on a Gentoo system: a union of **@selected**, **@system**, and **@profile** sets. That is to say the **@world** set includes *all* packages installed on the system: those defined by the system administrator and those defined by the Gentoo development team.

### Dynamic sets

Portage maintains additional dynamic sets that are based on the state of the package database or the type of packages installed on the system. Unlike the previous sets, these dynamically maintained sets cannot be used to define packages that should be installed. These sets are listed in the /usr/share/portage/config/sets/portage.conf file and are determined and maintained by Portage. They are therefore not intended to be modified by the system administrator.

#### @changed-deps

**changed-deps** is the set of packages that have [changed dependencies](https://wiki.gentoo.org/wiki/Project:Portage/Changed_dependencies).

#### @deprecated-live-rebuild

**deprecated-live-rebuild** is the older way to get the set of live packages.

#### @downgrade

**downgrade** is the set of packages that have an installed version that is higher than the highest visible ebuild
version.

#### @installed

**installed** is the set of all installed packages.

#### @live-rebuild

**live-rebuild** is the set of live (category/package-9999) packages.

#### @module-rebuild

**module-rebuild** is the set of out of kernel modules, packages that own files in /lib/modules.

#### @preserved-rebuild

**preserved-rebuild** is the set of packages that are using [preserved libraries](https://wiki.gentoo.org/wiki/Preserve-libs).

#### @rebuilt-binaries

**rebuilt-binaries** is the set of binary package that can be replaced with packages that have been rebuilt.

#### @security

**security** is the set of packages for which there is a new [GLSA](https://wiki.gentoo.org/wiki/GLSA).

**unavailable** is the set of installed packages that do not have an ebuild.

**unavailable-binaries** is the set of installed packages that do not have a binary package.

#### @x11-module-rebuild

**x11-module-rebuild** is the set of packages that own files inside /usr/lib/xorg/modules with
the exception of the package that owns /usr/bin/Xorg.

### Custom sets

There are two types of custom sets:

- static, as defined in [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets)
- dynamic, as defined in /etc/portage/sets.conf

#### Example: @larrys-custom-set

**larrys-custom-set** is an example of a custom set as defined by the system administrator. Custom sets are defined as files in the [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets) directory can be easily installed or rebuilt as a group by the package manager. Members defined within a custom set in this way are static and will only change via direct modification by the system administrator.

Once a list of packages have been defined, install a custom set via:

`root #``emerge --ask @larrys-custom-set`
In addition to emerging the packages, the set itself will be added to **@selected-sets** and therefore the individual packages within the set also be part of **@selected**. This is fine if the packages in Larry's set are all admin defined packages. However if some of the packages in Larry's set are only dependencies, then problems are likely to occur over time because the package will continue to be updated even if it is no longer required. This is equivalent to having a package's *dependencies* in the world file.

To emerge a custom set without adding the package(s) to the **@selected-sets** run:

`root #``emerge --ask --oneshot @larrys-custom-set`
To stop using a custom set, deselect it:

`root #``emerge --deselect @larrys-custom-set`
After deselection, issue a dependency clean. This will safely remove packages that are no longer on the system:

`root #``emerge --ask --depclean`
## Usage

### Installing a set

Sets can be passed as an argument to the emerge command:

`user $``emerge -p @world`
### Listing

#### Available sets

The list of available sets can be found by:

`user $``emerge --list-sets`
changed-deps
deprecated-live-rebuild
downgrade
installed
larrys-defined-set
live-rebuild
module-rebuild
preserved-rebuild
profile
rebuilt-binaries
security
selected
selected-packages
selected-sets
system
unavailable
unavailable-binaries
world
x11-module-rebuild

#### Packages within a set

Various tools can be used to list packages included within the system set. With the exceptions of eix and pquery, the tools below preface the set name with an `@` character.

##### Portage

The emerge command can be used to list packages included in sets using the following options:

`user $``emerge -pqeO @<set>`
Or using long options:

`user $``emerge --pretend --quiet --emptytree --nodeps @<set>`
##### pkgcore

Using pquery (from [sys-apps/pkgcore](https://packages.gentoo.org/packages/sys-apps/pkgcore)):

`user $``pquery --pkgset <set>`
##### eix

To receive a compact listing which emulates the listing output from other commands via [eix](https://wiki.gentoo.org/wiki/Eix) (from [app-portage/eix](https://packages.gentoo.org/packages/app-portage/eix)), issue:

`user $``eix --compact --<set>`
For a non-compact listing, issue:

`user $``eix --<set>`
Where `<set>` is one of `system` (@system), `world` (@world), `selected` (@selected), etc...
See man eix or the [eix article for more usage examples](https://wiki.gentoo.org/wiki/Eix#usage).

##### equery

Using equery (from [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit)):

`user $``equery list @<set>`
#### Packages that belong to a set

It is possible to search for specific packages that are defined within one or many custom sets with the following command, replace `<package>` with the package name as appropriate:

`user $``grep -ir <package> /etc/portage`
## See also

- [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets) — an optional directory that is used to create user defined package sets
- [Selected-packages set (Portage)](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) — contains the user-selected "world" packages that are listed in the /var/lib/portage/world file.
