<!-- source: https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Working | group: Gentoo Handbook | wiki-title: Handbook:AMD64/Full/Working -->
---
title: "Gentoo Linux amd64 Handbook: Working with Gentoo"
url: https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Working
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-12-13"
fingerprint: a6219a4a87238b84
license: CC BY-SA 4.0
---

# Gentoo Linux amd64 Handbook: Working with Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)





Portage is one of Gentoo's most notable innovations in software management. With its high flexibility and enormous amount of features it is frequently seen as the best software management tool available for Linux.

Portage is completely written in [Python](https://www.python.org/) and [Bash](https://www.gnu.org/software/bash) and therefore fully visible to the users as both are scripting languages.

Most users will work with Portage through the emerge tool. This chapter is not meant to duplicate the information available from the emerge man page. For a complete rundown of emerge's options, please consult the man page:

`user $``man emerge`
When Gentoo's documentation talks about packages, it means software titles that are available to the Gentoo users through the Gentoo repository. This repository is a collection of ebuilds, files that contain all information Portage needs to maintain software (install, search, query, etc.). These ebuilds reside in /var/db/repos/gentoo by default.

Whenever someone asks Portage to perform some action regarding software titles, it will use the ebuilds on the system as a base. It is therefore important to regularly update the ebuilds on the system so Portage knows about new software, security updates, etc.

The Gentoo repository is usually updated with rsync, a fast incremental file transfer utility. Updating is fairly simple as the emerge command provides a front-end for rsync:

`root #``emerge --sync`
Sometimes firewall restrictions apply that prevent rsync from contacting the mirrors. In this case, the Gentoo repository can be updated via daily generated snapshots. The emerge-webrsync tool automatically fetches and installs the latest snapshot on the system:

`root #``emerge-webrsync`
There are multiple ways to search through the Gentoo repository for software. One way is through emerge itself. By default, emerge --search returns the names of packages whose title matches (either fully or partially) the given search term.

For instance, to search for all packages who have "pdf" in their name:

`user $``emerge --search pdf`
To search through the descriptions as well, use the `--searchdesc` (or `-S`) option:

`user $``emerge --searchdesc pdf`
Notice that the output returns a lot of information. The fields are clearly labelled so we won't go further into their meanings:

When a software title has been found, then the installation is just one emerge command away. For instance, to install gnumeric:

`root #``emerge --ask app-office/gnumeric`
Since many applications depend on each other, any attempt to install a certain software package might result in the installation of several dependencies as well. Don't worry, Portage handles dependencies well. To find out what Portage would install, add the `--pretend` option. For instance:

`root #``emerge --pretend gnumeric`
To do the same, but interactively choose whether or not to proceed with the installation, add the `--ask` flag:

`root #``emerge --ask gnumeric`
During the installation of a package, Portage will download the necessary source code from the Internet (if necessary) and store it by default in /var/cache/distfiles/. After this it will unpack, compile and install the package. To tell Portage to only download the sources without installing them, add the `--fetchonly` option to the emerge command:

`root #``emerge --fetchonly gnumeric`
Many packages come with their own documentation. Sometimes, the `doc` USE flag determines whether the package documentation should be installed or not. To see if the `doc` USE flag is used by a package, use emerge -vp category/package:

`root #``emerge -vp media-libs/alsa-lib`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
\[ebuild   R    \] media-libs/alsa-lib-1.1.3::gentoo  USE="python -alisp -debug -doc" ABI\_X86="(64) -32 (-x32)" PYTHON\_TARGETS="python2\_7" 0 KiB

The best way of enabling the `doc` USE flag is doing it on a per-package basis via /etc/portage/package.use, so that only the documentation for the wanted packages is installed. For more information read the [USE flags](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE) section.

Once the package installed, its documentation is generally found in a subdirectory named after the package in the /usr/share/doc/ directory:

`user $``ls -l /usr/share/doc/alsa-lib-1.1.3`
total 16
-rw-r--r-- 1 root root 3098 Mar  9 15:36 asoundrc.txt.bz2
-rw-r--r-- 1 root root  672 Mar  9 15:36 ChangeLog.bz2
-rw-r--r-- 1 root root 1083 Mar  9 15:36 NOTES.bz2
-rw-r--r-- 1 root root  220 Mar  9 15:36 TODO.bz2

A more sure way to list installed documentation files is to use equery's `--filter` option. equery is used to query Portage's database and comes as part of the [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package:

`user $``equery files --filter=doc alsa-lib`
\* Searching for alsa-lib in media-libs ...
 \* Contents of media-libs/alsa-lib-1.1.3:
/usr/share/doc/alsa-lib-1.1.3/ChangeLog.bz2
/usr/share/doc/alsa-lib-1.1.3/NOTES.bz2
/usr/share/doc/alsa-lib-1.1.3/TODO.bz2
/usr/share/doc/alsa-lib-1.1.3/asoundrc.txt.bz2

The `--filter` option can be used with other rules to view the install locations for many other types of files. Additional functionality can be reviewed in equery's man page: man 1 equery.

To safely remove software from a system, use emerge --deselect. This will tell Portage a package is no longer required and it is eligible for cleaning through `--depclean`.

`root #``emerge --deselect gnumeric`
When a package is no longer selected, the package and its dependencies that were installed automatically when it was installed are still left on the system. To have Portage locate all dependencies that can now be removed, use emerge's `--depclean` functionality, which is documented later.

To keep the system in perfect shape (and not to mention install the latest security updates) it is necessary to update the system regularly. Since Portage only checks the ebuilds in the Gentoo repository, the first thing to do is to update this repository using emerge --sync. Then the system can be updated using emerge --deep --update @world.

Portage will, with `--deep`, search for newer versions of the applications that are installed. Without `--deep`, it will only verify the versions for the applications that are explicitly installed (the applications listed in /var/lib/portage/world) - it does not thoroughly check their dependencies. This option should almost always therefore be used:

`root #``emerge --update --deep @world`
The standard upgrade command should include `--changed-use` or `--newuse` because of possible changes within the repository's profiles, or if the USE settings of the system have been altered. Portage will then verify if the change requires the installation of new packages or recompilation of existing ones:

`root #``emerge --update --deep --newuse @world`
Some packages in the Gentoo repository do not contain functional software themselves, but exist to install a collection of related packages. For instance, the [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta) package will install the KDE Plasma desktop environment by pulling in various Plasma-related packages as dependencies.

To remove such a package from the system, running emerge --deselect on the package will not have much effect since the dependencies for the package remain on the system.

Portage has the functionality to remove orphaned dependencies; however, the availability of software is dynamically dependent. It is important to update the entire system (see [@world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>)), including all changes resulting from USE flag modifications. After this, the user can run emerge --depclean to remove the orphaned dependencies. Finally, it may be necessary to rebuild the applications that were dynamically linked to the now-removed software titles (see [@preserved-rebuild](https://wiki.gentoo.org/wiki/Preserved-rebuild)) but don't require them anymore, although recently automated support for this has been added to Portage.

All this is handled with the following two commands:

`root #````
emerge --update --deep --newuse @world
```
`root #````
emerge --ask --depclean
```
Beginning with Portage version 2.1.7, it is possible to accept or reject software installation based on its license. All packages in the tree contain a `LICENSE` entry in their ebuilds. Running emerge --search category/package will show the package's license.

By default, Portage permits licenses that are explicitly approved by the [Free Software Foundation](https://www.gnu.org/licenses/license-list.html), the [Open Source Initiative](https://opensource.org/licenses), or that follow the [Free Software Definition](https://www.gnu.org/philosophy/free-sw.html).

The variable that controls permitted licenses is called `ACCEPT_LICENSE`, which can be set in the /etc/portage/make.conf file. In the next example, this default value is shown:

**`/etc/portage/make.conf`**

**The default`ACCEPT_LICENSE` setting**

```
ACCEPT_LICENSE="-* @FREE"
```
With this configuration, packages with a free software or documentation license will be installable. Non-free software will not be installable.

It is possible to set `ACCEPT_LICENSE` globally in /etc/portage/make.conf, or to specify it on a per-package basis in the /etc/portage/package.license file.

For example, to allow the google-chrome license for the [www-client/google-chrome](https://packages.gentoo.org/packages/www-client/google-chrome) package, add the following to /etc/portage/package.license:

**`/etc/portage/package.license`**

**Accepting the google-chrome license for the google-chrome package**

This permits the installation of the [www-client/google-chrome](https://packages.gentoo.org/packages/www-client/google-chrome) package, but prohibits the installation of the [www-plugins/chrome-binary-plugins](https://packages.gentoo.org/packages/www-plugins/chrome-binary-plugins) package, even though it has the same license.

Or to allow the often-needed [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware):

**`/etc/portage/package.license`**

**Accepting the licenses for the linux-firmware package**

License groups defined in the `ACCEPT_LICENSE` variable are prefixed with an `@` sign. A possible setting  (which was the previous Portage default) is to allow all licenses, except *End User License Agreements (EULAs)* that require reading and signing an acceptance agreement. To accomplish this, accept all licenses (using `*`) and then remove the licenses in the EULA group as follows:

**`/etc/portage/make.conf`**

**Accept all licenses except EULAs**

```
ACCEPT_LICENSE="* -@EULA"
```
Note that this setting will also accept non-free software and documentation.

As stated before, Portage is extremely powerful and supports many features that other software management tools lack. To understand this, we explain a few aspects of Portage without going into too much detail.

With Portage different versions of a single package can coexist on a system. While other distributions tend to name their package to those versions (like gtk+2 and gtk+3) Portage uses a technology called *SLOT*s. An ebuild declares a certain SLOT for its version. Ebuilds with different SLOTs can coexist on the same system. For instance, the gtk+ package has ebuilds with SLOT="2" and SLOT="3".

There are also packages that provide the same functionality but are implemented differently. For instance, metalogd, sysklogd, and syslog-ng are all system loggers. Applications that rely on the availability of "a system logger" cannot depend on, for instance, metalogd, as the other system loggers are as good a choice as any. Portage allows for virtuals: each system logger is listed as an "exclusive" dependency of the logging service in the logger virtual package of the virtual category, so that applications can depend on the [virtual/logger](https://packages.gentoo.org/packages/virtual/logger) package. When installed, the package will pull in the first logging package mentioned in the package, unless a logging package was already installed (in which case the virtual is satisfied).

Software in the Gentoo repository can reside in different branches. By default the system only accepts packages that Gentoo deems stable. Most new software titles, when committed, are added to the testing branch, meaning more testing needs to be done before it is marked as stable. Although the ebuilds for those software are in the Gentoo repository, Portage will not update them before they are placed in the stable branch.

Some software is only available for a few architectures. Or the software doesn't work on the other architectures, or it needs more testing, or the developer that committed the software to the Gentoo repository is unable to verify if the package works on different architectures.

Each Gentoo installation also adheres to a certain profile which contains, amongst other information, the list of packages that are required for a system to function normally.

Ebuilds contain specific fields that inform Portage about its dependencies. There are two possible dependencies: build dependencies, declared in the `DEPEND` variable and run-time dependencies, likewise declared in `RDEPEND`. When one of these dependencies explicitly marks a package or virtual as being not compatible, it triggers a blockage.

While recent versions of Portage are smart enough to work around minor blockages without user intervention, occasionally such blockages need to be resolved manually.

To fix a blockage, users can choose to not install the package or unmerge the conflicting package first. In the given example, one can opt not to install x11-wm/i3 or to remove x11-wm/i3-gaps first. It is usually best to simply tell Portage the package is no longer desired, with emerge --deselect x11-wm/i3-gaps, for example, to remove it from the world file rather than removing the package itself forcefully.

Sometimes there are also blocking packages with specific atoms, such as `<media-video/mplayer-1.0_rc1-r2`. In this case, updating to a more recent version of the blocking package could remove the block.

It is also possible that two packages that are yet to be installed are blocking each other. In this rare case, try to find out why both would need to be installed. In most cases it is sufficient to do with one of the packages alone. If not, please file a bug on [Gentoo's bug tracking system](https://bugs.gentoo.org/).

When trying to install a package that isn't available for the system, this masking error occurs. Users should try installing a different application that is available for the system or wait until the package is marked as available. There is always a reason why a package is masked:

| Reason for mask | Description | 
|---|---|
| \~arch keyword | The application is not tested sufficiently to be put in the stable branch. Wait a few days or weeks and try again. | 
| -arch keyword or -\* keyword | The application does not work on the target architecture. If this is not the case, then please [file a bug](https://bugs.gentoo.org). | 
| missing keyword | The application has not yet been tested on the target architecture. Ask the architecture porting team to test the package or test it for them and report the findings on Gentoo's Bugzilla website. See [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) and [Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package). | 
| package.mask | The package has been found corrupt, unstable or worse and has been deliberately marked as do-not-use. | 
| profile | The package has been found not suitable for the current profile. The application might break the system if it is installed or is just not compatible with the profile currently in use. | 
| license | The package's license is not compatible with the `ACCEPT_LICENSE` value. Permit its license or the right license group by setting it in /etc/portage/make.conf or in [/etc/portage/package.license](https://wiki.gentoo.org/wiki//etc/portage/package.license). | 

The error message might also be displayed as follows, if `--autounmask` isn't set:

Such warning or error occurs when a package is requested for installation which not only depends on another package, but also requires that that package is built with a particular USE flag (or set of USE flags). In the given example, the package app-text/feelings needs to be built with USE="test", but this USE flag is not set on the system.

To resolve this, either add the requested USE flag to the global USE flags in /etc/portage/make.conf, or set it for the specific package in /etc/portage/package.use.

The application to install depends on another package that is not available for the system. Please check Bugzilla if the issue is known and if not, please report it. Unless the system is configured to mix branches, this should not occur and is therefore a bug.

The application that is selected for installation has a name that corresponds with more than one package. Supply the category name as well to resolve this. Portage will inform the user about possible matches to choose from.

Two (or more) packages to install depend on each other and can therefore not be installed. This is most likely a bug in one of the packages in the Gentoo repository. Please re-sync after a while and try again. It might also be beneficial to check [Bugzilla](https://bugs.gentoo.org/) to see if the issue is known and if not, report it.

Portage was unable to download the sources for the given application and will try to continue installing the other applications (if applicable). This failure can be due to a mirror that has not synchronized correctly or because the ebuild points to an incorrect location. The server where the sources reside can also be down for some reason.

Retry after one hour to see if the issue still persists.

The user has asked to remove a package that is part of the system's core packages. It is listed in the profile as required and should therefore not be removed from the system.

This is a sign that something is wrong with the Gentoo repository - often, caused by a mistake made when committing an ebuild to the Gentoo ebuild repository.

When the digest verification fails, do not try to re-digest the package personally. Running ebuild foo manifest will not fix the problem; it quite possibly could make it worse.

Instead, wait an hour or two for the repository to settle down. It is likely that the error was noticed right away, but it can take a little time for the fix to trickle down the rsync mirrors. Check [Bugzilla](https://bugs.gentoo.org/) and see if anyone has reported the problem yet or ask around on [#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)) (IRC). If not, go ahead and file a bug for the broken ebuild.

Once the bug has been fixed, re-sync the Gentoo ebuild repository to pick up the fixed digest.









When installing Gentoo, users make choices depending on the environment they are working with. A setup for a server differs from a setup for a workstation. A gaming workstation differs from a 3D rendering workstation.

This is not only true for choosing what packages to install, but also what features a certain package should support. If there is no need for OpenGL, why would someone bother to install and maintain OpenGL and build OpenGL support in most of the packages? If someone doesn't want to use KDE, why would they bother compiling packages with KDE support if those packages work flawlessly without?

To help users in deciding what to install/activate and what not, Gentoo wanted the user to specify his/her environment in an easy way. This forces the user into deciding what they really want and eases the process for Portage to make useful decisions.

Enter *USE flags*. Such a flag is a keyword that embodies support and dependency-information for a certain concept. If a certain USE flag is set to enabled, then Portage will know the system administrator desires support for the chosen keyword. Of course this may alter the dependency information for a package. Depending on the USE flag, this may require pulling in *many* more dependencies in order to fulfill the requested dependency changes.

Take a look at a specific example: the `kde` USE flag. If this flag is not set in the `USE` variable (or if the value is prefixed with a minus sign: `-kde`), then all packages that have optional KDE support will be compiled *without* KDE support. All packages that have an optional KDE dependency will be installed *without* installing the KDE libraries (as dependency).

When the `kde` flag is set to enabled, then those packages will be compiled *with* KDE support, and the KDE libraries *will* be installed as dependency.

By correctly defining USE flags, the system will be tailored specifically to the needs of the system administrator.

All USE flags are declared inside the `USE` variable. To make it easy for users to search and pick USE flags, we already provide a default USE setting. This setting is a collection of USE flags we think are commonly used by the Gentoo users. This default setting is declared in the make.defaults files that are part of the selected profile.

The profile the system listens to is pointed to by the /etc/portage/make.profile symlink. Each profile works on top of other profiles, and the end result is therefore the sum of all profiles. The top profile is the base profile (/var/db/repos/gentoo/profiles/base).

To view the currently active USE flags (completely), use emerge --info:

`root #``emerge --info | grep ^USE`
USE="a52 aac acpi alsa branding cairo cdr dbus dts ..."

This variable already contains quite a lot of keywords. Do not alter any make.defaults file to tailor the `USE` variable to personal needs though: changes in these files will be undone when the Gentoo repository is updated!

To change this default setting, add or remove keywords to/from the `USE` variable. This is done globally by defining the `USE` variable in /etc/portage/make.conf. In this variable one can add the extra USE flags required, or remove the USE flags that are no longer needed. This latter is done by prefixing the keyword with the minus-sign (`-`).

For instance, to remove support for KDE and Qt but add support for LDAP, the following USE can be defined in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

**Updating USE in make.conf**

```
USE="-kde -qt5 ldap"
```
Sometimes users want to declare a certain USE flag for one (or a couple) of applications but not system-wide. To accomplish this, edit /etc/portage/package.use. package.use is typically a single file, however it can also be a directory filled with children files; see the tip below and then man 5 portage for more information on how to use this convention. The following examples assume package.use is a single file.

For instance, to only have Blu-ray support for the VLC media player package:

**`/etc/portage/package.use`**

**Enabling Blu-ray support for VLC**

Similarly it is possible to explicitly disable USE flags for a certain application. For instance, to disable bzip2 support in PHP (but have it for all other packages through the USE flag declaration in make.conf):

**`/etc/portage/package.use`**

**Disable bzip2 support for PHP**

Sometimes users need to set a USE flag for a brief moment. Instead of editing /etc/portage/make.conf twice (to do and undo the USE changes) just declare the `USE` variable as an environment variable. Remember that this setting only applies for the command entered; re-emerging or updating this application (either explicitly or as part of a system update) will undo the changes that were triggered through the (temporary) USE flag definition.

The following example temporarily removes the `pulseaudio` value from the USE variable during the installation of SeaMonkey:

`root #``USE="-pulseaudio" emerge www-client/seamonkey`
Of course there is a certain precedence on what setting has priority over the USE setting. The precedence for the USE setting is, ordered by priority (first has lowest priority):

1. Default USE setting declared in the make.defaults files part of your profile
2. User-defined USE setting in /etc/portage/make.conf
3. User-defined USE setting in /etc/portage/package.use
4. User-defined USE setting as environment variable

To view the final USE setting as seen by Portage, run emerge --info. This will list all relevant variables (including the `USE` variable) with their current definition as known to Portage.

`root #``emerge --info`
After having altered USE flags, the system should be updated to reflect the necessary changes. To do so, use the `--newuse` option with emerge:

`root #``emerge --update --deep --newuse @world`
Next, run Portage's depclean to remove the conditional dependencies that were emerged on the "old" system but that have been obsoleted by the new USE flags.

When depclean has finished, emerge may prompt to rebuild the applications that are dynamically linked against shared objects provided by possibly removed packages. Portage will preserve necessary libraries until this action is done to prevent breaking applications.  It stores what needs to be rebuilt in the `preserved-rebuild` set. To rebuild the necessary packages, run:

`root #``emerge @preserved-rebuild`
When all this is accomplished, the system is using the new USE flag settings.

Let's take the example of seamonkey: what USE flags does it listen to? To find out, we use emerge with the `--pretend` and `--verbose` options:

`root #``emerge --pretend --verbose www-client/seamonkey````
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
[ebuild  N     ] www-client/seamonkey-2.48_beta1::gentoo  USE="calendar chatzilla crypt dbus gmp-autoupdate ipc jemalloc pulseaudio roaming skia startup-notification -custom-cflags -custom-optimization -debug -gtk3 -jack -minimal (-neon) (-selinux) (-system-cairo) -system-harfbuzz -system-icu -system-jpeg -system-libevent -system-libvpx -system-sqlite {-test} -wifi" L10N="-ca -cs -de -en-GB -es-AR -es-ES -fi -fr -gl -hu -it -ja -lt -nb -nl -pl -pt-PT -ru -sk -sv -tr -uk -zh-CN -zh-TW" 216,860 KiB
 
Total: 1 package (1 new), Size of downloads: 216,860 KiB
```
emerge isn't the only tool for this job. In fact, there is a tool dedicated to package information called equery which resides in the [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package

`root #``emerge --ask app-portage/gentoolkit`
Now run equery with the `uses` argument to view the USE flags of a certain package. For instance, for the [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) package:

`user $``equery --nocolor uses =app-portage/portage-utils-0.93.3`
\[ Legend : U - final flag setting for installation\]
\[        : I - package is installed with flag     \]
\[ Colors : set, unset                             \]
 \* Found these USE flags for app-portage/portage-utils-0.93.3:
 U I
 + + nls       : Add Native Language Support (using gettext - GNU locale utilities)
 + + openmp    : Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp"
 + + qmanifest : Build qmanifest applet, this adds additional dependencies for GPG, OpenSSL and BLAKE2B hashing
 + + qtegrity  : Build qtegrity applet, this adds additional dependencies for OpenSSL
 - - static    : !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically

Some ebuilds require or forbid certain combinations of USE flags in order to work properly. This is expressed via a set of conditions placed in a  `REQUIRED_USE` expression. This conditions ensure that all features and dependencies are complete and that the build will succeed and perform as expected. If any of these are not met, emerge will alert you and ask you to fix the issue.

| Example | Description | 
|---|---|
| `REQUIRED_USE="foo? ( bar )"` | If `foo` is set, `bar` must be set. | 
| `REQUIRED_USE="foo? ( !bar )"` | If `foo` is set, `bar` must not be set. | 
| `REQUIRED_USE="foo? ( \|\| ( bar baz ) )"` | If `foo` is set, `bar` or `baz` must be set. | 
| `REQUIRED_USE="^^ ( foo bar baz )"` | Exactly one of `foo` `bar` or `baz` must be set. | 
| `REQUIRED_USE="\|\| ( foo bar baz )"` | At least one of `foo` `bar` or `baz` must be set. | 
| `REQUIRED_USE="?? ( foo bar baz )"` | No more than one of `foo` `bar` or `baz` may be set. | 







Portage has several additional features that make the Gentoo experience even better. Many of these features rely on certain software tools that improve performance, reliability, security, ...

To enable or disable certain Portage features, edit /etc/portage/make.conf and update or set the `FEATURES` variable which contains the various feature keywords, separated by white space. In several cases it will also be necessary to install the additional tool on which the feature relies.

Not all features that Portage supports are listed here. For a full overview, please consult the make.conf man page:

`user $``man make.conf`
To find out what `FEATURES` are set by default, run emerge --info and search for the `FEATURES` variable or grep it out:

`user $``emerge --info | grep ^FEATURES=`
Gentoo provides a range of prebuilt binary packages known as binpkgs.

These have varying levels of support depending on arch and type, but on amd64 and arm64 there is very large range that supports the following profiles:

- `default/linux/amd64/23.0/no-multilib`
- `default/linux/amd64/23.0/desktop/gnome`
- `default/linux/amd64/23.0/desktop/gnome/systemd`
- `default/linux/amd64/23.0/desktop/plasma/systemd`

Other arches such as HPPA, Gentoo provides the binpkgs created during the stage3 builds to at least give some support.

More infomation on this can be found at [Gentoo Binary Host Quickstart](https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart) article.

To use Gentoo's provided binpkgs then see the section in Handbook on how to enable correctly at [Installation/Base#Optional:\_Adding\_a\_binary\_package\_host](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Optional:_Adding_a_binary_package_host)

To re-verify the integrity and (potentially) re-download previously removed/corrupted distfiles for all currently installed packages, run:

`root #``emerge --ask --fetchonly --emptytree @world`








When the system is booted, lots of text floats by. When paying close attention, one will notice this text is (usually) the same every time the system is rebooted. The sequence of all these actions is called the boot sequence and is (more or less) statically defined.

First, the boot loader will load the kernel image that is defined in the boot loader configuration. Then, the boot loader instructs the CPU to execute the kernel. When the kernel is loaded and run, it initializes all kernel-specific structures and tasks and starts the init process.

The init process makes sure that all filesystems (defined in /etc/fstab) are mounted and ready to be used. Then it executes several scripts located in /etc/init.d/, which will start the services needed in order to have a successfully booted system.

Finally, once all scripts have been executed, init activates the virtual consoles accessible via `Ctrl`+`Alt`+ `F1`, `Ctrl`+`Alt` + `F2`, etc.), attaching to each one a special process called agetty. This process ensures users are able to log on via login.

The scripts in /etc/init.d/ aren't executed randomly. init doesn't run all scripts in /etc/init.d/, only the scripts it is told to execute, via /etc/runlevels/.

First, init runs all scripts from /etc/init.d/ that have symbolic links inside /etc/runlevels/boot/. Usually, it will start the scripts in alphabetical order, but some scripts have dependency information in them, telling the system that another script must be run before they can be started.

Once all scripts referenced in /etc/runlevels/boot/ have been executed, init will run the scripts linked in /etc/runlevels/default/. Again, it will use the alphabetical order to decide what script to run first, unless a script has dependency information in it, in which case the order is changed to provide a valid start-up sequence. This is why commands used during the installation of Gentoo Linux use the `default` runlevel (e.g. `rc-update add sshd default`).

Of course init doesn't decide all that by itself. It needs a configuration file that specifies what actions need to be taken. The /etc/inittab file is used by init to determine the actions it needs to take.

As described above, init's first action is to mount all file systems. This is defined in the following line from /etc/inittab:

**`/etc/inittab`**

**Initialization command**

This line tells init that it must run /sbin/openrc sysinit to initialize the system. The /sbin/openrc script takes care of the initialization, so one might say that init doesn't do much - it delegates the task of initializing the system to another process.

Second, init executed all scripts that had symbolic links in /etc/runlevels/boot/. This is defined in the following line:

**`/etc/inittab`**

**Boot command invocation**

Again the OpenRC script performs the necessary tasks. Note that the option given to OpenRC (boot) is the same as the sub-directory of /etc/runlevels/ that is used.

Now init checks its configuration file to see what runlevel it should run. To decide this, it reads the following line from /etc/inittab:

**`/etc/inittab`**

**Default runlevel selection**

In this case (which the majority of Gentoo users will use), the runlevel id is 3. Using this information, init checks what it must run to start runlevel 3:

**`/etc/inittab`**

**Runlevel definitions**

The line that defines level 3, again uses the openrc script to start the services (now with argument `default`). Also note again that the argument to openrc is the same as the subdirectory from /etc/runlevels/.

When OpenRC has finished, init decides what virtual consoles it should activate and what commands need to be run at each console:

**`/etc/inittab`**

**Terminal definitions**

In a previous section, we saw that init uses a numbering scheme to decide what runlevel it should activate. A runlevel is a state in which the system is running and contains a collection of scripts (runlevel scripts or initscripts) that must be executed when entering or leaving a runlevel.

In Gentoo, there are seven runlevels defined: three internal runlevels, and four user-defined runlevels. The internal runlevels are called *sysinit*, *shutdown* and *reboot* and do exactly what their names imply: initializing the system, powering off the system, and rebooting the system.

The user-defined runlevels are those with an accompanying /etc/runlevels/ subdirectory: *boot*, *default*, *nonetwork* and *single*. The *boot* runlevel starts all system-necessary services used by all the other runlevels. The remaining three runlevels differ in what services they start: *default* is used for day-to-day operations, *nonetwork* is used in case no network connectivity is required, and *single* is used when the system needs to be fixed.

The scripts that the openrc process starts are called *init scripts*. Each script in /etc/init.d/ can be executed with the arguments `start`, `stop`, `restart`, `zap`, `status`, `ineed`, `iuse`, `iwant`, `needsme`, `usesme`, or `wantsme`.

To start, stop, or restart a service (and all dependent services), the `start`, `stop`, and `restart` arguments should be used:

`root #``rc-service postfix start`
To stop a service, but not the services that depend on it, use the `--nodeps` option together with the `stop` argument:

`root #``rc-service --nodeps postfix stop`
To get the status of a service (started, stopped, ...), use the status argument:

`root #``rc-service postfix status`
If the status information shows that the service is running, but in reality it is not, then reset the status information to "stopped" with the `zap` argument:

`root #``rc-service postfix zap`
To also ask what dependencies the service has, use `iwant`, `iuse` or `ineed`. With `ineed` it is possible to see the services that are really necessary for the correct functioning of the service. `iwant` or `iuse`, on the other hand, shows the services that can be used by the service, but are not necessary for the correct functioning of the service.

`root #``rc-service postfix ineed`
Similarly, it is possible to ask what services require the service (`needsme`) or can use it (`usesme` or `wantsme`):

`root #``rc-service postfix needsme`
Gentoo's init system uses a dependency tree to decide what service needs to be started first. As this is a tedious task that we wouldn't want our users to have to do manually, we have created tools that ease the administration of the runlevels and init scripts.

With rc-update it is possible to add and remove init scripts to a runlevel. The rc-update tool will then automatically ask the depscan.sh script to rebuild the dependency tree.

In earlier instructions, init scripts have already been added to the *default* runlevel. What *default* means has been explained earlier in this document. Next to the runlevel, the rc-update script requires a second argument that defines the action: `add`, `del`, or `show`.

In addition to the runlevel, the rc-update script requires a second argument specifying the appropriate action: `add`, `del`, or `show`. For instance:

`root #``rc-update del postfix default`
The rc-update -v show command will show all the available init scripts and the runlevels in which they will execute:

`root #``rc-update -v show`
It is also possible to run rc-update show (without `-v`) to just view enabled init scripts and their runlevels.

Init scripts can be quite complex. It is therefore not desirable to have users edit init scripts directly, as it would make them more error-prone. However, it's important to be able configure services: for instance, users might want to run the service with additional options.

A second reason to have this configuration outside the init script is to be able to update the init scripts without the fear that the user's configuration changes will be undone.

Gentoo provides an easy way to configure such a service: every init script that can be configured has a file in /etc/conf.d/. For instance, the apache2 initscript (called /etc/init.d/apache2) has a configuration file called /etc/conf.d/apache2, which can contain the options to give to the Apache 2 server when it is started:

**`/etc/conf.d/apache2`**

**Example options for apache2 init script**

```
APACHE2_OPTS="-D PHP5"
```
Such a configuration file contains *only* variables (just like /etc/portage/make.conf does), making it very easy to configure services. It also allows us to provide more information about the variables (as comments).

Another useful resource is OpenRC's [service script guide](https://github.com/OpenRC/openrc/blob/master/service-script-guide.md).

No, writing an init script is usually not necessary as Gentoo provides ready-to-use init scripts for all provided services. However, some users might have installed a service without using Portage, in which case they will most likely have to create an init script.

Do not use the init script provided by the service if it isn't explicitly written for Gentoo: Gentoo's init scripts are not compatible with the init scripts used by other distributions, unless the other distribution is using OpenRC.

The basic layout of an init script is shown below.

Every init script requires the `start()` function or `command` variable to be defined. All other sections are optional.

There are three dependency-related settings which can influence the start-up or sequencing of init scripts:: `want`, `use` and `need`. Next to these, there are also two order-influencing methods called `before` and `after`. These last two are not dependencies *per se* - they don't make the init script fail if the specified dependency isn't scheduled to start (or fails to start).

- The `use` setting informs the init system that the script uses functionality offered by the selected script, but does not directly depend on it. Some examples are `use logger` and `use dns`: if the services are available, they will be used, but if the system does not have a logger or DNS server, the services will still work. If the services exist, then they are started before the script that uses them.
- The `want` setting is similar to `use` with one exception. `use` only considers services which were added to a runlevel; `want` will try to start any available service even if not added to any runlevel. `want` will try to start any available service even if not added to an init level.
- The `need` setting indicates a hard dependency: a script that `need`s another script will not start before the latter script is started successfully. Also, if the `need`ed script is restarted, the script needing it will be restarted as well.
- The `before` setting ensures the script is launched before a specified script, if the latter is part of the runlevel. So an init script xdm that defines `before alsasound` will start before the alsasound script, but only if alsasound is scheduled to start in the same runlevel. If alsasound is not scheduled to start in that runlevel, then the `before` has no effect, and xdm will be started when the init system deems it most appropriate.
- Similarly, `after` informs the init system that the given script should be launched after a specified script if the latter is part of the same runlevel. If not, then the setting has no effect and the script will be launched by the init system when it deems it most appropriate.

It should be clear from the above that `need` is the only "true" dependency setting, as it affects whether the script will be started or not. All the others merely tell the init system the order in which scripts can be (or should be) started.

#### Virtual dependencies

Many of Gentoo's init scripts depend on things that are themselves not init scripts: virtual dependencies.

A *virtual dependency* is a dependency that a service provides, but that is not provided solely by that service. An init script can depend on a system logger, but there are many system loggers available (metalogd, syslog-ng, sysklogd, ...). As the script cannot need every single one of them (no sensible system has all these system loggers installed and running) we made sure that all these services provide a virtual dependency.

For instance, consider the dependency information in the postfix script:

**`/etc/init.d/postfix`**

**Dependency information of the postfix service**

```
() {
  need net
  use logger dns
  provide mta
}
```
As can be seen, the postfix service:

- Requires the (virtual) `net` dependency (which is provided by, for instance, /etc/init.d/net.eth0).
- Uses the (virtual) `logger` dependency (which is provided by, for instance, /etc/init.d/syslog-ng).
- Uses the (virtual) `dns` dependency (which is provided by, for instance, /etc/init.d/named).
- Provides the (virtual) `mta` dependency (which is common for all mail servers).

As described in the previous section, it is possible to tell the init system what order it should use for starting (or stopping) scripts. This ordering is handled both through the dependency settings `use` and `need`, but also through the order settings `before` and `after`. As we have described these earlier already, let's take a look at the portmap service as an example of such init script.

**`/etc/init.d/portmap`**

**Dependency information of the portmap service**

```
() {
  need net
  before inetd
  before xinetd
}
```
It's possible to use the `*` glob to refer to all services in the same runlevel, although this isn't advisable.

If the service must write to local disks, it should `need localmount`. If it places anything in /var/run/, such as a PID file, then it should start `after bootmisc`:

In addition to the `depend()` functionality, it's also necessary to define the `start()` function. This function contains all the commands necessary to initialize the service. It's advisable to use the `ebegin` and `eend` functions to inform the user about what's happening:

Both `--exec` and `--pidfile` should be used in `start()` and `stop()` functions. If the service doesn't create a PID file, then use `--make-pidfile` if possible, though it is recommended to test this to be sure. Otherwise, don't use PID files. It is also possible to add `--quiet` to the start-stop-daemon options, but this is not recommended unless the service is extremely verbose. Using `--quiet` may hinder debugging if the service fails to start.

Note also that the above example check the contents of the `RC_CMD` variable. OpenRC doesn't support script-specific restart functionality; instead, the script needs to check the contents of the `RC_CMD` variable to see if a function (e.g. `start()` or `stop()`) is being called as part of a restart or not.

For more examples of the `start()` function, please read the source code of the available init scripts in the /etc/init.d/ directory.

Another function that can (but does not have to) be defined is `stop()`. The init system is intelligent enough to fill in this function by itself if start-stop-daemon is used.

If the service runs some other script (for example, Bash, Python, or Perl), and this script later changes names (for example, from `foo.py` to `foo`), then it is necessary to add `--name` as an option to start-stop-daemon. This must specify the name that the script will be changed to. In this example, a service starts foo.py, which changes names to `foo`:

start-stop-daemon has an excellent man page available if more information is needed:

`user $``man start-stop-daemon`
Gentoo's init script syntax is based on the POSIX shell ('sh'), so people are free to use sh-compatible constructs inside their init scripts. Keep other constructs, like Bash-specific ones, out of init scripts to ensure that the scripts remain functional regardless of any changes Gentoo might make to its init system.

If the initscript needs to support an option other than the ones we've already encountered, add the option to one of the following variables, and create a function with the same name as the option. For instance, to support an option called `restartdelay`:

- `extra_commands` - Command is available with the service in any state
- `extra_started_commands` - Command is available when the service is started
- `extra_stopped_commands` - Command is available when the service is stopped



In order to support configuration files in /etc/conf.d/, no specifics need to be implemented: when the init script is executed, the following files are automatically sourced (i.e. the variables are available to use):

- /etc/conf.d/YOUR\_INIT\_SCRIPT
- /etc/conf.d/basic
- /etc/rc.conf

Also, if the init script provides a virtual dependency (such as `net`), the file associated with that dependency (such as /etc/conf.d/net) will be sourced too.

Many laptop users know the situation: at home they need to start net.eth0, but they don't want to start net.eth0 while on the road (as there is no network available). With Gentoo the runlevel behavior can be altered at will.

For instance, a second "default" runlevel can be created, with other init scripts assigned to it. At boot time, the user can select what "default" runlevel to use.

First of all, create the runlevel directory for the second "default" runlevel. As an example we create the *offline* runlevel:

`root #``mkdir /etc/runlevels/offline`
Add the necessary init scripts to the newly created runlevel. For instance, to have an exact copy of the current default runlevel but without net.eth0:

`root #````
cd /etc/runlevels/default
```
`root #````
for service in *; do rc-update add $service offline; done
```
`root #````
rc-update del net.eth0 offline
```
`root #``rc-update show offline````
(Partial sample Output)
               acpid | offline
          domainname | offline
               local | offline
            net.eth0 |
```
Even though net.eth0 has been removed from the offline runlevel, udev might want to attempt to start any devices it detects and launch the appropriate services, functionality that is called hotplugging. By default, Gentoo does not enable hotplugging.

To enable hotplugging, but only for a selected set of scripts, use the `rc_hotplug` variable in /etc/rc.conf:

**`/etc/rc.conf`**

**Enable hotplugging of the WLAN interface**

```
rc_hotplug="net.wlan !net.*"
```
Edit the bootloader configuration and add a new entry for the offline runlevel. In that entry, add `softlevel=offline` as a boot parameter.

Using bootlevel is completely analogous to softlevel. The only difference here is that a second "boot" runlevel is defined instead of a second "default" runlevel.









An environment variable is a named object that contains information used by one or more applications. By using environment variables one can easily change a configuration setting for one or more applications.

The following table lists a number of variables used by a Linux system and describes their use. Example values are presented after the table.

| Variable | Description | 
|---|---|
| `PATH` | This variable contains a colon-separated list of directories in which the system looks for executable files. If a name is entered of an executable (such as ls, rc-update, or emerge) but this executable is not located in a listed directory, then the system will not execute it (unless the full path is entered as the command, such as /bin/ls). | 
| `ROOTPATH` | This variable has the same function as `PATH`, but this one only lists the directories that should be checked when the root-user enters a command. | 
| `LDPATH` | This variable contains a colon-separated list of directories which the dynamic linker searches to find a library. | 
| `MANPATH` | This variable contains a colon-separated list of directories which the [man(1)](https://man.archlinux.org/man/man.1.en) [command searches for man pages.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| `INFODIR` | This variable contains a colon-separated list of directories which the [info(1)](https://man.archlinux.org/man/info.1.en) [command searches for info pages.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| `PAGER` | This variable contains the path to the program used to list the contents of files (such as [less](https://wiki.gentoo.org/wiki/Less) or [more(1)](https://man.archlinux.org/man/more.1.en)[).](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| `EDITOR` | This variable contains the path to the program used to edit files (such as [nano](https://wiki.gentoo.org/wiki/Nano) or [vi](https://wiki.gentoo.org/wiki/Vi)). | 
| `KDEDIRS` | This variable contains a colon-separated list of directories which contain KDE-specific material. | 
| `CONFIG_PROTECT` | This variable contains a space-delimited list of directories which should be protected by Portage during package updates. | 
| `CONFIG_PROTECT_MASK` | This variable contains a space-delimited list of directories which should *not* be protected by Portage during package updates. | 

Below is an example definition of all these variables:

To centralize the definitions of these variables, Gentoo introduced the /etc/env.d/ directory. Inside this directory a number of files are available, such as 50baselayout, gcc/config-x86\_64-pc-linux-gnu, etc. which contain the variables needed by the application mentioned in their name.

For instance, when gcc is installed, a file called gcc/config-x86\_64-pc-linux-gnu was created by the ebuild which contains the definitions of the following variables:

**`/etc/env.d/gcc/config-x86_64-pc-linux-gnu`**

**Default gcc enabled environment variables for GCC 13**

```
GCC_PATH="/usr/x86_64-pc-linux-gnu/gcc-bin/13"
LDPATH="/usr/lib/gcc/x86_64-pc-linux-gnu/13:/usr/lib/gcc/x86_64-pc-linux-gnu/13/32"
MANPATH="/usr/share/gcc-data/x86_64-pc-linux-gnu/13/man"
INFOPATH="/usr/share/gcc-data/x86_64-pc-linux-gnu/13/info"
STDCXX_INCDIR="g++-v13"
CTARGET="x86_64-pc-linux-gnu"
GCC_SPECS=""
MULTIOSDIRS="../lib64:../lib"
```
Other distributions might tell the system administrator to change or add such environment variable definitions in /etc/profile or other locations. Gentoo on the other hand makes it easy for the sysadmins (and for Portage) to maintain and manage the environment variables without having to pay attention to the numerous files that can contain environment variables.

For instance, when gcc is updated, the associated file(s) under /etc/env.d/gcc are updated too without requesting any administrative interaction.

There are still occasions where a system administrator is asked to set a certain environment variable system-wide. As an example, take the `http_proxy` variable. Instead of editing a file under the /etc/profile directory, create a file named /etc/env.d/99local and enter the definition in it:

**`/etc/env.d/99local`**

**Setting a global environment variable**

```
http_proxy="proxy.server.com:8080"
```
By using the same file for all customized environment variables, system administrators have a quick overview on the variables they have defined themselves.

Several files within the /etc/env.d directory add definitions to the `PATH` variable. This is not a mistake: when the env-update command is executed, it will append the several definitions before it atomically updates each environment variable, thereby making it easy for packages (or system administrators) to add their own environment variable settings without interfering with the already existing values.

The env-update script will append the values in the alphabetical order of the /etc/env.d/ files. The file names must begin with two decimal digits.

The concatenation of variables does not always happen, only with the following variables: `ADA_INCLUDE_PATH`, `ADA_OBJECTS_PATH`, `CLASSPATH`, `KDEDIRS`, `PATH`, `LDPATH`, `MANPATH`, `INFODIR`, `INFOPATH`, `ROOTPATH`, `CONFIG_PROTECT`, `CONFIG_PROTECT_MASK`, `PRELINK_PATH`, `PRELINK_PATH_MASK`, `PKG_CONFIG_PATH`, and `PYTHONPATH`. For all other variables the latest defined value (in alphabetical order of the files in /etc/env.d/) is used.

It is possible to add more variables into this list of concatenate-variables by adding the variable name to either `COLON_SEPARATED` or `SPACE_SEPARATED` variables (also inside an /etc/env.d/ file).

When executing env-update, the script will create all environment variables and place them in /etc/profile.env (which is used by /etc/profile). It will also extract the information from the `LDPATH` variable and use that to create /etc/ld.so.conf. After this, it will run ldconfig to recreate the /etc/ld.so.cache file used by the dynamical linker.

To notice the effect of env-update immediately after running it, execute the following command to update the environment. Users who have installed Gentoo themselves will probably remember this from the installation instructions:

`root #``env-update && source /etc/profile`
It might not be necessary to define an environment variable globally. For instance, one might want to add /home/my\_user/bin and the current working directory (the directory the user is in) to the `PATH` variable but not want all other users on the system to have those directories in their `PATH`. To define an environment variable locally, use \~/.bashrc (for all interactive shell sessions) or \~/.bash\_profile (for login shell sessions):

**`~/.bashrc`**

**Extending PATH for local usage**

```
# A colon followed by no directory is treated as the current working directory
PATH="${PATH}:/home/my_user/bin:"
```
After logout/login, the `PATH` variable will be updated.

Sometimes even stricter definitions are required. For instance, a user might want to be able to use binaries from a temporary directory without needing to use the full path to the binaries and without needing to temporarily change \~/.bashrc.

In this case, just define the `PATH` variable in the current session by using the export command. As long as the user does not log out, the `PATH` variable will be using the temporary settings.

`root #``export PATH="${PATH}:/home/my_user/tmp/usr/bin"`
