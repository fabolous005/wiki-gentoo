<!-- source: https://wiki.gentoo.org/wiki/RPM | group: Gentoo Wiki (Main) | wiki-title: RPM -->
---
title: RPM
url: https://wiki.gentoo.org/wiki/RPM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-17"
fingerprint: "7e02395af18e38c4"
license: CC BY-SA 4.0
---

# RPM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## USE flags


| [+sequoia](https://packages.gentoo.org/useflags/+sequoia) | Use app-crypt/rpm-sequoia instead of the deprecated internal parser | 
| [+sqlite](https://packages.gentoo.org/useflags/+sqlite) | Add support for sqlite - embedded sql database | 
| [+zstd](https://packages.gentoo.org/useflags/+zstd) | Enable support for ZSTD compression | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [berkdb](https://packages.gentoo.org/useflags/berkdb) | Add support for sys-libs/db (Berkeley DB) | 
| [bzip2](https://packages.gentoo.org/useflags/bzip2) | Enable bzip2 compression support | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [iconv](https://packages.gentoo.org/useflags/iconv) | Enable support for the iconv character set conversion library | 
| [lzma](https://packages.gentoo.org/useflags/lzma) | Support for LZMA compression algorithm | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 

## Emerge

`root #``emerge --ask app-arch/rpm`
## RPMs - faking them

Handy for persuading an installer that something is actually installed. The following recipe creates an RPM that does nothing but fake an RPM installation. Usually something like this will work to install an RPM:

`root #``rpm -Uvh --nodeps <example.rpm>`
If an application has an installer that calls RPM directly to see whether something is installed, this might help get over that hurdle.

- Create a .spec file in /usr/src/rpm/SPECS/. Then "build" the rpm

`root #``rpmbuild -ba <specfile>`
- Watch the output from the above to see where the .rpm is created and then install it

`root #``rpm -Uvh <rpm>`
### Sample .spec file

Create a .spec file using the following example for gettext as a template. Set the name and version strings accordingly.

**`/usr/src/rpm/SPECS/gettext-0.18.1.1.spec`**

**An example of a .spec file**
