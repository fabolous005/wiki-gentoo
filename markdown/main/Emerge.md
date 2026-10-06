<!-- source: https://wiki.gentoo.org/wiki/Emerge | group: Gentoo Wiki (Main) | wiki-title: Emerge -->
---
title: emerge
url: https://wiki.gentoo.org/wiki/Emerge
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-23"
fingerprint: a6855a6e8ac7990f
license: CC BY-SA 4.0
---

# emerge

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**emerge** is  the main command-line interface to [Portage](https://wiki.gentoo.org/wiki/Portage), the Gentoo package manager.

emerge is used to download, install, update, and maintain software packages on Gentoo Linux.

emerge is a very powerful and flexible command that can, among other things, [automatically build and install packages "from source"](https://wiki.gentoo.org/wiki/Ebuild), fetch and install "ready-to-use" [binary packages](https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart), create binary packages, search for packages, report system information, etc.

## Usage

### Invocation

The emerge command should be followed by appropriate options, actions, and package or set of packages. If emerge is invoked without any parameters or package, it will print a help text and exit.

For most uses emerge will need to be executed with [superuser privileges](https://wiki.gentoo.org/wiki/Sudo), though when used simply to report information it may be possible to run it as an unprivileged user.

If emerge is invoked with a package and no other options, it will **immediately** attempt to install the corresponding package **without requesting confirmation from the user**. This is often not the desired behavior, so one of the following options will probably be required.

The `--ask` (`-a`) and `--pretend` (`-p`) options allow examination of the planned system changes before they are actually made. The `--ask` option will make emerge display the intended changes and ask for confirmation before continuing. The `--pretend` option will simply display the intended changes and halt, and does not require superuser privileges.

emerge provides rich output, with information and warnings about individual packages and the system as a whole. The `--verbose` option is useful to have Portage show even more information, such as what [USE flags](https://wiki.gentoo.org/wiki/USE_flag) will be used to install or update a package, what USE flags are available for each package, the size of the package download, [overlay](https://wiki.gentoo.org/wiki/Ebuild_repository) name.

Running emerge with the `--help` option provides information on command line options:

`user $``emerge --help````
emerge: command-line interface to the Portage system
Usage:
   emerge [ options ] [ action ] [ ebuild | tbz2 | file | @set | atom ] [ ... ]
   emerge [ options ] [ action ] < @system | @world >
   emerge < --sync | --metadata | --info >
   emerge --resume [ --pretend | --ask | --skipfirst ]
   emerge --help
Options: -[abBcCdDefgGhjkKlnNoOpPqrsStuUvVwW]
          [ --color < y | n >            ] [ --columns    ]
          [ --complete-graph             ] [ --deep       ]
          [ --jobs JOBS ] [ --keep-going ] [ --load-average LOAD            ]
          [ --newrepo   ] [ --newuse     ] [ --noconfmem  ] [ --nospinner   ]
          [ --oneshot   ] [ --onlydeps   ] [ --quiet-build [ y | n ]        ]
          [ --reinstall changed-use      ] [ --with-bdeps < y | n >         ]
Actions:  [ --depclean | --list-sets | --search | --sync | --version        ]
 
 
For more help consult the man page.
```
Below is an example invocation of emerge, installing "package". The options `-atv` are short options for `--ask` (see above), `--tree` (display the dependency tree of packages to be installed), and `--verbose` (see above). Hover the mouse cursor over the red dotted boxes to see an explanation of each section of output:

These are the packages that would be merged, in reverse order:

Calculating dependencies... done!
\[ebuild     **U**  \] **category/package-3.0-r2::gentoo \[2.0::gentoo\]** USE="**enabled -disabled toggled\* new% (-unavailable)**" MAKE\_OPTIONS="**-disabled**" 777 kB
\[ebuild     **UD** \]  category/package-2.0::gentoo **\[3.0::gentoo\]** 777 kB
\[ebuild   **R**    \]   category/package-1.0::gentoo  777 kB
\[ebuild  **N**     \]  category/package-0.5::some-overlay-name  777 kB

Total: 4 packages (1 new, 1 reinstall, 1 upgrade, 1 downgrade), Size of downloads: 3108 kB

**Would you like to merge these packages?**\[Yes/No\]

The *U* symbol shows a package that will be upgraded, *D* a package that will be downgraded, *R* re-emerged, *N* a new package. In square brackets is the version of the previously installed package. Packages present in the world file are shown in bold - these are the user-installed packages, the other packages will be dependencies, or from the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>).

In the context of Portage, the term "package" can have a similar meaning to "atom", see [version specifier](https://wiki.gentoo.org/wiki/Version_specifier).

### Install a package

Packages are installed ("emerged") using the emerge command followed by a [version specifier](https://wiki.gentoo.org/wiki/Version_specifier) that indicates which package to install (and optionally a specific version, slot, and from which [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository)). emerge is executed with [root privileges](https://wiki.gentoo.org/wiki/Sudo).

Package functionality is governed by [USE flags](https://wiki.gentoo.org/wiki/USE_flag) which can be set or unset depending on the intended use of a piece of software by editing their configuration in [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use).

As an example, install the [net-proxy/tinyproxy](https://packages.gentoo.org/packages/net-proxy/tinyproxy) package with `--ask` and `--verbose` options:

`root #``emerge --ask --verbose net-proxy/tinyproxy`
### Search for packages

Search for packages with *proxy* in their names:

`user $``emerge --search proxy`
Search for packages with *proxy* in their names or description:

`user $``emerge --searchdesc proxy`
Search packages using a regular expression:

`user $``emerge -s '%^python$'`
List all packages in a category:

`user $``emerge -s '@^net-ftp/'`
### Remove (uninstall / depclean) packages

To uninstall a package in Gentoo is colloquially said to **depclean** them. The `--depclean` (`-c`) option will remove the specified packages.

The depclean option will not remove any packages that are currently dependencies of other installed packages, of the [@system](https://wiki.gentoo.org/wiki/Package_sets#.40system), or [@profile](https://wiki.gentoo.org/wiki/Package_sets#.40profile) sets.

Here is an example of removing the [net-proxy/tinyproxy](https://packages.gentoo.org/packages/net-proxy/tinyproxy) package:

`root #``emerge --ask --verbose --depclean net-proxy/tinyproxy`
An alternative to using `--depclean` to uninstall packages, is to use emerge --deselect (or `-W` option), then cleaning out orphaned packages, as described in the following section.

#### Cleaning out orphaned packages

### Update packages

See  [Upgrading Gentoo](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) for instructions on how to update packages.

### Get system information

emerge can print system information that can be useful for troubleshooting. This information is often required to be posted when asking for [support](https://wiki.gentoo.org/wiki/Support), or when [filing a bug](https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide).

`user $``emerge --info`
Extra information may be output by using the `--verbose` flag.

## Tips

### Verifying and (re)downloading distfiles

To re-verify the integrity of and re-download previously removed/corrupted distfiles for all currently installed packages, run:

`root #``emerge --ask --fetchonly --emptytree @world`
### Do not add dependencies to the world file

If a dependency must be reinstalled, use the `--oneshot` (`-1`) option. Installing dependencies with the emerge package command would add them to the [world file](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) and may lead to issues.

Installing dependencies with Portage for compiling custom source software is also ill advised: it is preferable to [write an ebuild](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds).

### Resume emerge

If an emerge of several packages is interrupted (e.g. ctrl+c, crash...), the emerge may be resumed from the failed package with the `--resume` option. The `--keep-going` and `--skipfirst` options may also be of interest. See the emerge man page for details.

### Temporary Portage configuration through environment variables defined for current invocation

The emerge command can be passed temporary configuration values by declaring environment variables on the command line, in order to affect behavior for that invocation alone. For example, to merge [app-editors/emacs](https://packages.gentoo.org/packages/app-editors/emacs) with the [svg](https://packages.gentoo.org/useflags/svg) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) enabled, but not make this USE flag setting permanent:

`root #``USE='svg' emerge app-editors/emacs`
Or to pass extra configuration options to packages that use the `econf` function in their ebuild:

`root #``EXTRA_ECONF='--without-compress-install' emerge app-editors/emacs`
### re-emerging a package that provided a specific File

Sometimes it is useful to be able to re-emerge a package simply by specifying a particular file that was provided by that package.

As an example, if the user wants to reinstall /usr/lib/libunwind.a but it is not known which package provided this file, the package from where that file came can be determined by emerge by simply indicating the file path:

`user $``emerge -p /usr/lib/libunwind.a`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
Dependency resolution took 2.76 s (backtrack: 0/20).
 
\[ebuild   R    \] sys-libs/llvm-libunwind-17.0.6

Only files that have been provided by a currently-installed package may be re-emerged in this way. See [Pfl](https://wiki.gentoo.org/wiki/Pfl) for other ways to find what packages files might "belong" to.

## Troubleshooting

### Emerging packages fail during 'unpack' stage

The following message can occur when emerging packages:

\* Error messages for package dev-libs/libinput-1.16.0:
 \* The ebuild phase 'unpack' has exited unexpectedly. This type of behavior
 \* is known to be triggered by things such as failed variable assignments
 \* (bug #190128) or bad substitution errors (bug #200313). Normally, before
 \* exiting, bash should have displayed an error message above. If bash did
 \* not produce an error message above, it's possible that the ebuild has
 \* called \`exit\` when it should have called \`die\` instead. This behavior
 \* may also be triggered by a corrupt bash binary or a hardware problem
 \* such as memory or cpu malfunction. If the problem is not reproducible or
 \* it appears to occur randomly, then it is likely to be triggered by a
 \* hardware problem. If you suspect a hardware problem then you should try
 \* some basic hardware diagnostics such as memtest. Please do not report
 \* this as a bug unless it is consistently reproducible and you are sure
 \* that your bash binary and hardware are functioning properly.

Although this issue may be due to the reasons listed in the output above, it can often be caused by low disk space in the path used by Portage to unpack the ebuild's source files. This location is set via the `PORTAGE_TMPDIR` variable and can be quickly found by querying Portage:

`user $``portageq envvar PORTAGE_TMPDIR`
/var/tmp

The [df](https://wiki.gentoo.org/wiki/GNU_Coreutils#df) command may be used to view available disk space for the partition where `PORTAGE_TMPDIR` has been mounted (this will likely be the root (/) partition). See [Freeing disk space](https://wiki.gentoo.org/wiki/Knowledge_Base:Freeing_disk_space) for details on how to free up disk space.

## See also

- [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) — a utility included with [Portage](https://wiki.gentoo.org/wiki/Portage), used to safely and conveniently manage configuration files after package updates.
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.
