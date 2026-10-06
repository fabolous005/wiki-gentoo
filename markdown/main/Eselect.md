<!-- source: https://wiki.gentoo.org/wiki/Eselect | group: Gentoo Wiki (Main) | wiki-title: Eselect -->
---
title: eselect
url: https://wiki.gentoo.org/wiki/Eselect
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-19"
categories: ['installed from the Gentoo repository', 'Additional modules']
fingerprint: fe0552663c82bfc0
license: CC BY-SA 4.0
---

# eselect

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**eselect** is a tool for administration and configuration on Gentoo systems. It modifies the system's behavior and should be used with care by the system administrator. eselect  is a modular framework for writing configuration utilities, consisting of:

- a main program named eselect (found in [app-admin/eselect](https://packages.gentoo.org/packages/app-admin/eselect))
- various modules (\*.eselect files) which carry out different tasks
- several libraries which help to ensure consistent behavior and simplify the creation of new modules


A module provides several actions. Actions typically either display some information (list and show actions are common) or update the system somehow (for example, set and update). Each module also provides help and usage actions which explain how to use the module.

To list all currently installed modules, run eselect without any arguments. See also the [eselect user guide](https://wiki.gentoo.org/wiki/Project:Eselect/User_guide).

## Installation

Deployments of Gentoo Linux should already have eselect installed, as the [app-admin/eselect](https://packages.gentoo.org/packages/app-admin/eselect) package is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>).

### USE flags


### USE flags for
            [app-admin/eselect](https://packages.gentoo.org/packages/app-admin/eselect)
            
            Gentoo's multi-purpose configuration and management tool

| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [vim-syntax](https://packages.gentoo.org/useflags/vim-syntax) | Pulls in related vim syntax scripts | 

### Emerge

#### After USE flag changes

After changing USE flags [just for the eselect package](https://wiki.gentoo.org/wiki//etc/portage/package.use), rebuild eselect for the new flags to be applied. As eselect is in the system set, `--oneshot` should be used to avoid adding it to the [world file](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>):

`root #``emerge --ask --changed-use --oneshot app-admin/eselect`
After changing any global USE flags in [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) that affect the eselect package, emerge world to update to the new USE flags:

`root #``emerge --ask --verbose --update --deep --newuse @world`
#### Modules

Install an eselect module just like any other package. For example, for the [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) module:

`root #``emerge --ask --verbose app-eselect/eselect-repository`
Some modules will be pulled in when installing packages that depend on them.

See the [modules section](https://wiki.gentoo.org/wiki/Eselect#Modules_2) for information on finding optional modules for installation.

## Invocation

Just running eselect, or eselect help for a little more information, will provide usage information and list all installed modules and their function:

`root #``eselect help````
Usage: eselect <global options> <module name> <module options>
Global options:
  --brief                   Make output shorter
  --colour=<yes|no|auto>    Enable or disable colour output (default 'auto')
Built-in modules:
  help                      Display a help message
  usage                     Display a usage message
  version                   Display version information
Extra modules:
  arptables                 Manage the iptables/arptables/ebtables symlinks
  binutils                  Manage installed versions of sys-devel/binutils
  cdparanoia                Manage /usr/bin/cdparanoia implementation
  ctags                     Manage /usr/bin/ctags implementations
  ebtables                  Manage the iptables/arptables/ebtables symlinks
  editor                    Manage the EDITOR environment variable
  emacs                     Manage /usr/bin/emacs version
  env                       Manage environment variables set in /etc/env.d/
  etags                     Manage /usr/bin/etags implementations
  fontconfig                Manage fontconfig /etc/fonts/conf.d/ symlinks
  gcc                       Manage installed versions of sys-devel/gcc
  gnuclient                 Manage /usr/bin/gnuclient implementations
  iptables                  Manage the iptables/arptables/ebtables symlinks
  kernel                    Manage the /usr/src/linux symlink
  locale                    Manage the LANG environment variable
  lua                       Manage lua symlinks
  modules                   Query eselect modules
  mpg123                    Manage /usr/bin/mpg123 implementation
  news                      Read Gentoo ("GLEP 42") news items
  notify-send               Manage /usr/bin/notify-send implementation
  pager                     Manage the PAGER environment variable
  pinentry                  Manage /usr/bin/pinentry implementation
  postgresql                Manage active PostgreSQL client applications and libraries
  profile                   Manage the make.profile symlink
  rc                        Manage /etc/init.d scripts in runlevels
  repository                Manage repository list in repos.conf
  rust                      Manage the Rust compiler versions
  sh                        Manage /bin/sh (POSIX shell) implementations
  vi                        Manage /usr/bin/vi implementations
  visual                    Manage the VISUAL environment variable
  wxwidgets                 Manage the system default wxWidgets profile
```
## Modules

Gentoo has tens of eselect modules available, to automate various system configuration tasks. Several modules allow to select between optional subsytems, such as which installed vi editor package to use or which emacs distribution to be default.

Some modules are [shipped by default](https://gitweb.gentoo.org/proj/eselect.git/tree/modules) with eselect, while other modules can be [installed from the Gentoo repository](https://packages.gentoo.org/categories/app-eselect).

To list all modules currently installed, run eselect without any arguments.

### Default modules

These modules are maintained as part of the [eselect project](https://wiki.gentoo.org/wiki/Project:Eselect), so they should be available as long as the [app-admin/eselect](https://packages.gentoo.org/packages/app-admin/eselect) package is installed.

#### Arptables, iptables, and ebtables

Manage the iptables/arptables/ebtables symlinks. Because [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2) is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), [app-eselect/eselect-iptables](https://packages.gentoo.org/packages/app-eselect/eselect-iptables) is often pulled in by the dependency graph, to provide these modules.

`user $``eselect arptables help````
Manage the iptables/arptables/ebtables symlinks
Usage: eselect arptables <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List available arptables symlink targets
  set                       Set a new arptables symlink target
    target                    Target name or number (from 'list' action)
  show                      Show the current arptables symlink
  unset                     Unset arptables symlink targets
```
#### Binutils

Manage installed versions of [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils). This module is not provided as part of the [app-admin/eselect](https://packages.gentoo.org/packages/app-admin/eselect) package, but is from [sys-devel/binutils-config](https://packages.gentoo.org/packages/sys-devel/binutils-config), a dependency of [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils). As [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils) is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), eselect binutils should be available on all Gentoo systems.

`user $``eselect binutils help````
Manage installed versions of sys-devel/binutils
Usage: eselect binutils <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List all installed version of binutils
  set <target>              Activate one of the installed binutils
    target                    Target name or number (from 'list' action)
  show                      Print the currently active binutils version
```
#### Editor

Manage the `EDITOR` environment variable, this determines what many CLI programs will run to edit text files.

See also the [visual module](https://wiki.gentoo.org/wiki/Eselect#Visual) section.

`user $``eselect editor help`
`Manage the EDITOR environment variable`

Usage: eselect editor \<action> \<options>

Standard actions:

help                      Display help text
 usage                     Display usage information
 version                   Display version information

Extra actions:

list                      List available targets for the EDITOR variable
 set \<target>              Set the EDITOR variable in profile
   target                    Target name or number (from 'list' action)
 show                      Show value of the EDITOR variable in profile
 update                    Update the EDITOR variable if it is unset or invalid

#### Env

Manage environment variables set in [/etc/env.d](https://wiki.gentoo.org/wiki//etc/env.d).

`user $``eselect env help````
Manage environment variables set in /etc/env.d/
Usage: eselect env <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  update <noldconfig>       Collect environment variables from all scripts in /etc/env.d/
    noldconfig                Do not alter the ld.so cache or configuration.
```
#### Gcc

Manage installed versions of sys-devel/gcc. Because [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), [sys-devel/gcc-config](https://packages.gentoo.org/packages/sys-devel/gcc-config) will be pulled in, providing this module.

`user $``eselect gcc help````
Manage installed versions of sys-devel/gcc
Usage: eselect gcc <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List all installed version of gcc
  set <target>              Activate one of the installed gcc
    target                    Target name or number (from 'list' action)
  show                      Print the currently active gcc version
```
See also [upgrading GCC](https://wiki.gentoo.org/wiki/Upgrading_GCC#Quick_guide_to_GCC_upgrades).

#### Kernel

The /usr/src/linux symlink should point to the currently running kernel, this can be done with eselect.

`user $``eselect kernel help````
Manage the /usr/src/linux symlink
Usage: eselect kernel <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List available kernel symlink targets
  set <target>              Set a new kernel symlink target
    target                    Target name or number (from 'list' action)
  show                      Show the current kernel symlink
```
See [setting the kernel link with eselect](https://wiki.gentoo.org/wiki/Kernel/Upgrade#Default:_Setting_the_link_with_eselect).

#### Locale

Manage the `LANG` environment variable, this sets the system language for users, date formats etc.

`user $``eselect locale help````
Manage the LANG environment variable
Usage: eselect locale <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List available targets for the LANG variable
  set <target>              Set the LANG variable in profile
    target                    Target name or number (from 'list' action)
  show                      Show value of the LANG variable in profile
```
See [the handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Locale_selection), [localization guide](https://wiki.gentoo.org/wiki/Localization/Guide#OpenRC) and [UTF-8 article](https://wiki.gentoo.org/wiki/UTF-8#Alternatively.2C_using_eselect_to_set_locales).

#### Modules

Query eselect modules.

`user $``eselect modules help````
Query eselect modules
Usage: eselect modules <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  has <module>              Return true if the module is installed, and false otherwise
  list                      List all available modules
    --only-names              Output names of modules only
```
#### News

Read Gentoo ("GLEP 42") [news items](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items). It is ***important to read and follow these***, when they are shown after a [Gentoo ebuild repository synchronization](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization).

`user $``eselect news help````
Read Gentoo ("GLEP 42") news items
Usage: eselect news <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  count                     Display number of news items
    new                       Count unread news items (default)
    all                       Count all news items
  list                      List news items
  purge                     Purge read news
  read <item>...            Read news items
    --mbox                    Output in mbox format
    --quiet                   Suppress output, only change status
    --raw                     Output in raw format
    new                       Read unread news items (default)
    all                       Read all news items
    item                      Item name or number (from 'list' action)
  unread <item>...          Mark read news items as unread again
    all                       Mark all news items as unread
    item                      Item name or number (from 'list' action)
```
#### Pager

Manage the `PAGER` environment variable, this will influence what programs will use to display pages of text to the user.

`user $``eselect pager help````
Manage the PAGER environment variable
Usage: eselect pager <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List available targets for the PAGER variable
  set <target>              Set the PAGER variable in profile
    target                    Target name or number (from 'list' action)
  show                      Show value of the PAGER variable in profile
  update                    Update the PAGER variable if it is unset or invalid
```
#### Profile

Manage the [make.profile](https://wiki.gentoo.org/wiki//etc/portage/make.profile) symlink, this is an important configuration item for Portage, and for the whole system.

`user $``eselect profile help````
Manage the make.profile symlink
Usage: eselect profile <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List available profile symlink targets
  set <target>              Set a new profile symlink target
    target                    Target name or number (from 'list' action)
    --force                   Forcibly set the symlink
  show                      Show the current make.profile symlink
```
See [handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Choosing_the_right_profile), and the article about [switching profiles](https://wiki.gentoo.org/wiki/Portage/Profiles/Switching_profiles).

#### Rc

The eselect rc module, and thus its corresponding rc-config command, are deprecated as of version 1.4.30 (2025-03-22).

This module was used to manage [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) scripts, the rc-update, rc-service or rc-status commands can be used instead.

#### Visual

Manage the `VISUAL` environment variable, to set the default text editor for capable terminals.

See also the [editor module](https://wiki.gentoo.org/wiki/Eselect#Editor) section.

`user $``eselect visual help````
Manage the VISUAL environment variable
Usage: eselect visual <action> <options>
Standard actions:
  help                      Display help text
  usage                     Display usage information
  version                   Display version information
Extra actions:
  list                      List available targets for the VISUAL variable
  set <target>              Set the VISUAL variable in profile
    target                    Target name or number (from 'list' action)
  show                      Show value of the VISUAL variable in profile
  update                    Update the VISUAL variable if it is unset or invalid
```
### Additional modules

[Additional modules](https://packages.gentoo.org/categories/app-eselect) for eselect are available from the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

Some of these modules will be pulled-in as dependencies when associated packages are installed, so it may not be required to emerge these specifically. Once installed, these modules can be used with eselect \<module> commands, similarly to the default modules.

Here are some of the modules documented in the wiki:

- [eselect bashcomp](https://wiki.gentoo.org/wiki/Bash#Shell_completion) - enable or disable specific completions.
- [eselect blas / eselect lapack](https://wiki.gentoo.org/wiki/Blas-lapack-switch#Enabling_the_feature) - BLAS/LAPACK switching: numerical linear algebra libraries.
- [eselect emacs](https://wiki.gentoo.org/wiki/GNU_Emacs#Several_versions_side-by-side) - link /usr/bin/emacs and its auxiliary programs to the ones belonging to the desired Emacs version.
- [eselect fontconfig](https://wiki.gentoo.org/wiki/Fontconfig#Gentoo_specific) - manage symlinks of files in /etc/fonts/conf.avail/.
- [eselect gnome-shell-extensions](https://wiki.gentoo.org/wiki/Project:GNOME/GNOME3_upgrade_guide#General_configurability_and_extensions) -  manage system defaults that control whether Gnome shell extensions were installed system-wide.
- [eselect java-vm](https://wiki.gentoo.org/wiki/Java#Setting_a_default) - set default Java VM.
- [eselect php cli](https://wiki.gentoo.org/wiki/PHP#Running_multiple_versions_of_PHP) - select which CLI PHP version to use by default.
- [eselect php apache2](https://wiki.gentoo.org/wiki/Apache#Modify_PHP_versions) - change which version of PHP is used by Apache.
- [eselect pinentry](https://wiki.gentoo.org/wiki/GnuPG#Pinentry) - choose between pinentry windows.
- [eselect python](https://wiki.gentoo.org/wiki/Project:Python/python-exec#eselect-python) - configure preferred/active Python interpreters.
- [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) — an [eselect] module for configuring [ebuild repositories](https://wiki.gentoo.org/wiki/Ebuild_repository) for [Portage](https://wiki.gentoo.org/wiki/Portage).
- [eselect ruby](https://wiki.gentoo.org/wiki/Rails#Configuration) - change the selected Ruby slot.
- [eselect vi](https://wiki.gentoo.org/wiki/Vi#.2Fusr.2Fbin.2Fvi_symlink) - mange the  /usr/bin/vi symlink.

## See also

- [Project:Eselect/User guide](https://wiki.gentoo.org/wiki/Project:Eselect/User_guide) — a modular framework for writing configuration utilities.
- [Project:Eselect/Developer guide](https://wiki.gentoo.org/wiki/Project:Eselect/Developer_guide) — a framework for simplifying and introducing consistency to the various foo-config and blah-update tools.
- [Project:Base/Alternatives](https://wiki.gentoo.org/wiki/Project:Base/Alternatives)
- [app-alternatives/sh guide](https://wiki.gentoo.org/wiki/Shell#Changing_the_default_system_shell) - change the default system shell.
- [Useful Portage tools](https://wiki.gentoo.org/wiki/Useful_Portage_tools) — provides a list of Gentoo-specific system management tools, notably for [Portage](https://wiki.gentoo.org/wiki/Portage), available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).
