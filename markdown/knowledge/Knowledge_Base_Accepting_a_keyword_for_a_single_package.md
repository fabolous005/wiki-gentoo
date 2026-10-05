<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Accepting_a_keyword_for_a_single_package -->
---
title: Knowledge Base:Accepting a keyword for a single package
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: "9fa13e6e8ca3950b"
license: CC BY-SA 4.0
---

# Knowledge Base:Accepting a keyword for a single package

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Not all software present in the [Gentoo repository](https://wiki.gentoo.org/wiki/Ebuild_repository) will be immediately available to install with the [emerge](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Installing_software) command. Software that is still undergoing testing (also referred to as stabilization) is hidden by a keyword and needs to be explicitly accepted in order to be installed (except when [using the testing branch system-wide](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_all_packages)). Beware that using software from the [testing branch](https://wiki.gentoo.org/wiki/Handbook:X86/Portage/Branches#Testing) might incur stability issues, it is not recommended for inexperienced users.

This guide is focused on a particular subject, for complete and precise information on working with [Portage](https://wiki.gentoo.org/wiki/Portage), see the [emerge man page](https://dev.gentoo.org/~zmedico/portage/doc/man/emerge.1.html) and the [Gentoo handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage).


## Trying to emerge a package from the testing branch

On trying to install a package from the testing branch of the Gentoo repository, the [Portage](https://wiki.gentoo.org/wiki/Portage) emerge command will quit after issuing a warning. For example, when trying to emerge `games-kids/gcompris` that currently only has versions in the testing branch:

`root #``emerge --ask games-kids/gcompris`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
 
!!! All ebuilds that could satisfy "games-kids/gcompris" have been masked.
!!! One of the following masked packages is required to complete your request:
- games-kids/gcompris-1.0::gentoo (masked by: \~amd64 keyword)
 
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.

Portage found that the only way it could honor the request would be by installing the testing branch ("\~\<arch>") version of the requested package. In the example above, the testing branch uses the **\~amd64** keyword since the example system uses the **amd64** [architecture](https://wiki.gentoo.org/wiki/Handbook:Main_Page#Architectures) (x86\_64).

Portage will warn that a testing branch package is being requested on trying to emerge a package for which there is no stable branch version but for which there exists a testing branch version in the Gentoo repository. It will also warn if a specific version of a package is requested but that version of the package is in the testing branch.

The same error will be issued if a dependency of a requested package is only available in the testing branch.


## How to make packages from the testing branch available

Packages from the [stable branch](https://wiki.gentoo.org/wiki/Handbook:X86/Portage/Branches#Stable) should always be preferred, if practical. Users should be prepared for the risk of instability when using packages from the testing branch, as such new users are discouraged from installing packages from testing.

If bugs are encountered, it is encouraged that they be [properly reported in the correct place](https://wiki.gentoo.org/wiki/Bugzilla/Guide).


### Using --autounmask-write emerge command option

Probably the easiest method to install packages from the testing branch.

Run the emerge command with `--autounmask=y --autounmask-write` to queue the [text file alteration](https://wiki.gentoo.org/wiki/Knowledge_Base:Autounmask-write) required to unhide a package from the testing branch, for example:

`root #``emerge --ask --autounmask=y --autounmask-write games-kids/gcompris`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
\[...\]
\[ebuild  N    \~\] games-kids/gcompris-1.0  USE="handbook -debug -kiosk -test" 
 
The following keyword changes are necessary to proceed:
 (see "package.accept\_keywords" in the portage(5) man page for more details)
# required by games-kids/gcompris (argument)
=games-kids/gcompris-1.0 \~amd64
 
Would you like to add these changes to your config files? \[Yes/No\] y
 
Autounmask changes successfully written.
 
 \* IMPORTANT: 2 config files in '/etc/portage' need updating.
 \* See the CONFIGURATION FILES and CONFIGURATION FILES UPDATE TOOLS
 \* sections of the emerge man page to learn how to update config files.

Note that the tilde ("\~") in the section in front of the package name indicates that the package is in the testing branch.

To review and merge changes to the protected files that the previous command modified, run [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf)

`root #``dispatch-conf`
--- /etc/portage/package.accept\_keywords        2021-04-01 09:12 +0200
+++ /etc/portage/.\_cfg0000\_package.accept\_keywords      2021-04-24 00:29 +0200
@@ -11,3 +11,5 @@
\[...\]
+# required by games-kids/gcompris (argument)
+=games-kids/gcompris-1.0 \~amd64
 
>> (1 of 2) -- /etc/portage/package.accept\_keywords
>> q quit, h help, n next, e edit-new, z zap-new, u use-new
   m merge, t toggle-merge, l look-merge:

Depending on system configuration, changes may be autocommited, or a pager screen could be shown which will have to be closed to continue (`q` in the default pager, less). If the changes are not autocomitted, dispatch-conf will ask for an action. If the proposed modifications are correct, press `u` to commit the changes.

See the [handbook entry on dispatch-conf](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Tools#dispatch-conf) for detailed information.

Once the autounmask changes have been reviewed and merged, the package should be available to Portage and can be installed:

`root #``emerge --ask games-kids/gcompris`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
\[...\]
\[ebuild  N    \~\] games-kids/gcompris-1.0  USE="handbook -debug -kiosk -test" 
 
Would you like to merge these packages? \[Yes/No\]


### Manual configuration

Packages can be made available by writing to Portage configuration files. According to user preference, packages from the testing branch can be made available to be installed with the emerge command autounmask options, as we have seen, or by writing configuration options either to a flat file in the portage configuration directory or by having a directory in place of that file for organising the configuration into individual files.


#### Flat file

Having one file with entries for each package may offer more flexibility to some users than using the autounmask option.

If the /etc/portage directory does not exist yet, it may need to be created first:

`root #``mkdir --parents /etc/portage`
Add the appropriate line to make the desired package available, for example, to install the games-kids/gcompris package from the unstable branch:

`root #``echo "games-kids/gcompris ~amd64" >> /etc/portage/package.accept_keywords`
To install a specific [version of a package](https://wiki.gentoo.org/wiki/Version_specifier):

`root #``echo "=games-kids/gcompris-1.0 ~amd64" >> /etc/portage/package.accept_keywords`

#### Using directories

Having a directory to organize the configuration can offer other benefits in flexibility, for example.

To organise the configuration in separate files, create /etc/portage/package.accept\_keywords as a directory, rather than as a file. If it does not exist yet:

`root #``mkdir --parents /etc/portage/package.accept_keywords`
As an example of how to use the directory structure as a way to organize configuration, say the Term-ScreenColor package is to be installed but it has the Term-Screen package as a dependency but the Term-Screen package is only available in the testing repository.

A file named Term-ScreenColor is used to contain the configuration directive unhiding the Term-Screen package, as a reference as to why the Term-Screen was made available to install from testing:

`root #``echo "=dev-perl/Term-Screen-1.30.0 ~amd64" >> /etc/portage/package.accept_keywords/Term-ScreenColor`

#### Additional notes

To also let in `-r#` package revisions which may contain immediately beneficial updates, replace the leading `=` (equals sign) with a `~` (tilde):

`~dev-perl/Term-Screen-1.30.0 ~amd64`

The keyword itself can usually be omitted:

`~dev-perl/Term-Screen-1.30.0`

To always install the latest testing version, drop the leading `=` or `~` as well as the version at the end:

`dev-perl/Term-Screen`

If the mask is because no `KEYWORDS` are defined, use the `**` notation:

`~sys-apps/portage-9999 **`


## How packages are marked as being in the testing branch

Every Gentoo ebuild (package) has a set of `[KEYWORDS](https://wiki.gentoo.org/wiki/KEYWORDS)` values that are referenced when it compiles. These keywords identify for which architectures the ebuild is installable, and if the ebuild is stable (no prefix) or still in testing phase (prefixed by `~`).

In certain cases, the package is defined with no `KEYWORDS` content. In this specific case, Portage will suggest to use the special `**` keyword value to override.


## See also

- [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS)
- [KEYWORDS](https://wiki.gentoo.org/wiki/KEYWORDS) — the `KEYWORDS` variable informs in which [architectures](https://wiki.gentoo.org/wiki/Handbook:Main_Page#Architectures) the ebuild is stable or still in testing phase.
- [Knowledge Base:Accepting a keyword for all packages](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_all_packages)
