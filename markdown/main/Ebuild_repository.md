<!-- source: https://wiki.gentoo.org/wiki/Ebuild_repository | group: Gentoo Wiki (Main) | wiki-title: Ebuild repository -->
---
title: ebuild repository
url: https://wiki.gentoo.org/wiki/Ebuild_repository
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-02"
fingerprint: cf813c0e9ce02be8
license: CC BY-SA 4.0
---

# ebuild repository

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

An **ebuild repository** is a file-structure  that can provide packages for installation on a Gentoo system. Ebuild repositories contain [ebuilds](https://wiki.gentoo.org/wiki/Ebuild), [eclasses](https://wiki.gentoo.org/wiki/Eclass), and other types of descriptive metadata files that supply [Portage](https://wiki.gentoo.org/wiki/Portage) with packages,  [news items](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items), [profile targets](https://wiki.gentoo.org/wiki/Portage/Profiles), etc.

The **[Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)** is <u>Gentoo Linux's primary and official ebuild repository</u> - it contains all the information needed to build and install every package that makes up Gentoo. Additional ebuild repositories, such as [GURU](https://wiki.gentoo.org/wiki/GURU), can be configured with Portage, to provide even more packages.

Portage will install the latest available version of a package from any configured ebuild repository, by default. If the latest available version is provided by several ebuild repositories, it will be chosen according to a set order of priority - hence the colloquial name **overlay**.

Administrators of Gentoo systems can configure additional ebuild repositories with Portage by using the utilities and methods described below.

## The Gentoo ebuild repository

The ***Gentoo ebuild repository*** is the main ebuild repository for a Gentoo Linux system, and is where all the packages come from by default. It is maintained on the [gitweb.gentoo.org server](https://gitweb.gentoo.org/repo/gentoo.git/tree), and gets [synchronized](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization) to local machines (in [/var/db/repos/gentoo](https://wiki.gentoo.org/wiki//var/db/repos/gentoo)), to be available to [Portage](https://wiki.gentoo.org/wiki/Portage).

The Gentoo ebuild repository contains *[ebuild](https://wiki.gentoo.org/wiki/Ebuild)* files that tell Portage how to build and install each package. The ebuilds come with metadata, dependency information, and everything else needed to get a package in working order.

The [metadata](https://wiki.gentoo.org/wiki/Repository_format/package/metadata.xml) provides the package's name, version, where to get sources from, available [USE flags](https://wiki.gentoo.org/wiki/USE_flag), [license](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Licenses), website etc. Dependency information in ebuilds allows Portage to pull in any other packages required to build and run a package that is to be installed - no more, no less. Dependencies are very granular in Gentoo, they will even vary depending on what USE flags are selected, for ultimate selectivity. Perhaps most importantly, *ebuilds* contain the information required to [configure](https://wiki.gentoo.org/wiki/Autotools), [build](https://wiki.gentoo.org/wiki/Build_automation) (compile), [install](https://wiki.gentoo.org/wiki/Emerge), and [test](https://wiki.gentoo.org/wiki/Package_testing) each package - usually from a project's own source code.

In addition to ebuilds, the Gentoo ebuild repository contains the official *[profiles](https://wiki.gentoo.org/wiki/Portage/Profiles)*, which define the default state of [USE flags](https://wiki.gentoo.org/wiki/USE_flag), default values for most variables found in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf), the [set of system packages](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), etc.

The *Gentoo ebuild repository* is also the place where *[news items](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items)* are posted, which is why any new news items will be highlighted after a Gentoo ebuild repository [synchronization](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization).

The *Gentoo  ebuild repository*, and its ebuilds, are maintained by the [Gentoo developers](https://devmanual.gentoo.org/general-concepts/package-maintainers/index.html) and other [members of the community](https://wiki.gentoo.org/wiki/Project:Proxy_Maintainers).

## Where do ebuild repositories come from?

Because an ebuild repository is simply a structure of files and directories, a new ebuild repository can be made available to Portage simply by copying those files and directories to a location known to Portage. The ebuild repositories and their files are usually under /var/db/repos/, but the location of repositories configured for Portage is specified in [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf). Ebuild repositories can be configured on any accessible filesystem however, even on an [nfs](https://wiki.gentoo.org/wiki/Nfs-utils) or [SSHFS](https://wiki.gentoo.org/wiki/SSHFS) filesystem - allowing them to be stored on a network or Internet server.

As previously discussed, the Gentoo ebuild repository is hosted on [gitweb.gentoo.org](https://gitweb.gentoo.org/repo/gentoo.git/tree/). That server also hosts [other ebuild repositories](https://gitweb.gentoo.org/repo/).

In practice, any additional ebuild repositories usually aren't just copied to a directory by hand and configured for Portage (meaning added to [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf)). Generally, new repositories are made available by third parties, and once configured for Portage, are **[synchronized](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization)** by Portage. Synchronization mirrors all the files from a remote location to a locally available filesystem, as configured.

Because ebuild repositories are just file-structures, many methods can be used to synchronize them, and Portage offers several possibilities. [Rsync](https://wiki.gentoo.org/wiki/Rsync) is the default synchronization method, [git](https://wiki.gentoo.org/wiki/Git) is also popular. The synchronization method is specified in [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) when configuring a repository, along with the information needed to retrieve it.

## Repository management

Use the [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) tool to easily add, disable, or remove ebuild repositories configured with Portage. This tool also provides a handy way to list and add repositories available through being [registered on repos.gentoo.org](https://repos.gentoo.org/).

Ebuild repositories can always be configured manually, by editing [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf).

New ebuild repositories for use with Portage can also be [created by the user](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository).

The list of active ebuild repositories can be obtained through the output of one of the following commands:

`user $``emerge --info``user $``portageq repos_config /`
## Installing packages from other repositories

Packages from repositories other than the Gentoo ebuild repository can be installed with the emerge command, just as usual.

For example, once the [GURU](https://wiki.gentoo.org/wiki/GURU) repository is added, to install the *x11-misc/xbanish* package from that repository:

`root #``emerge --ask x11-misc/xbanish`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
Dependency resolution took 2.96 s.
 
\[ebuild   R   #\] x11-misc/xbanish-1.7::guru  0 KiB
 
Total: 1 package (1 reinstall), Size of downloads: 0 KiB

Note that the repository is not specified in the command. The "::guru" appended to the package atom in the output shows what repository the package will be installed from. This works because the *x11-misc/xbanish* package is present in the GURU repository, but *not* in the Gentoo repository.

If multiple versions of the same package are available from two or more different ebuild repositories, Portage will install the most recent version.

If the latest version of a package is available from more than one ebuild repository, the repository with the highest priority will be used. The priority can be set for an ebuild repository in  [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf). The Gentoo ebuild repository has a default priority set to `-1000`, and the default if a priority is not set for a repository is `0`. If several ebuild repositories have the same priority (such as two or more not having any priority set, so having priority `0`), the order is **undetermined** - a package to install will be selected arbitrarily.

It is possible to instruct [Portage](https://wiki.gentoo.org/wiki/Portage) to install a package from a specific ebuild repository with the `::` [version specifier](https://wiki.gentoo.org/wiki/Version_specifier#By_ebuild_repository) (can be used for different emerge instructions, e.g. uninstalling a package through `--depclean`):

`root #``emerge --ask category/atom::repository-name`
See the [repository management](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_management) section to see how to list repositories configured for portage with their respective priorities.

## Repository synchronization

Ebuild repositories should be **synchronized**, so that the local mirrors will reflect a recent state of the repositories. This is necessary to be able to [keep the system up to date](https://wiki.gentoo.org/wiki/Upgrading_Gentoo), and [install](https://wiki.gentoo.org/wiki/Portage#emerge) current software.

Repository synchronization is performed with the emaint sync command, and is configured through the files in [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf):

`user $``emaint --help````
usage: usage: emaint [options] COMMAND
 
The emaint program provides an interface to system health checks
and maintenance. See the emaint(1) man page for additional
information about the following commands:
 
Commands:
  all            Perform all supported commands
  binhost        Scan and generate metadata indexes for binary packages.
  cleanconfmem   Check and clean the config tracker list for uninstalled packages.
  cleanresume    Discard emerge --resume merge lists
  logs           Check and clean old logs in the PORTAGE_LOGDIR.
  merges         Scan for failed merges and fix them.
  movebin        Perform package move updates for binary packages
  moveinst       Perform package move updates for installed and binary packages.
  sync           Check repos.conf settings and sync repositories.
  world          Check and fix problems in the world file.
 
optional arguments:
  -h, --help            show this help message and exit
  -c, --check           Check for problems (a default option for most modules)
  -f, --fix             Attempt to fix problems (a default option for most modules)
  --version             show program's version number and exit
  -C, --clean           Cleans out logs more than 7 days old (cleanlogs only) module-options: -t, -p
  -t NUM, --time NUM    (cleanlogs only): -t, --time Delete logs older than NUM of days
  -p, --pretend         (cleanlogs only): -p, --pretend Output logs that would be deleted
  -P, --purge           Removes the list of previously failed merges. WARNING: Only use this option if you plan on manually fixing them or do not want them re-installed.
  -y, --yes             (merges submodule only): Do not prompt for emerge invocations
  -r REPO, --repo REPO  (sync module only): -r, --repo Sync the specified repo
  -A, --allrepos        (sync module only): -A, --allrepos Sync all repos that have a sync-url defined
  -a, --auto            (sync module only): -a, --auto Sync auto-sync enabled repos only
  --sync-submodule {glsa,news,profiles}
                        (sync module only): Restrict sync to the specified submodule(s)
```
To sync all repositories for which `auto-sync=true` is set, run emaint sync with the `--auto` switch (`-a` for short). This is usually the **command that should be run regularly**, before system updates and package installation (and is equivalent to using the old emerge --sync command):

`root #``emaint sync --auto`
To sync the *foo* repository (irrespective of the *foo* auto-sync setting):

`root #``emaint sync --repo foo`
To sync all repositories with a valid sync-type and sync-url defined (ignoring auto-sync settings):

`root #``emaint sync --allrepos`
See man emaint for information on how to use the portage synchronization commands. See the [Portage project sync](https://wiki.gentoo.org/wiki/Project:Portage/Sync) article about migrating to the new modular sync system from Portage version 2.2.16, it contains important information, notably for users of eix-sync, esync -l, and emerge --sync .

## Best practices

### Cache generation

When large ebuild repositories are installed, Portage may take a long time to perform operations like dependency resolution. This is because ebuild repositories do not usually contain a metadata cache.

There are several available methods to generate such a cache. In order of speed:

1. [Pkgcraft](https://wiki.gentoo.org/wiki/Pkgcraft)'s pk repo metadata from [sys-apps/pkgcraft-tools](https://packages.gentoo.org/packages/sys-apps/pkgcraft-tools)
2. [Pkgcore](https://wiki.gentoo.org/wiki/Pkgcore)'s pmaint regen from [sys-apps/pkgcore](https://packages.gentoo.org/packages/sys-apps/pkgcore)
3. [Portage](https://wiki.gentoo.org/wiki/Portage)'s egencache or emerge --regen from [sys-apps/portage](https://packages.gentoo.org/packages/sys-apps/portage)

To do this using, for example, emerge --regen after syncing the ebuild repositories:

`root #````
emaint sync --allrepos
```
`root #``( ulimit -n 4096 && emerge --regen )`
There is no need to (re)generate the cache if syncing ::gentoo via rsync as it is already there (most users of ::gentoo are rsync users). Ditto if using the 'metadata' or 'sync' repo for ::gentoo with git. Regeneration via any method is time-consuming. Using raw repositories with git however will require generation.

### Masking enabled ebuild repositories

When using *large* ebuild repositories or those with *unknown/low quality code*, it is best practice to hard mask the whole ebuild repository and only accept specific ebuilds on a case-by-case basis. For example, for an overlay named "repository-foobar":

**`/etc/portage/package.mask`**

**Mask all packages in an ebuild repository**

```
*/*::repository-foobar
```
Then add the specific package(s) from the repository-foobar overlay so that they will be available visible to Portage for installation:

**`/etc/portage/package.unmask`**

**Unmask a specific package in an ebuild repository**

```
foo-category/bar::repository-foobar
```
After the above unmask, the package named "foo-category/bar" should be available and none of the other packages from the repository-foobar overlay will be available.

## See also

- [Creating an ebuild repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) — basics of creating an ebuild repository and maintaining ebuilds in it.
- [Creating a custom ebuild repository](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/CustomTree#Creating_a_custom_ebuild_repository) - Section in the Gentoo Handbook
- [Overlays guide (Overlay project)](https://wiki.gentoo.org/wiki/Project:Overlays/Overlays_guide) - A user guide written by the Overlay project.
- [Overlays project](https://wiki.gentoo.org/wiki/Project:Overlays) - The official Gentoo project for ebuild repositories' support.

## External resources

- [https://repos.gentoo.org](https://repos.gentoo.org) - Gentoo's official ebuild repository hosting location.
- [https://github.com/gentoo/](https://github.com/gentoo/) - GitHub mirror of the Gentoo's ebuild repository.
- [https://gpo.zugaina.org/Overlays](https://gpo.zugaina.org/Overlays) - A non-official very useful site for searching overlays.
