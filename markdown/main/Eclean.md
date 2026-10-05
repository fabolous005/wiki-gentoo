<!-- source: https://wiki.gentoo.org/wiki/Eclean | group: Gentoo Wiki (Main) | wiki-title: Eclean -->
---
title: eclean
url: https://wiki.gentoo.org/wiki/Eclean
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: "87c5480f80937f0e"
license: CC BY-SA 4.0
---

# eclean

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


eclean is a tool for cleaning repository source files and binary packages. It is part of the [gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) suite of tools.

Also available on Gentoo is the [eclean-kernel](https://wiki.gentoo.org/wiki/Kernel/Removal#Using_eclean-kernel) tool, available separately from the eclean tools installed with gentoolkit, in the [app-admin/eclean-kernel](https://packages.gentoo.org/packages/app-admin/eclean-kernel) package. eclean-kernel is used to automate the clean up of old Linux kernels.

## Installation

### Emerge

Install [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit):

`root #``emerge --ask app-portage/gentoolkit`
## Usage

By default, source files are located in the [/var/cache/distfiles](https://wiki.gentoo.org/wiki//var/cache/distfiles) directory, while binary packages are located in the [/var/cache/binpkgs](https://wiki.gentoo.org/wiki//var/cache/binpkgs) directory. The locations for each can be changed by altering the `[DISTDIR](https://wiki.gentoo.org/wiki/DISTDIR)` and the `PKGDIR` variables respectively in /etc/portage/make.conf. Both locations can grow quite big if not periodically cleaned; this is the reason eclean was created.

### Invocation

Use eclean --help to see full action summary, options list, and usage breakdown:

`user $``eclean --help````
Usage:
 eclean [global-option] ... <action> [action-option] ...
 eclean-dist [global-option, distfiles-option] ...
 eclean-pkg [global-option, packages-option] ...
 eclean(-dist,-pkg) [--help, --version]
 
Available global options:
 -C, --nocolor             - turn off colors on output
 -d, --deep                - only keep the minimum for a reinstallation
 -e, --exclude-file=<path> - path to the exclusion file
 -i, --interactive         - ask confirmation before deletions
 -n, --package-names       - protect all versions (when --deep)
 -p, --pretend             - only display what would be cleaned
 -q, --quiet               - be as quiet as possible
 -t, --time-limit=<time>   - don't delete files modified since <time>
   <time> is a duration: "1y" is "one year", "2w" is "two weeks", etc. 
   Units are: y (years), m (months), w (weeks), d (days) and h (hours).
 -h, --help                - display the help screen
 -V, --version             - display version info
 
Available actions:
 packages     - clean outdated binary packages from PKGDIR
 distfiles    - clean outdated packages sources files from DISTDIR
 
Available options for the packages action:
     --changed-deps               - delete packages for which ebuild dependencies have changed
 -i, --ignore-failure             - ignore failure to locate PKGDIR
 
Available options for the distfiles action:
 -f, --fetch-restricted   - protect fetch-restricted files (when --deep)
 -s, --size-limit=<size>  - don't delete distfiles bigger than <size>
   <size> is a size specification: "10M" is "ten megabytes", "200K" is
   "two hundreds kilobytes", etc.  Units are: G, M, K and B.
 
More detailed instruction can be found in `man eclean`
```
### Cleaning distfiles

Clean the source files directory by passing the `distfiles` argument:

`root #``eclean distfiles`
Or by running the short option:

`root #``eclean-dist`
If `distfiles` takes up too much space, perform a deep clean:

`root #``eclean -d distfiles`
### Cleaning packages

For the directory with the binary packages use the following command instead:

`root #``eclean packages`
Or by running the short option:

`root #``eclean-pkg`
## Options

By default, source files and binary packages corresponding to any ebuild in the current repository will **not** be deleted. This way, system administrators can easily downgrade a package or install a previously removed package, provided the package is still in the current repository tree.

As an example, suppose packages foo-1.0 and foo-1.1 are both in the repository. After updating from foo-1.0 to foo-1.1, run eclean distfiles: source files for both versions will be kept, so if a problem occurs with foo-1.1 then the user can easily re-install foo-1.0 without re-downloading anything.

The other possible case is installing a previously removed package. Suppose that a package foo (any version) is installed on the system. After (inadvertently) removing it and running eclean distfiles, the source files for foo will be kept, so it can be re-installed without re-downloading anything.

The same examples also apply for binary packages.

To save more disk space, add the `--deep` option: every source file or binary package that does not correspond to some *currently installed* package (version does matter) will be deleted. Please notice that this way users will not be protected in case they need to downgrade a package or re-install a previously removed package.

As an alternative, use both the `--deep` and the `--package-names` options: every source file or binary package that does not correspond to some currently installed package (version does not matter) will be deleted. This still will not protect in case a re-install of a previously removed package is needed, but it will protect the sources if the package needs to be downgraded later.

For more details read the eclean(1) man page:

`user $``man 1 eclean`
## Troubleshooting

### Cleaning leaves some distfiles

Problem: when trying to clean distfiles, some distfiles are not removed and the message "**The following unavailable installed packages were found**" is displayed. For example:

`root #``eclean --deep distfiles````
 * Building file list for distfiles cleaning...
 * Your distfiles directory was already clean.
 
   The following unavailable installed packages were found
             sys-kernel/gentoo-sources-4.19.44
             sys-kernel/gentoo-sources-4.19.45
             sys-kernel/gentoo-sources-4.19.46
```
This situation occurs when a package's ebuild has been removed from the Gentoo ebuild repository and the package is currently installed on the system. Generally if the listed packages/distfiles are no longer needed; the reason they are not being removed is because they are listed in [Portage's world file](<https://wiki.gentoo.org/wiki/World_file_(Portage)>).

The solution is to to remove the packages from the world file, or remove the specific package atom(s) via:

`root #``emerge --ask --depclean =sys-kernel/gentoo-sources-4.19.44`
Then re-run eclean again in order to remove the distfiles.

## See also

- [Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) — a suite of tools to ease the administration of a Gentoo system, and [Portage](https://wiki.gentoo.org/wiki/Portage) in particular.
- [Knowledge Base: Remove obsoleted distfiles](https://wiki.gentoo.org/wiki/Knowledge_Base:Remove_obsoleted_distfiles)
- [DISTDIR](https://wiki.gentoo.org/wiki/DISTDIR) — defines the location where Portage will store the downloaded source code archives.
