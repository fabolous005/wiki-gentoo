<!-- source: https://wiki.gentoo.org/wiki/Eselect/Repository | group: Gentoo Wiki (Main) | wiki-title: Eselect/Repository -->
---
title: eselect/repository
url: https://wiki.gentoo.org/wiki/Eselect/Repository
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: fa09531f84a0bbe0
license: CC BY-SA 4.0
---

# eselect/repository

[Eselect](https://wiki.gentoo.org/wiki/Special:MyLanguage/Eselect)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


eselect-repository is an [eselect](https://wiki.gentoo.org/wiki/Eselect) module for configuring [ebuild repositories](https://wiki.gentoo.org/wiki/Ebuild_repository) for [Portage](https://wiki.gentoo.org/wiki/Portage). Ebuild repository configuration files are stored in [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf).

eselect-repository is written and maintained by Gentoo's [Michał Górny (mgorny)](https://wiki.gentoo.org/wiki/User:MGorny) .

## Installation

### USE flags


### USE flags for
            [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository)
            
            Manage repos.conf via eselect

### Emerge

`root #``emerge --ask app-eselect/eselect-repository`
## Configuration

### Initial setup

The repos.conf file or directory as configured by the `REPOS_CONF` variable in /etc/eselect/repository.conf, must exist before the module will function properly. The  [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/CustomTree#Defining_a_custom_repository) prefers to have it as a directory, and some tools will not work otherwise:

`root #``mkdir -p /etc/portage/repos.conf`
### Files

Paths and options can be changed in /etc/eselect/repository.conf. This file has comments and is self-explanatory.

## Usage

### repos.gentoo.org

Gentoo allows users and developers to register [repositories on repos.gentoo.org](https://repos.gentoo.org/), for public consumption.
eselect repository will fetch and read the known list.

#### Listing ebuild repositories registered with repos.gentoo.org

eselect repository can print all repositories listed on repos.gentoo.org:

`user $``eselect repository list`
Available repositories:
  \[1\]   foo
  \[2\]   bar
  \[3\]   baz
  \[4\]   cross #
  \[5\]   good \*
  \[6\]   my\_overlay @

- Installed, enabled repositories are suffixed with a \* character.
- Repositories suffixed with #, need their sync information updated (via disable/enable) or were customized by the user.
- Repositories suffixed with @ are not listed by name in the official, published list.


Use the `-i` option to show currently configured repositories only:

`user $``eselect repository list -i`
#### Add ebuild repositories from repos.gentoo.org

Syntax: enable (\<name>|\<index>)...

`root #``eselect repository enable foo bar baz`
### Add repositories

Syntax:

`root #``eselect repository add <name> <sync-type> <sync-uri>`
The most common sync methods are git and rsync. Other options are cvs, mercurial, svn, websync and zipfile.

The formats and required packages are as follows:

git ([dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git)) installed by default.

`root #``eselect repository add test git https://github.com/test/test.git`
rsync ([net-misc/rsync](https://packages.gentoo.org/packages/net-misc/rsync)) installed by default.

`root #``eselect repository add test rsync rsync://example.com/path`
mercurial ([dev-vcs/mercurial](https://packages.gentoo.org/packages/dev-vcs/mercurial), note that [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) should be compiled with the [mercurial](https://packages.gentoo.org/useflags/mercurial) [USE flag).](https://wiki.gentoo.org/wiki/USE_flag)

`root #``eselect repository add test hg https://example.com/path`
cvs ([dev-vcs/cvs](https://packages.gentoo.org/packages/dev-vcs/cvs)).

`root #``eselect repository add test cvs :ext:anoncvs@example.com:/var/cvsroot`
svn ([dev-vcs/subversion](https://packages.gentoo.org/packages/dev-vcs/subversion)).

`root #``eselect repository add test svn https://example.com/repos/name/trunk`
webrsync installed with portage, [app-portage/emerge-delta-webrsync](https://packages.gentoo.org/packages/app-portage/emerge-delta-webrsync) can be installed to reduce bandwidth usage.

`root #``eselect repository add test webrsync https://example.com/path`
zipfile installed by default.

`root #``eselect repository add test zipfile https://example.com/file.zip`
To sync the new repository which is called test in this example:

`root #``emaint sync --repo test`
For more information on synchronization in Gentoo, please see [Ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization) before use.

### Disable repositories without removing contents

Syntax: disable \[-f\] (\<name>|\<index>)...

`root #``eselect repository disable foo bar`
The `-f` option is required for repositories not registered with repos.gentoo.org, and those without sync attributes. Use with care.

### Disable repositories and remove contents

Syntax: remove \[-f\] (\<name>|\<index>)...

`root #``eselect repository remove bar baz`
The `-f` option is required for repositories not registered with repos.gentoo.org, and those without sync attributes. Use with care.

### Create a new ebuild repository

The create subcommand will [create an ebuild repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) skeleton, and configure it with Portage:

Syntax: create \<name> \[\<path>\]

`root #``eselect repository create <ebuild_repository_name>`
Adding \<ebuild\_repository\_name> to /etc/portage/repos.conf ...
Repository \<ebuild\_repository\_name> created and added

## See also

- [Eselect](https://wiki.gentoo.org/wiki/Eselect) — a tool for administration and configuration on Gentoo systems.
- [Useful Portage tools](https://wiki.gentoo.org/wiki/Useful_Portage_tools) — provides a list of Gentoo-specific system management tools, notably for [Portage](https://wiki.gentoo.org/wiki/Portage), available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).
- [Project:Portage/Sync](https://wiki.gentoo.org/wiki/Project:Portage/Sync)
