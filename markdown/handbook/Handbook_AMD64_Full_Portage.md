<!-- source: https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Portage | group: Gentoo Handbook | wiki-title: Handbook:AMD64/Full/Portage -->
---
title: "Gentoo Linux amd64 Handbook: Working with Portage"
url: https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Portage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-12-13"
fingerprint: bc283e3e8ea39180
license: CC BY-SA 4.0
---

# Gentoo Linux amd64 Handbook: Working with Portage

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)





Portage comes with a default configuration stored in /usr/share/portage/config/make.globals. All Portage configuration is handled through variables. Portage's variables and what they mean are described later.

Since many configuration directives differ between architectures, Portage also has default configuration files which are part of the system profile. This profile is pointed to by the /etc/portage/make.profile symlink; Portage's configurations are set in the make.defaults files of the profile and all parent profiles. We'll explain more about profiles and the /etc/portage/make.profile directory later on.

When changing a configuration variable, do not alter /usr/share/portage/config/make.globals or make.defaults. Instead use /etc/portage/make.conf which has precedence over the previous files. For more information, read the /usr/share/portage/config/make.conf.example file. As the name implies, this is merely an example file - Portage does not read it.

It is also possible to define a Portage configuration variable as an environment variable, but we don't recommend this.

We've already encountered the /etc/portage/make.profile directory. This is actually a symbolic link to a profile - by default, one inside /var/db/repos/gentoo/profiles/, although one can create profiles elsewhere and point to them. The profile this symlink points to is the profile to which the system adheres.

A profile contains architecture-specific information for Portage, such as a list of packages that belong to the system corresponding with that profile, a list of packages that don't work (or are masked-out) for that profile, etc.

When Portage's behavior needs to be changed, certain files inside /etc/portage/ will need to be modified. Using files within /etc/portage/ is highly recommended; overriding Portage's behavior through environment variables is highly discouraged.

Within /etc/portage/ users can create the following files:

- package.mask which lists the packages that Portage should never try to install
- package.unmask which lists the packages Portage should be able to install even though the Gentoo developers highly discourage users from emerging them
- package.accept\_keywords which lists the packages Portage should be able to install even though the package hasn't been found suitable for the system or architecture (yet)
- package.use which lists the USE flags to use for certain packages without having the entire system use those USE flags

These don't have to be files; they can also be directories that contain one or more file(s) with the relevant settings. More information about the /etc/portage/ directory, together with a full list of files that can be created, can be found in the Portage man page:

`user $``man portage`
The previously mentioned configuration files cannot be stored elsewhere - Portage will always look for those configuration files at those exact locations. However, Portage uses many other locations, for various purposes: package builds, source code storage, repository locations, and so on. All of these have default locations, but can be altered to personal taste through /etc/portage/make.conf.

All these purposes have well-known default locations but can be altered to personal taste through /etc/portage/make.conf. The rest of this chapter explains what special-purpose locations Portage uses and how to alter their placement on the filesystem.

This Handbook isn't meant to be used as a reference though - for details, please refer to the `portage` and `make.conf` man pages:

`user $````
man portage
```
`user $````
man make.conf
```
The default location of the Gentoo ebuild repository is /var/db/repos/gentoo.  This is defined by the default repos.conf file found at /usr/share/portage/config/repos.conf. To modify the default, copy this file to /etc/portage/repos.conf/gentoo.conf and change the `location` setting. When storing the Gentoo ebuild repository elsewhere (by altering this variable), don't forget to change the /etc/portage/make.profile symbolic link accordingly.

After changing the `location` setting in /etc/portage/repos.conf/gentoo.conf, it is recommended to also alter the following variables in /etc/portage/make.conf, since - due to how Portage handle variables - they will not notice the location change: `PKGDIR`, `DISTDIR`, and `RPMDIR`.

Even though Portage doesn't use pre-built binaries by default, it has extensive support for them. When asking Portage to work with pre-built packages, it will look for them in /var/cache/binpkgs. This location is defined by the  `PKGDIR` variable.

Application source code is stored in /var/cache/distfiles by default. This location is defined by the `DISTDIR` variable.

Portage stores the state of the system (what packages are installed, what files belong to which package, ...) in /var/db/pkg.

The Portage cache (with modification times, virtuals, dependency tree information, ...) is stored in /var/cache/edb. This location really is a cache: users can clean it if they are not running any Portage-related application at that moment.

Portage's temporary files are stored in /var/tmp/ by default. This is defined by the `PORTAGE_TMPDIR` variable.

Portage creates specific build directories for each package it emerges inside /var/tmp/portage/. This location is defined by the `PORTAGE_TMPDIR` variable with portage/ appended.

By default Portage installs all files on the current filesystem (/), but this can be changed by setting the `ROOT` environment variable. This is useful when creating new build images.

Portage can create per-ebuild log files, but only when the `PORT_LOGDIR` variable is set to a location that is writable by Portage (through the Portage user). By default this variable is unset. If `PORT_LOGDIR` is not set, then there will not be any build logs with the current logging system, though users may receive some logs from the new elog support.

If `PORT_LOGDIR` is not set, there will be no build logs stored in the enabled logging system (although users may receive some logs from the new elog support.

Portage offers fine-grained control over logging through the use of elog:

- `PORTAGE_ELOG_CLASSES`: This is where users can define the kinds of messages to be logged. The variable's value can be any space-separated combination of `info`, `warn`, `error`, `log`, and `qa`.
  - `info`: Logs "einfo" messages printed by an ebuild
  - `warn`: Logs "ewarn" messages printed by an ebuild
  - `error`: Logs "eerror" messages printed by an ebuild
  - `log`: Logs the "elog" messages found in some ebuilds
  - `qa`: Logs the "QA Notice" messages printed by an ebuild

- `PORTAGE_ELOG_SYSTEM`: This selects the module(s) to process the log messages. If left empty, logging is disabled. Anny space-separated combination of `save`, `custom`, `syslog`, `mail`, `save_summary`, and `mail_summary` can be used. At least one module must be used in order to use elog.
  - `save`: This saves one log per package in $PORT\_LOGDIR/elog, or /var/log/portage/elog if $PORT\_LOGDIR is not defined.
  - `custom`: Passes all messages to a user-defined command in $PORTAGE\_ELOG\_COMMAND; this will be discussed later.
  - `syslog`: Sends all messages to the installed system logger.
  - `mail`: Passes all messages to a user-defined mailserver in $PORTAGE\_ELOG\_MAILURI; this will be discussed later. The mail features of elog require >=portage-2.1.1.
  - `save_summary`: Similar to save, but it merges all messages in $PORT\_LOGDIR/elog/summary.log, or /var/log/portage/elog/summary.log if $PORT\_LOGDIR is not defined.
  - `mail_summary`: Similar to mail, but it sends all messages in a single mail when emerge exits.

- `PORTAGE_ELOG_COMMAND`: This is only used when the `custom` module is enabled. Users can specify a command to process log messages. Note that the command can make use of two variables: ${PACKAGE} is the package name and version, while ${LOGFILE} is the absolute path to the logfile. For instance:

- `PORTAGE_ELOG_MAILURI`: This contains settings for the mail module, such as address, user, password, mail server, and port number. The default setting is "root@localhost localhost". The following is an example for an SMTP server that requires username and password-based authentication on a particular port (the default is port 25):

- `PORTAGE_ELOG_MAILFROM`: Allows the user to set the "from" address of log mails; defaults to "Portage" if unset.

- `PORTAGE_ELOG_MAILSUBJECT`: Allows the user to create a subject line for log mails. Note that it can make use of two variables: ${PACKAGE} will display the package name and version, while ${HOST} is the fully qualified domain name of the host Portage is running on. For instance:









As noted previously, Portage is configurable through variables in /etc/portage/make.conf or one of the subdirectories of /etc/portage/. Please refer to the man pages for `make.conf` and `portage` for more information:

`user $````
man make.conf
```
`user $``man portage`
When Portage builds applications, it passes the contents of the following variables to the compiler and configure script:

- `CFLAGS` and `CXXFLAGS`
- Define the desired compiler flags for C and C++ compiling.
- `CHOST`
- Defines the build host information for the application's configure script
- `MAKEOPTS`
- Passed to the make command and usually set to define the amount of parallelism used during the compilation. Information about options for make can be found in the [make(1)](https://man.archlinux.org/man/make.1.en)

The `USE` variable is also used during configure and compilation; refer to previous chapters of this Handbook for details.

When Portage has merged a newer version of a certain software title, it will remove the obsoleted files of the older version from the system. Portage gives the user a 5 second delay before unmerging the older version. These 5 seconds are defined by the `CLEAN_DELAY` variable.

It is possible to tell emerge to use certain options every time it is run by setting `EMERGE_DEFAULT_OPTS`. Some useful options include `--ask`, `--verbose`, and `--tree`.

Portage overwrites files provided by newer versions of a software title if the files aren't stored in a protected location. These protected locations are defined by the `CONFIG_PROTECT` variable and are generally configuration file locations. The directory listing is space-delimited.

A file that would be written in such a protected location is renamed and the user is warned about the presence of a newer version of the (proposed) configuration file.

To find out about the current `CONFIG_PROTECT` setting, use the emerge --info output:

`user $``emerge --info | grep 'CONFIG_PROTECT='`
More information about Portage's configuration file protection is available in the CONFIGURATION FILES section of the emerge man page:

`user $``man emerge`
To 'unprotect' certain subdirectories of protected locations, use the `CONFIG_PROTECT_MASK` variable.

When the requested information or data is not available on the system, Portage will retrieve it from the Internet. The server locations for the various information and data channels are defined by the following variables:

- `GENTOO_MIRRORS`
- Defines a list of server locations which contain source code (distfiles).
- `PORTAGE_BINHOST`
- Defines a particular server location containing prebuilt packages for the system.

A third setting involves the location of the rsync server which users use to update their local Gentoo repository. This is defined in the /etc/portage/repos.conf file (or a file inside that directory if it is defined as a directory):

- `sync-type`
- Defines the type of server and defaults to `rsync`.
- `sync-uri`
- Defines a particular server which Portage uses to fetch the Gentoo repository.

The `GENTOO_MIRRORS`, `sync-type`, and `sync-uri` variables can be set automatically through the mirrorselect application, provided by the [app-portage/mirrorselect](https://packages.gentoo.org/packages/app-portage/mirrorselect) package. For more information, refer to mirrorselect's online help:

`root #``mirrorselect --help`
If the environment requires the use of a proxy server, then the `http_proxy`, `ftp_proxy`, and `RSYNC_PROXY` variables can be declared.

When Portage needs to fetch source code, it uses [wget(1)](https://man.archlinux.org/man/wget.1.en) [by default. This can be changed through the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `FETCHCOMMAND` variable.

Portage is able to resume partially downloaded source code. It uses [wget(1)](https://man.archlinux.org/man/wget.1.en) [by default, but this can be altered through the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `RESUMECOMMAND` variable.

Make sure that the `FETCHCOMMAND` and `RESUMECOMMAND` store the source code in the correct location. Inside the variables the `URI` and `DISTDIR` variables can be used to point to the source code location and distfiles location, respectively.

It is also possible to define protocol-specific handlers with `FETCHCOMMAND_HTTP`, `FETCHCOMMAND_FTP`, `RESUMECOMMAND_HTTP`, `RESUMECOMMAND_FTP`, and so on.

It is not possible to alter the rsync command used by Portage to update the Gentoo repository, but it is possible to set some variables related to the rsync command:

- `PORTAGE_RSYNC_OPTS`
- Sets a number of default variables used during sync, each space-separated. These shouldn't be changed unless you know exactly what you're doing. Note that certain absolutely required options will always be used even if `PORTAGE_RSYNC_OPTS` is empty.

- `PORTAGE_RSYNC_EXTRA_OPTS`
- Used to set additional options when syncing. Each option should be space separated:
  - `--timeout=<number>`
  - This defines the number of seconds an rsync connection can idle before rsync sees the connection as timed-out. This variable defaults to `180` but dialup users or individuals with slow computers might want to set this to `300` or higher.
  - `--exclude-from=/etc/portage/rsync_excludes`
  - This points to a file listing the packages and/or categories rsync should ignore during the update process. In this case, it points to /etc/portage/rsync\_excludes.
  - `--quiet`
  - Reduces output to the screen.
  - `--verbose`
  - Prints a complete filelist.
  - `--progress`
  - Displays a progress meter for each file.

- `PORTAGE_RSYNC_RETRIES`
- Defines how many times rsync should try connecting to the mirror pointed to by the `SYNC` variable before bailing out. This variable defaults to `3`.

For more information on these options and others, please refer to the [rsync(1)](https://man.archlinux.org/man/rsync.1.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

It is possible to change the default branch with the `ACCEPT_KEYWORDS` variable. It defaults to the architecture's stable branch. More information on Gentoo's branches can be found in the next chapter.

It is possible to activate certain Portage features through the `FEATURES` variable. The Portage features have been discussed in previous chapters.

With the `PORTAGE_NICENESS` variable users can augment or reduce the nice value Portage will use while running. The `PORTAGE_NICENESS` value is *added* to the current nice value of Portage.

For more information about nice values, refer to the [Portage niceness](https://wiki.gentoo.org/wiki/Portage_niceness) page and the [nice(1)](https://man.archlinux.org/man/nice.1.en) [man page:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``man nice`
The `NOCOLOR` variable, which defaults to false, determines whether Portage should disable the use of colored output.









The `ACCEPT_KEYWORDS` variable defines the system's software branch. This variable is set in the /etc/portage/make.conf file and is configured to the *stable* branch by default. In this instance the default value is **amd64**.

**`/etc/portage/make.conf`**

**Using the stable branch**

```
# The following value is set by default, and does _not_ need explicitly defined.
ACCEPT_KEYWORDS="amd64"
```
Using the testing branch and testing new packages (both ebuild changes and upstream releases) helps Gentoo fix problems. Alternatively, using the stable branch has less risk of instability or other issues. Cautious users should stay on the stable software branch in general, and only allow testing versions of packages on a case-by-case basis - refer to [the "Mixing stable with testing" section](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Mixing_stable_with_testing) below.

In cases where the system administrator would like to run a less-tested software stack, and in exchange receive 'bleeding edge' updates, the testing branch can be selected. To switch to the testing branch, set the `ACCEPT_KEYWORDS` value to **\~amd64**.

**`/etc/portage/make.conf`**

**Using the testing branch**

```
# Instructing Portage to use the testing branch (~) for the amd64 architecture.
ACCEPT_KEYWORDS="~amd64"
```
Any architecture supported by the Gentoo project can move to a testing branch by simply adding a `~` (tilde symbol) in front of the architecture's `ACCEPT_KEYWORDS` value.

The testing branch is exactly what one would expect - unstable. If a package is in testing, it means that the developers believe it is functional but has not been thoroughly tested. Systems using the testing branch may be the first to encounter bugs in the package. If bugs are discovered, please [file a bug report](https://bugs.gentoo.org) to make the developer(s) aware.

When changing from the stable to the testing branch and performing a [@world](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) update, it is normal for many - sometimes *hundreds* - of packages to be updated to the latest available versions. Updates generally correspond to the upstream project's current release. Keep in mind that, after moving from stable to testing, it may prove a challenge to revert back to the stable branch. This is to be expected.

It is possible to instruct Portage to allow the *testing* branch for particular packages, but use the *stable* branch for the rest of the system. To achieve this, add the package category and name into the /etc/portage/package.accept\_keywords file. It is also possible to create a *directory* (with the same name) and list the package in a file under the directory.

For instance, to use the testing branch for gnumeric:

**`/etc/portage/package.accept_keywords`**

**Use the testing branch for just the gnumeric application**

To use a specific software version from the testing branch but not allow updates from the testing branch for subsequent versions, a specific version of the package can be defined in the package.accept\_keywords file. To define a specific version, the `=` operator is utilized. It is also possible to enter a version range using the `<=`, `<`, `>` or `>=` operators.

In any case, if any package version information is defined, an operator *must* be used. Without version information, an operator *cannot* be used.

In the following Portage is instructed to allow the installation of a specific version of gnumeric, version *1.2.13*, even if it is in the testing branch:

**`/etc/portage/package.accept_keywords`**

**Allow a specific version selection of the gnumeric package**

For the next example, presume a package has been masked by the Gentoo developers. When attempting to install the package, Portage will output a message indicating the package has been masked, together with (usually) the reason why. Next presume a system administrator, despite the reason mentioned in the package.mask file (situated in /var/db/repos/gentoo/profiles/ by default) still desires to install the package.

To do so, the system administrator would add the target package version (usually this will be the exact same line from the package.mask file in the profile) to the /etc/portage/package.unmask location.

For instance, if `=mail-client/mutt-2.2.9` is masked, then it can be unmasked by adding the exact same line in the package.unmask location:

**`/etc/portage/package.unmask`**

**Unmasking a particular version of a package**

It is also possible to instruct Portage to *not* install a certain package or not update to a specific version of a package. This is called masking a package. To mask a  package, add qualifiers as necessary to the /etc/portage/package.mask location (either in that file or in a file in this directory).

For instance, to prevent Portage from installing kernel sources *newer* than gentoo-sources-6.18.8, add the following line at the package.mask location:

**`/etc/portage/package.mask`**

**Mask sys-kernel/gentoo-sources for versions greater than 6.18.8**











dispatch-conf is a tool that aids in merging the .\_cfg0000\_\<name> files. .\_cfg0000\_\<name> files are generated by Portage when it wants to overwrite a file in a directory protected by the CONFIG\_PROTECT variable.

With dispatch-conf, users are able to merge updates to their configuration files while keeping track of all changes. dispatch-conf stores the differences between the configuration files as patches or by using the RCS revision system. This means that if someone makes a mistake when updating a config file, the administrator can revert the file to the previous version at any time.

When using dispatch-conf, users can ask to keep the configuration file as-is, use the new configuration file, edit the current one or merge the changes interactively. dispatch-conf also has some nice additional features:

- Automatically merge configuration file updates that only contain updates to comments.
- Automatically merge configuration files which only differ in the amount of whitespace.

Edit /etc/dispatch-conf.conf first and create the directory referenced by the `archive-dir` variable. Then, execute dispatch-conf:

`root #``dispatch-conf`
When running dispatch-conf, each changed config file will be reviewed one at a time. Press `u` to update (replace) the current config file with the new one and continue to the next file. Press `z` to zap (delete) the new config file and continue to the next file. The `n` key will instruct dispatch-conf to skip to the next file. This can be done to delay a merge until a future time. Once all config files have been taken care of, dispatch-conf will exit. At any time, `q` can be used to exit the application as well.

For more information, check out the dispatch-conf man page. It describes how to interactively merge current and new config files, edit new config files, examine differences between files, and more.

`user $``man dispatch-conf`
With quickpkg users can create archives of the packages that are already merged on the system. These archives can be used as prebuilt packages. Running quickpkg is straightforward: just add the names of the packages to archive.

For instance, to archive curl, orage, and procps:

`root #``quickpkg curl orage procps`
The prebuilt packages will be stored in $PKGDIR (/var/cache/binpkgs/ by default). These packages are placed in $PKGDIR/CATEGORY.









It is possible to selectively update certain categories/packages and ignore other categories/packages. This can be achieved by having rsync exclude categories/packages during the emerge --sync step.

Define the name of the file that contains the exclude patterns in the `PORTAGE_RSYNC_EXTRA_OPTS` variable in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

**Defining the exclude file**

```
PORTAGE_RSYNC_EXTRA_OPTS="--exclude-from=/etc/portage/rsync_excludes"
```
**`/etc/portage/rsync_excludes`**

**Excluding all games**

Alternatively, a custom ebuild repository can be quickly created using the eselect repository module (from [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository)). In the following example, substitute `localrepo` with a name of choice:

`root #``eselect repository create localrepo`
Adding localrepo to /etc/portage/repos.conf/eselect-repo.conf ...
Repository \<ebuild\_repository\_name> created and added

A bare repository named "localrepo" will be made available at /var/db/repos/localrepo.

It is possible to instruct Portage to use ebuilds that are not officially available through the Gentoo ebuild repository. In order to do so, create a new directory (for instance /var/db/repos/localrepo) in which to store the 3rd party ebuilds. This new repository will require the same directory structure as the official Gentoo repository.

`root #````
mkdir -p /var/db/repos/localrepo/{metadata,profiles}
```
`root #````
chown -R portage:portage /var/db/repos/localrepo
```
Next, pick a sensible name for the repository. The next example uses "localrepo" as the name:

`root #``echo 'localrepo' > /var/db/repos/localrepo/profiles/repo_name`
Then define the EAPI used for the profiles within the repository:

`root #``echo '8' > /var/db/repos/localrepo/profiles/eapi`
Tell Portage that the repository master is the main Gentoo ebuild repo, and that the local repository should not be automatically synchronized (as it is not backed by an external source such as an rsync server, git mirror, or other repository type):

**`/var/db/repos/localrepo/metadata/layout.conf`**

```
masters = gentoo
auto-sync = false
thin-manifests = true
sign-manifests = false
```
Finally, enable the repository on the local system by creating a repository configuration file inside /etc/portage/repos.conf. This will inform Portage of where the custom local repository can be found:

**`/etc/portage/repos.conf/localrepo.conf`**

```
[localrepo]
location = /var/db/repos/localrepo
```
For those wishing to develop several ebuild repos, test packages before they hit the Gentoo repository, or who want to use unofficial ebuilds from various sources, [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) also provides tooling to aid in keeping repositories up to date. For details, refer to the [Eselect/Repository](https://wiki.gentoo.org/wiki/Eselect/Repository) page.

For instance, to enable the [GURU](https://wiki.gentoo.org/wiki/Project:GURU) repository:

`root #``eselect repository enable guru`
Updating of repositories added with this method will occur automatically on each sync:

`root #``emerge  --sync`
Sometimes users want to configure, install, and maintain software individually without having Portage automate the process, even though Portage can provide the software. Known cases are packages like kernel sources and Nvidia drivers. It is possible to configure Portage so it knows that a certain package is manually installed on the system (and thus take this information into account when calculating dependencies). This process is called *injecting* and is supported by Portage through the /etc/portage/profile/package.provided file.

For instance, to inform Portage about gentoo-sources-6.18.8 which has been installed manually, add the following line to /etc/portage/profile/package.provided:

**`/etc/portage/profile/package.provided`**

**Marking gentoo-sources-6.18.8 as manually installed**









For most readers, the information received thus far is sufficient for all their Linux operations. But Portage is capable of much more; many of its features are for advanced customization purposes or are only applicable in specific corner cases.

Of course, with lots of flexibility comes a huge list of potential cases. It will not be possible to document them all here. Instead, the goal is to focus on some generic issues which can then be augmented to fit a particular niche. More tweaks and tips such as these can be found across the Gentoo wiki.

Most, if not all, of these additional features can be easily found by digging through the manual pages that are provided with Portage:

`user $````
man portage
```
`user $````
man make.conf
```
Finally, know that these are advanced features which, if not configured correctly, can make debugging and troubleshooting much more difficult. Be sure any of these sorts of customization is explicitly mentioned when opening a bug report.

By default, package builds will use the environment variables defined in /etc/portage/make.conf, such as `CFLAGS`, `MAKEOPTS` and others. In some cases, it is beneficial to provide different variables for specific packages. To do so, Portage supports the use of the /etc/portage/env directory and /etc/portage/package.env file.

The /etc/portage/package.env file contains a list of packages for which deviating environment variables are needed as well as a specific identifier that indicates to Portage which identifier file to apply. The identifier name is free format. Portage will look for a file with the exactly same case-sensitive name as the identifier. See the following example for more detail.

To enable debugging for the [media-video/mplayer](https://packages.gentoo.org/packages/media-video/mplayer) package:

Set the debugging variables in a file called /etc/portage/env/debug-cflags. The filename is arbitrarily chosen, but of course the name reflects the *purpose* of the file, which should make it obvious if reviewing later why the file exists. The /etc/portage/env directory will need created if it does not yet exist:

`root #``mkdir /etc/portage/env`
**`/etc/portage/env/debug-cflags`**

**Set specific variables for debugging**

```
# Add -ggdb to CFLAGS for debugging 
CFLAGS="-O2 -ggdb -pipe"
FEATURES="${FEATURES} nostrip"
```
Next, the [media-video/mplayer](https://packages.gentoo.org/packages/media-video/mplayer) package is 'tagged' to use the new debug identifier in the package.env file:

**`/etc/portage/package.env`**

**Use the debug-cflags identifier for the mplayer package**

When Portage works with ebuilds, it uses a Bash environment in which it calls the various build functions (like `src_prepare`, `src_configure`, `pkg_postinst`, etc.). Portage allows system administrators to set up a specific Bash environment.

The advantage of using a specific Bash environment is that it allows for hooks into the emerge process during each step it performs. This can be done for every emerge (through /etc/portage/bashrc) or by using per-package environments (through /etc/portage/env as discussed earlier).

Portage calls so-called phase hook functions if they are defined in /etc/portage/bashrc. These functions follow the naming convention `pre_` and *phase\_name*`post_`. For example, Portage will call the *phase\_name*`post_pkg_postinst` hook just after calling the `pkg_postinst` phase function.

To hook in the process, the Bash environment can listen to the variables that are always available during ebuild development (such as `CATEGORY`, `P`, `PF`, ...). Based on the values of these variables additional steps and/or functions can be executed.

In this example, the /etc/portage/bashrc file will be used to call some file database applications to ensure their databases are up-to-date with the system. The applications used in the example are aide (an intrusion detection tool) and updatedb (to use with mlocate), but these are meant as examples. Do *not* consider this as a guide for AIDE.

To use /etc/portage/bashrc for this case, we need to “hook” in the `postinst` (after installation of files) and `postrm` (after removal of files) phases, because that is when the files on the file system have been changed.

**`/etc/portage/bashrc`**

**Hooking into the postinst and postrm phases**

```
() {
    echo ":: Calling aide --update to update its database"
    aide --update
    echo ":: Calling updatedb to update its database"
    updatedb
}
post_pkg_postinst() { my_database_update; }
post_pkg_postrm() { my_database_update; }
```
In addition to hooking into the ebuild phases, Portage offers another option for hook functionality: postsync.d.These types of hooks are run after updating (also referred to as 'syncing') the Gentoo ebuild repository. In order to run tasks after updating the Gentoo repository, put scripts inside the /etc/portage/postsync.d directory. Be sure any files inside the directory have been marked as executable or they will not run.

If eix-sync was not used to update sync the repository, then it is still possible to update the eix database after running emerge --sync (or emerge-webrsync) by putting a symlink to /usr/bin/eix called eix-update in /etc/portage/postsync.d.

`root #``ln -s /usr/bin/eix /etc/portage/postsync.d/eix-update`
By default, Gentoo uses the settings contained in the profile pointed to by /etc/portage/make.profile, which is a symbolic link to the target profile directory. These profiles define specific settings and inherit settings from other profiles (through their parent files).

By using /etc/portage/profile, system administrators can override profile settings such as packages (what packages are considered to be part of the @system set), force USE flags, and more.

When using an NFS-based file systems for critical file systems, it might be necessary to mark [net-fs/nfs-utils](https://packages.gentoo.org/packages/net-fs/nfs-utils) as a system package, which will cause Portage to heavily warn administrators in the event they attempt to unmerge it.

To accomplish this, the package must be added to the /etc/portage/profile/packages file, prepended with a `*` (asterisk symbol):

**`/etc/portage/profile/packages`**

**Marking net-fs/nfs-utils as a system package**

### /etc/portage/patches/

User patches provide a way to apply patches to package source code when sources are extracted before installation. This can be useful for applying upstream patches to unresolved bugs, or simply for local customizations.

Patches need to be dropped into /etc/portage/patches/. They will automatically be applied during [package installation](https://wiki.gentoo.org/wiki/Emerge).

Patches can be dropped into any of the following directories:

- /etc/portage/patches/${CATEGORY}/${P}/ e.g. /etc/portage/patches/dev-lang/python-3.3.5-r1/
- /etc/portage/patches/${CATEGORY}/${PN}/ e.g. /etc/portage/patches/dev-lang/python-3.4.2/
- /etc/portage/patches/${CATEGORY}/${P}-${PR}/ e.g. /etc/portage/patches/dev-lang/python-3.3.5-r0/

The [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) package is one of the many that already call `eapply_user` (possibly implicitly) from within the ebuild, so there is no need to override anything specific.

If for some reason (for instance because a developer provided a patch and asked to check if it fixes the bug reported) patching Firefox is wanted, all that is needed is to put the patch in /etc/portage/patches/www-client/firefox/ (it might be best to use the full name and version so that the patch does not interfere with later versions) and rebuild Firefox.
