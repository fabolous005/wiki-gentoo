<!-- source: https://wiki.gentoo.org/wiki/Login | group: Gentoo Wiki (Main) | wiki-title: Login -->
---
title: Login
url: https://wiki.gentoo.org/wiki/Login
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-13"
fingerprint: "1273ba4813850b9f"
license: CC BY-SA 4.0
---

# Login

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article covers logging in to a shell, and setting up the default environment. See [display manager](https://wiki.gentoo.org/wiki/Display_manager) for graphical logins.

Login consists of several steps:

1. Authentication - See [Category:Authentication](https://wiki.gentoo.org/wiki/Category:Authentication)
2. Provision of services with privilege - [PAM](https://wiki.gentoo.org/wiki/PAM) organizes services, and also works in several other cases.
3. Login shell invocation

On effective [shell](https://wiki.gentoo.org/wiki/Shell) invocation, the UID is set to that of the logged in user, then any shell initialization scripts are invoked, as described below.

## Login shell startup scripts and environment variables

One important job of a login shell is to set system-wide environment variables. In Gentoo this is done:

- for [Bash](https://wiki.gentoo.org/wiki/Bash) and variants, by reading /etc/profile.env file
- for csh/tcsh, by reading /etc/csh.env file.

The following sections concentrate on Gentoo login shells. For general information about login shells in Unix, see the [#external links](https://wiki.gentoo.org#external_links).

## Bash and variants - default

For Bash and variants, starting a login shell sources (executes in current process, conserving environment) configuration scripts, e.g. /etc/profile, among others. These configuration files set environment variables, and perform some setup. These settings will be inherited by other shells, say opened in [terminal emulators](https://wiki.gentoo.org/wiki/Terminal_emulator).

A bash login shell reads, in order:

- /etc/profile
- /etc/profile.env
- Any files under the /etc/profile.d/ directory with a .sh suffix.

/etc/bash/bashrc is also sourced - this does *not* configure Gentoo itself, but provides the Gentoo-default bash look-and-feel.

### System wide config files for Bash

- /etc/profile
- Provided by [sys-apps/baselayout](https://packages.gentoo.org/packages/sys-apps/baselayout) (or alternatives). Other than reading other files, this file doesn't do many jobs.

- /etc/profile.env
- This file sets environment variables. Some packages require environment variables to be set, and in Gentoo, such environment variables are collected here.
- This file is generated from files /etc/env.d/\*. These files are installed by some packages (or created by config tools like eselect). For example /etc/env.d/30gnupg is provided by [app-crypt/gnupg](https://packages.gentoo.org/packages/app-crypt/gnupg).
- This file is usually automatically updated.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, but can also be manually updated by calling env-update.
- For details of how /etc/profile.d is generated, see Handbook -> Environment variables -> section "[Defining variables globally](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/EnvVar#Defining_variables_globally)".

- /etc/profile.d/\*.sh
- Some packages install files here, to be sourced by the login shell, to provide per-package configurations . It is a Unix custom, and part of the Linux Standard Base, 5.0 (2015).<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>

## Tcsh

When tcsh is run as the login shell, it reads one system-wide configuration file, /etc/csh.login, which then sources other system-wide configuration files. These configuration files set environment variables, and do some other setup.  These settings will be inherited by other shells, say opened in [terminal emulators](https://wiki.gentoo.org/wiki/Terminal_emulator).

A tcsh login shell  also reads /etc/csh.cshrc, which provides some tcsh-specific default behavior, but does *not* configure Gentoo itself.

A tcsh login shell reads files in the following order:

- /etc/csh.login
- /etc/csh.env
- Any files under the /etc/profile.d/ directory with a .csh suffix.

### System wide config files for tcsh

- /etc/csh.login
- Provided by [app-shells/tcsh](https://packages.gentoo.org/packages/app-shells/tcsh). (This file is distributed by Gentoo, *not* by the tcsh upstream.)

- /etc/csh.env
- This file corresponds to /etc/profile.env for bash, explained above. This file is also maintained by env-update, even when no csh, or variant, is installed.

- /etc/profile.d/\*.csh
- Similar to /etc/profile.d/\*.sh for bash explained above, but there is no standard for `*.csh`.

## Fish

As stated in the article [fish](https://wiki.gentoo.org/wiki/Fish), fish can't load system-wide configuration files mentioned above. To use fish as a user's default shell, there is a good workaround detailed in the fish article.

## See also

- [Display manager](https://wiki.gentoo.org/wiki/Display_manager) — presents the user with a graphical login screen to start a GUI session, either [X](https://wiki.gentoo.org/wiki/Xorg) or [Wayland](https://wiki.gentoo.org/wiki/Wayland).
- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).

## External links

- [login shell](https://wiki.archlinux.org/index.php/Login_shell) - ArchLinux Wiki article, for more general aspects of Unix login shells.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) More precisely, if there's any change in the directory /etc/env.d, `emerge` will re-generate /etc/profile.env. (*Any* change will be caught; it is not limited to changes done by ebuild-helper-commands `doenvd` or `newenvd`.) Same can be expected for Gentoo official tools like eselect.
2. [↑](https://wiki.gentoo.org#cite_ref-2) See [Linux Standard Base Core Specification, Generic Part, 5.0 Edition](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/book1.html)
