<!-- source: https://wiki.gentoo.org/wiki/Portage | group: Gentoo Wiki (Main) | wiki-title: Portage -->
---
title: Portage
url: https://wiki.gentoo.org/wiki/Portage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-01"
tags: ['https://gitweb.gentoo.org/proj/portage.git/tag/?h=portage-2.3.66']
fingerprint: b4317e2e8ea393b2
license: CC BY-SA 4.0
---

# Portage

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Portage** is the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo. It functions as the heart of Gentoo-based operating systems, providing advanced dependency resolution, flexible building and installation of software from source or from [binary packages](https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart), and most other core distribution functionality.

Portage will provision software from the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), any [additional ebuild repositories](https://wiki.gentoo.org/wiki/Portage#Ebuild_repositories), or binhost. Portage includes many [commands](https://wiki.gentoo.org/wiki/Portage#Usage) for repository and package management, the primary of which is the [emerge](https://wiki.gentoo.org/wiki/Portage#emerge) command.

Some common questions about portage and the emerge command are answered in the [FAQ](https://wiki.gentoo.org/wiki/FAQ) and the [Portage FAQ](https://wiki.gentoo.org/wiki/Project:Portage/FAQ).

This article describes Portage from a user's perspective. Those looking to contribute to Portage development should visit the [Portage project page](https://wiki.gentoo.org/wiki/Project:Portage).

All Gentoo installations come with Portage, so ***there is no need to install it!***.

In the rare eventuality of a corrupt or missing Portage, see the [Corrupt or absent Portage](https://wiki.gentoo.org/wiki/Portage#Corrupt_or_absent_Portage) section.


| [+ipc](https://packages.gentoo.org/useflags/+ipc) | Use inter-process communication between portage and running ebuilds. | 
| [+native-extensions](https://packages.gentoo.org/useflags/+native-extensions) | Compiles native "C" extensions (speedups, instead of using python backup code). Currently includes libc-locales. This should only be temporarily disabled for some bootstrapping operations. Cross-compilation is not supported. | 
| [+rsync-verify](https://packages.gentoo.org/useflags/+rsync-verify) | Enable full-tree cryptographic verification of Gentoo repository git or rsync checkouts using app-portage/gemato. | 
| [apidoc](https://packages.gentoo.org/useflags/apidoc) | Build html API documentation with sphinx-apidoc. | 
| [build](https://packages.gentoo.org/useflags/build) | !!internal use only!! DO NOT SET THIS FLAG YOURSELF!, used for creating build images and the first half of bootstrapping \[make stage1\] | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gentoo-dev](https://packages.gentoo.org/useflags/gentoo-dev) | Enable features required for Gentoo ebuild development. | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [xattr](https://packages.gentoo.org/useflags/xattr) | Preserve extended attributes (filesystem-stored metadata) when installing files. Usually only required for hardened systems. | 

In order for Gentoo to stay up to date, Portage must stay up to date. Generally the usual, regular [updating of Gentoo](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#Updating_packages), will automatically update Portage without issue.

On occasion, updates to Portage can make it advisable to update Portage before the rest of the system. After [synchronizing Portage](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization), a message requesting this may be displayed:

\* An update to portage is available. It is \_highly\_ recommended
\* that you update portage now, before any other packages are updated.
\* To update portage, run 'emerge --oneshot sys-apps/portage' now.

Emerge Portage as advised (adapt the command if the message differs from this example). **The `--oneshot` option is important**, to avoid adding [sys-apps/portage](https://packages.gentoo.org/packages/sys-apps/portage) to the [world file](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>):

`root #``emerge --ask --oneshot sys-apps/portage`
If there is an issue with updating Portage, [User:Sam/Portage\_help/Upgrading\_Portage](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Upgrading_Portage) may help.

The main Portage configuration is in [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf), though there are many files used to configure Portage, mainly in the [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage) directory.

See man make.conf for comprehensive documentation, notably a list of variables that can be set in this file.

The [/usr/share/portage/config/make.globals](https://gitweb.gentoo.org/proj/portage.git/tree/cnf/make.globals) file contains many default configuration values sourced by Portage. These values can be overwritten by specifying the same variable names in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf).

Portage can be configured to a vast extent through environment variables.

See man make.conf for information on available environment variables. Refer also to the [Handbook section for working with environment variables](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar) in Gentoo.

To view all presently set environment variables, run:

`user $``emerge --info --verbose`
In addition to the **[Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)**, from which Portage will pull packages by default, additional [ebuild repositories](https://wiki.gentoo.org/wiki/Ebuild_repository) are available, for example:

- [repos.gentoo.org](https://repos.gentoo.org/) - list of repositories contributed by the community, some by Gentoo developers
- [GURU](https://wiki.gentoo.org/wiki/GURU) - official ebuild repository maintained collaboratively by Gentoo users, with a little support from a few Gentoo developers
- [gpo.zugaina.org](https://gpo.zugaina.org/) - third-party list of ebuild repositories

The ebuild repository article has a section on [configuring ebuild repositories](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_management) to be used by Portage.

Search for available ebuilds on the command line with emerge --search or [eix](https://wiki.gentoo.org/wiki/Eix).

Binary hosts are configured in /etc/portage/binrepos.conf and allow fast installation of binary packages, as long as there is a package available for the requested [USE flags](https://wiki.gentoo.org/wiki/USE_flag) for the package being installed or updated.

There is an [official Gentoo binary host](https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart) that contains many binary packages for the **amd64** and **arm64** architectures - see the guide at that link for further setup and usage instructions.

To configure alternative binary hosts, and for more information on using binary packages with Portage, see the [binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide).

Portage includes many different tools and utilities to help with system administration and maintenance. The following sections list these in alphabetical order.

The purpose of archive-conf is to save off a config file in the dispatch-conf archive directory. Most users should not *ever* need to run this command:

`root #``archive-conf`
Usage: archive-conf /CONFIG/FILE \[/CONFIG/FILE...\]

The dispatch-conf utility is used to manage configuration file updates. See the [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) article.

ebuild is Portage's command for running the various [ebuild functions](https://devmanual.gentoo.org/ebuild-writing/functions/).

For a brief summary of usage and command-line options:

`root #``ebuild --help````
usage: Usage: ebuild <ebuild file> <command> [command] ...
See the ebuild(1) man page for more info
options:
  -h, --help            show this help message and exit
  --force               When used together with the digest or manifest command, this option forces regeneration of digests for all distfiles associated
                        with the current ebuild. Any distfiles that do not already exist in ${DISTDIR} will be automatically fetched.
  --color {y,n}         enable or disable color output
  --debug               show debug output
  --version             show version and exit
  --ignore-default-opts
                        do not use the EBUILD_DEFAULT_OPTS environment variable
  --skip-manifest       skip all manifest checks
```
For more information on the ebuild command, view it's man page:

`user $``man 1 ebuild`
The egencache tool rebuilds the cache of metadata information for the ebuild repositories. See the [egencache](https://wiki.gentoo.org/wiki/Egencache) article for additional information.

Performs package management related system health checks and maintenance.

See [repository synchronization](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization) about how to use emaint to synchronize repositories. See man 1 emaint for detailed information.

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
[emerge](https://wiki.gentoo.org/wiki/Emerge) is the command-line interface to Portage and is how most users will interact with Portage.

See the [emerge](https://wiki.gentoo.org/wiki/Emerge) article for more information on the wiki.

Install a Gentoo ebuild repository snapshot from the web. See [Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Installing_a_Gentoo_ebuild_repository_snapshot_from_the_web).

`root #``emerge-webrsync -h`
Usage: /usr/bin/emerge-webrsync \[options\]
 
Options:
  --revert=yyyymmdd   Revert to snapshot
  -k, --keep          Keep snapshots in DISTDIR (don't delete)
  -q, --quiet         Only output errors
  -v, --verbose       Enable verbose output
  -x, --debug         Enable debug output
  -h, --help          This help screen (duh!)

emerge-webrsync is called internally by [eix-sync](https://wiki.gentoo.org/wiki/Ebuild_repository#eix) when `sync-type` in [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) is set to [webrsync](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Features#Validated_Gentoo_repository_snapshots)

Tool for mirroring of package distfiles.

`root #``emirrordist -h````
usage: emirrordist [options] <action>
 
emirrordist - a fetch tool for mirroring of package distfiles
 
optional arguments:
  -h, --help            show this help message and exit
 
Actions:
  --version             display portage version and exit
  --mirror              mirror distfiles for the selected repository
 
Common options:
  --dry-run             perform a trial run with no changes made (usually
                        combined with --verbose)
  --verbose, -v         display extra information on stderr (multiple
                        occurences increase verbosity)
  --ignore-default-opts
                        do not use the EMIRRORDIST_DEFAULT_OPTS environment
                        variable
  --distfiles DIR       distfiles directory to use (required)
  --jobs JOBS, -j JOBS  number of concurrent jobs to run
  --load-average LOAD, -l LOAD
                        load average limit for spawning of new concurrent jobs
  --tries TRIES         maximum number of tries per file, 0 means unlimited
                        (default is 10)
  --repo REPO           name of repo to operate on
  --config-root DIR     location of portage config files
  --repositories-configuration REPOSITORIES_CONFIGURATION
                        override configuration of repositories (in format of
                        repos.conf)
  --strict-manifests <y|n>
                        manually override "strict" FEATURES setting
  --failure-log FILE    log file for fetch failures, with tab-delimited
                        output, for reporting purposes
  --success-log FILE    log file for fetch successes, with tab-delimited
                        output, for reporting purposes
  --scheduled-deletion-log FILE
                        log file for scheduled deletions, with tab-delimited
                        output, for reporting purposes
  --delete              enable deletion of unused distfiles
  --deletion-db FILE    database file used to track lifetime of files
                        scheduled for delayed deletion
  --deletion-delay SECONDS
                        delay time for deletion, measured in seconds
  --temp-dir DIR        temporary directory for downloads
  --mirror-overrides FILE
                        file holding a list of mirror overrides
  --mirror-skip MIRROR_SKIP
                        comma delimited list of mirror targets to skip when
                        fetching
  --restrict-mirror-exemptions RESTRICT_MIRROR_EXEMPTIONS
                        comma delimited list of mirror targets for which to
                        ignore RESTRICT="mirror"
  --verify-existing-digest
                        use digest as a verification of whether existing
                        distfiles are valid
  --distfiles-local DIR
                        distfiles-local directory to use
  --distfiles-db FILE   database file used to track which ebuilds a distfile
                        belongs to
  --recycle-dir DIR     directory for extended retention of files that are
                        removed from distdir with the --delete option
  --recycle-db FILE     database file used to track lifetime of files in
                        recycle dir
  --recycle-deletion-delay SECONDS
                        delay time for deletion of unused files from recycle
                        dir, measured in seconds (defaults to the equivalent
                        of 60 days)
  --fetch-log-dir DIR   directory for individual fetch logs
  --whitelist-from FILE
                        specifies a file containing a list of files to
                        whitelist, one per line, # prefixed lines ignored
```
See also man emirrordist.

Updates environment settings automatically.

`root #``env-update -h`
Usage: env-update \[--no-ldconfig\]
 
See the env-update(1) man page for more info

See also man env-update. See the [login](https://wiki.gentoo.org/wiki/Login) article for some information on how the environment is set up in Gentoo.

Perform package move updates for all packages.

`root #``fixpackages -h`
usage: fixpackages \[-h\]
 
The fixpackages program performs package move updates on configuration files,
installed packages, and binary packages.
 
optional arguments:
  -h, --help  show this help message and exit

See also man fixpackages.

[Gentoo Linux Security Announcements](https://wiki.gentoo.org/wiki/GLSA), or [GLSAs](https://www.gentoo.org/support/security/), are notifications sent out to the community to inform of security vulnerabilities related broadly to Gentoo Linux or specifically to packages contained in the ::gentoo ebuild repository.

glsa-check is a tool to keep track of the various [GLSAs](https://security.gentoo.org/glsa/). It can be used to view GLSAs, but more importantly to test if the system is vulnerable to known GLSAs.

See man glsa-check and glsa-check --help for more information:

`user $``glsa-check --help`
usage: glsa-check \<option> \[glsa-id | all | new | affected\]
 
optional arguments:
  -h, --help        show this help message and exit
  -V, --version     Show information about glsa-check
  -q, --quiet       Be less verbose and do not send empty mail
  -v, --verbose     Print more messages
  -n, --nocolor     Removes color from output
  -e, --emergelike  Upgrade to latest version (not least-change)
  -c, --cve         Show CVE IDs in listing mode
  -r, --reverse     List GLSAs in reverse order
 
Modes:
  -l, --list        List a summary for the given GLSA(s) or set and whether they affect the system
  -d, --dump        Show all information about the GLSA(s) or set
  --print           Alias for --dump
  -t, --test        Test if this system is affected by the GLSA(s) or set and output the GLSA ID(s)
  -p, --pretend     Show the necessary steps to remediate the system
  -f, --fix         (experimental) Attempt to remediate the system based on the instructions given in the GLSA(s) or set. This will only upgrade (when an upgrade path exists) or remove packages
  -i, --inject      Inject the given GLSA(s) into the glsa\_injected file
  -m, --mail        Send a mail with the given GLSAs to the administrator
 
glsa-list can contain an arbitrary number of GLSA ids, filenames containing GLSAs or the special identifiers 'all' and 'affected'

**Todo:**

- This section needs an explanation on the use of this command.

`user $``gpkg-sign --help````
usage: gpkg-sign [options] <gpkg package file>
options:
  -h, --help            show this help message and exit
  --keep-current-signature
                        Keep existing signature when updating signature (default: false)
  --allow-unsigned      Allow signing from unsigned packages when binpkg-request-signature is enabled (default: false)
  --skip-signed         Skip signing if a package is already signed (default: false)
```
For details see [portageq](https://wiki.gentoo.org/wiki/Portageq).

Creates binary packages **using the current state of files present on the system**. See the [quickpkg section of the Binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide#Using_quickpkg) for more information.

`user $``quickpkg --help````
usage: quickpkg [options] <list of package atoms or package sets>
 
optional arguments:
  -h, --help            show this help message and exit
  --umask UMASK         umask used during package creation (default is 0077)
  --ignore-default-opts
                        do not use the QUICKPKG_DEFAULT_OPTS environment variable
  --include-config <y|n>
                        include all files protected by CONFIG_PROTECT (as a security precaution, default is 'n')
  --include-unmodified-config <y|n>
                        include files protected by CONFIG_PROTECT that have not been modified since installation (as a
                        security precaution, default is 'n')
```
See also man quickpkg.

See the [regenworld](https://wiki.gentoo.org/wiki/Regenworld) article.

Gentoo has many more configuration options than most distributions allow. This leads to terminology which can be confusing at first, such as **blockers**, **circular dependencies**, **REQUIRED\_USE**, etc.

[Portage/Help](https://wiki.gentoo.org/wiki/Portage/Help) will help a user understand how they come about and how to resolve them.

To see when the Gentoo ebuild repository was last updated (synced), run the following command:

`user $``cat /var/db/repos/gentoo/metadata/timestamp.chk`
Need to determine what packages are inside each set? See [Package sets](https://wiki.gentoo.org/wiki/Package_sets#Listing).

Although it should be very rare, as with all data, there remains a possibility that Portage could become corrupt or even uninstalled, which would be *very* bad for the functioning of the whole system. If ever this were to occur, there *are* ways Portage can be recovered, however, because Portage is so central, re-installation is a rather involved operation, requiring manual intervention to, in effect, install a package manager without having a functioning package manager.

See [Fix my Gentoo](https://wiki.gentoo.org/wiki/Fix_my_Gentoo) for details on emergency installation via binary packages. See also [Fixing broken Portage](https://wiki.gentoo.org/wiki/Project:Portage/Fixing_broken_portage).

As of portage v2.3.66<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, which was released on 2019-04-29<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, the default locations changed for the `portdir`, `distdir`, `repo_name`, `repo_basedir` directories.

For more information see bug [bug #662982](https://bugs.gentoo.org/show_bug.cgi?id=662982).

- [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage) — the primary configuration directory for [Portage], Gentoo's package manager.
- [/etc/portage/bashrc](https://wiki.gentoo.org/wiki//etc/portage/bashrc) — a global bashrc file referenced by Portage.
- [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) — the main configuration file used to customize the [Portage] environment on a global level., the location [Portage] keeps binary packages.
- [/etc/portage/color.map](https://wiki.gentoo.org/wiki//etc/portage/color.map) — a file containing variables that define color classes used by Portage.
- [prefix](https://wiki.gentoo.org/wiki/Prefix) — enables the power of Gentoo and [Portage] on other distributions and/or operating systems (Microsoft Windows via Cygwin, Android via Termux, etc.).

- [Upgrading Gentoo](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) — explains how to **upgrade (update)** Gentoo, as well as how to proceed for a well maintained system.
- [Catalyst](https://wiki.gentoo.org/wiki/Catalyst) — a tool to build [stage files](https://wiki.gentoo.org/wiki/Stage_file) and [live-images](https://wiki.gentoo.org/wiki/Live_image) for Gentoo
- [Creating an ebuild repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) — basics of creating an ebuild repository and maintaining ebuilds in it.
- [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization) — an introduction to optimizing compiled code using safe, sane [`CFLAGS` and `CXXFLAGS`](https://en.wikipedia.org/wiki/CFLAGS).
- [Portage tips](https://wiki.gentoo.org/wiki/Portage_tips) — the main command-line interface to [Portage]
- [Repository format](https://wiki.gentoo.org/wiki/Repository_format) — A quick reference to Gentoo ebuild repository (overlay) format.
- [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification) — a standardization effort to ensure that the [ebuild](https://wiki.gentoo.org/wiki/Ebuild) file format, the ebuild repository format (of which the Gentoo ebuild repository is the main incarnation), as well as behavior of the package managers interacting with these ebuilds is properly agreed upon and documented.
- [Ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) — a file-structure  that can provide packages for installation on a Gentoo system.
- [Category:Portage](https://wiki.gentoo.org/wiki/Category:Portage)
- [Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) — a suite of tools to ease the administration of a Gentoo system, and [Portage] in particular.
- [Portage Multi Stage Dockerfile](https://wiki.gentoo.org/wiki/Portage_Multi_Stage_Dockerfile) — The emerge --quickpkg-direct and related emerge --quickpkg-direct-root options are useful inside Dockerfiles
- [Portage Security](https://wiki.gentoo.org/wiki/Portage_Security) — aims to answer the question *"How can I dispel doubts regarding the security of the Gentoo ebuild repository on a system?"*
- [Portage TMPDIR on tmpfs](https://wiki.gentoo.org/wiki/Portage_TMPDIR_on_tmpfs) — It is unlikely that tmpfs will provide any performance gain for modern systems

- [Useful Portage tools](https://wiki.gentoo.org/wiki/Useful_Portage_tools) — provides a list of Gentoo-specific system management tools, notably for [Portage], available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).
- [Cfg-update](https://wiki.gentoo.org/wiki/Cfg-update) — a utility used on Gentoo to manage configuration file updates.

- [Pkgcore](https://wiki.gentoo.org/wiki/Pkgcore) — an alternative package manager for Gentoo that aims for high performance, extensibility, and a clean design.
- [app-portage/kuroo](https://packages.gentoo.org/packages/app-portage/kuroo) - Graphical Portage frontend based on KF5/Qt5.
- [App Swipe](https://github.com/k9spud/appswipe) - Qt GUI for browsing local Portage repositories.

- [Package sets](https://wiki.gentoo.org/wiki/Package_sets) — describes package sets in high detail and includes a list of all typically available sets on a Gentoo system.

- [Official Portage documentation](https://dev.gentoo.org/~zmedico/portage/doc) - Built by Portage developer [Zac Medico (zmedico)](https://wiki.gentoo.org/wiki/User:Zmedico)
- [packages.gentoo.org](https://packages.gentoo.org/) - online searchable database of packages from the Gentoo package repository.

The man pages contain complete technical documentation for Portage. Type man \<subject> in a shell on a Gentoo system to read the local man page. Note that man pages have a *see also* section for further information.

- [emerge - command-line interface to the Portage system](https://dev.gentoo.org/~zmedico/portage/doc/man/emerge.1.html) - emerge man page.
- [Portage configuration files](https://dev.gentoo.org/~zmedico/portage/doc/man/portage.5.html) - Portage man page.
