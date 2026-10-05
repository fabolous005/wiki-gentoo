<!-- source: https://wiki.gentoo.org/wiki/CVS | group: Gentoo Wiki (Main) | wiki-title: CVS -->
---
title: CVS
url: https://wiki.gentoo.org/wiki/CVS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-05"
fingerprint: "9e17fb7991a7b984"
license: CC BY-SA 4.0
---

# CVS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**CVS** (**C**oncurrent **V**ersions **S**ystem) is a [version control system](https://en.wikipedia.org/wiki/Version_control) that builds on and expands [RCS](https://en.wikipedia.org/wiki/Revision_Control_System). CVS enables users to record the history of source files and documents.

## Installation

### USE flags


| [crypt](https://packages.gentoo.org/useflags/crypt) | Add support for encryption -- using mcrypt or gpg where applicable | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [server](https://packages.gentoo.org/useflags/server) | Enable server support | 

### Emerge

Installing cvs is as easy as running an emerge command:

`root #``emerge --ask dev-vcs/cvs`
### Configuration

The default configuration file for CVS should be located in a file called \~/.cvsrc the user's home directory. Currently installing [dev-vcs/cvs](https://packages.gentoo.org/packages/dev-vcs/cvs) through [Portage](https://wiki.gentoo.org/wiki/Portage) does not create a default configuration file, therefore any specific configuration must be done by the user.

## Usage

Checkout a CVS module by using the following command:

`user $``cvs checkout <module_name>`
## See also

- The [CVS Tutorial](https://wiki.gentoo.org/wiki/CVS/Tutorial) article.

## External resources

- The CVS man page locally (`man cvs`) or online at
- [\[gentoo-dev\] Packages up for grabs: dev-vcs/cvs\* (post CVS project disband)](https://archives.gentoo.org/gentoo-dev/message/8c3e00874040dafa46f77470d6d5e121)
