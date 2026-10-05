<!-- source: https://wiki.gentoo.org/wiki/Q_applets | group: Gentoo Wiki (Main) | wiki-title: Q applets -->
---
title: q applets
url: https://wiki.gentoo.org/wiki/Q_applets
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-30"
fingerprint: "83291f2e42a72f02"
license: CC BY-SA 4.0
---

# q applets

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The q applets are a collection of small, fast [Portage](https://wiki.gentoo.org/wiki/Portage) **q**uery utilities written in C.

These are meant to offer a faster but more limited alternative to their [gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) counterparts.

q applets were not created to replace gentoolkit. 
q applets do not consider [eclasses](https://devmanual.gentoo.org/ebuild-writing/using-eclasses/index.html) and do not provide tools like revdep-rebuild or glsa-check.

## Installation

### USE flags


| [+gpkg](https://packages.gentoo.org/useflags/+gpkg) | Build GLEP-78 gpkg support in qpkg | 
| [+gtree](https://packages.gentoo.org/useflags/+gtree) | Build gtree repository cache support | 
| [+qmanifest](https://packages.gentoo.org/useflags/+qmanifest) | Build qmanifest applet, this adds additional dependencies for GPG, OpenSSL and BLAKE2B hashing | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 

### Emerge

Install the q applets:

`root #``emerge --ask app-portage/portage-utils`
## Usage

### Invocation

The complete list of applications that are provided by [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) can be listed by typing man q or q --help. Please read the man page for each utility described, as this guide is only meant to be a short reference for the most useful functions and does not include all the information about each application.

To see the available applets with a short description of their function, use q --help:

`user $``q --help````
usage: q <applet> <args>  : invoke a portage utility applet
currently defined applets:
         q <applet> <args> : virtual applet
     qatom <pkg>           : split atom strings
    qcheck <pkgname>       : verify integrity of installed packages
  qdepends <pkgname>       : show dependency info
     qfile <filename>      : list all pkgs owning files
     qgrep <expr> [pkg ...]: grep in ebuilds
  qkeyword <action> <args> : list packages based on keywords
     qlist <pkgname>       : list files owned by pkgname
      qlop <pkgname>       : emerge log analyzer
 qmanifest <misc args>     : verify or generate thick Manifest files
    qmerge <pkgnames>      : fetch and merge binary package
      qpkg <misc args>     : create or manipulate Gentoo binpkgs
   qsearch <regex>         : search pkgname/desc
     qsize <pkgname>       : calculate size usage
     qtbz2 <misc args>     : manipulate tbz2 packages
  qtegrity <misc args>     : verify files with IMA
      quse <useflag>       : find pkgs using useflags
    qwhich <pkg ...>       : find path to pkg
     qxpak <misc args>     : manipulate xpak archives
options: -[ioemvqChV]
  -i, --install    * Install symlinks for applets
  -o, --overlays   * Print available overlays (read from repos.conf)
  -e, --envvar     * Print used variables and their found values
  -m, --masks      * Print (package.)masks for the current profile
      --root <arg> * Set the ROOT env var
  -v, --verbose    * Report full package versions, emit more elaborate output
  -q, --quiet      * Tighter output; suppress warnings
  -C, --nocolor    * Don't output color
      --color      * Force color in output
  -h, --help       * Print this help and exit
  -V, --version    * Print version and exit
```
### How to find a package to which a file belongs (qfile)

The qfile command finds the package to which a file belongs:

`user $``qfile /etc/fonts/fonts.conf`
media-libs/fontconfig (/etc/fonts/fonts.conf)

### Verifying package integrity (qcheck)

To check the MD5 checksums or modification times of the files installed by some package, use the qcheck application:

`user $``qcheck portage-utils`
Checking app-portage/portage-utils-0.1.13 ...
  \* 36 out of 36 files are good

All the files which were changed after installation will be reported here. Configuration files which have been manually edited after installation are reported too (such as the /etc/conf.d/ directory for OpenRC systems). Most packages do not require root permissions. However, if a package has files that are only accessible to root qcheck should be run as root.

To check the integrity of *all* installed packages, enter:

`root #``qcheck`
### Listing package dependencies (qdepends)

qdepends lists the dependencies of a package in either direction. Options restrict the type of dependencies listed: `DEPEND` (`-d`), `RDEPEND` (`-r`), `PDEPEND` (`-p`) or `BDEPEND` (`-b`). Without options, all dependencies needed by a package are displayed merged into one list.

`user $``qdepends mutt`
mail-client/mutt-1.13.1: >=app-portage/elt-patches-20170815 >=sys-devel/automake-1.15.1:1.15 dev-libs/libressl:0/47= dev-db/lmdb:0/0.9.24= virtual/libintl www-client/w3m !\<sys-devel/gettext-0.18.1.1-r3 dev-libs/libxslt dev-libs/libxml2 >=sys-devel/automake-1.16.1:1.16 >=sys-devel/libtool-2.4 >=sys-devel/autoconf-2.69 net-dns/libidn2 virtual/libiconv >=app-crypt/gpgme-0.9.0:1/11= www-client/elinks app-misc/mime-types app-text/docbook-xsl-stylesheets >=dev-libs/cyrus-sasl-2 www-client/lynx net-mail/mailbase >=sys-libs/ncurses-5.2:0/6=

Use `-v` to get a shell-compatible and formatted dependency output list, like found in ebuilds.

`user $``qdepends -rv mutt````
mail-client/mutt-1.13.1:
RDEPEND="
    app-misc/mime-types
    virtual/libiconv
    dev-db/lmdb:0/0.9.24=
    dev-libs/libressl:0/47=
    virtual/libintl
    >=dev-libs/cyrus-sasl-2
    net-dns/libidn2
    >=app-crypt/gpgme-0.9.0:1/11=
    >=sys-libs/ncurses-5.2:0/6=
"
```
To list all of the installed packages that depend on a package use the `-Q` option.

`user $``qdepends -Q mime-types`
mail-client/mutt-1.13.1: >=app-portage/elt-patches-20170815 >=sys-devel/automake-1.15.1:1.15 dev-libs/libressl:0/47= dev-db/lmdb:0/0.9.24= virtual/libintl www-client/w3m !\<sys-devel/gettext-0.18.1.1-r3 dev-libs/libxslt dev-libs/libxml2 >=sys-devel/automake-1.16.1:1.16 >=sys-devel/libtool-2.4 >=sys-devel/autoconf-2.69 net-dns/libidn2 virtual/libiconv >=app-crypt/gpgme-0.9.0:1/11= app-misc/mime-types www-client/elinks app-text/docbook-xsl-stylesheets >=dev-libs/cyrus-sasl-2 www-client/lynx net-mail/mailbase >=sys-libs/ncurses-5.2:0/6=
dev-lang/python-2.7.16: >=app-portage/elt-patches-20170815 >=sys-libs/readline-4.1:0/8= >=sys-devel/automake-1.15.1:1.15 dev-libs/libressl:0/47= virtual/libintl >=dev-db/sqlite-3.3.8:3/3= virtual/pkgconfig !\<sys-devel/gettext-0.18.1.1-r3 virtual/libffi >=sys-devel/automake-1.16.1:1.16 >=dev-libs/expat-2.1 >=sys-libs/zlib-1.1.3:0/1= >=sys-devel/autoconf-2.69 >=app-eselect/eselect-python-20140125-r1 app-misc/mime-types >=sys-devel/autoconf-2.65 !!\<sys-apps/portage-2.1.9 app-arch/bzip2:0/1= !sys-devel/gcc\[libffi(+)\] >=sys-libs/ncurses-5.2:0/6=
dev-lang/python-3.7.2: >=app-portage/elt-patches-20170815 >=sys-libs/readline-4.1:0/8= >=sys-devel/automake-1.15.1:1.15 virtual/libffi:0/7= dev-libs/libressl:0/47= virtual/libintl >=dev-db/sqlite-3.3.8:3/3= app-arch/xz-utils:0/0= virtual/pkgconfig !\<sys-devel/gettext-0.18.1.1-r3 >=sys-devel/automake-1.16.1:1.16 !!\<sys-apps/sandbox-2.6-r1 >=dev-libs/expat-2.1:0/0= >=sys-libs/zlib-1.1.3:0/1= >=sys-devel/autoconf-2.69 >=app-eselect/eselect-python-20140125-r1 app-misc/mime-types app-arch/bzip2:0/1= !sys-devel/gcc\[libffi(+)\] >=sys-libs/ncurses-5.2:0/6=
dev-lang/python-3.6.8: >=app-portage/elt-patches-20170815 >=sys-libs/readline-4.1:0/8= >=sys-devel/automake-1.15.1:1.15 virtual/libffi:0/7= dev-libs/libressl:0/47= virtual/libintl >=dev-db/sqlite-3.3.8:3/3= app-arch/xz-utils:0/0= virtual/pkgconfig !\<sys-devel/gettext-0.18.1.1-r3 >=sys-devel/automake-1.16.1:1.16 !!\<sys-apps/sandbox-2.6-r1 >=dev-libs/expat-2.1:0/0= >=sys-libs/zlib-1.1.3:0/1= >=sys-devel/autoconf-2.69 >=app-eselect/eselect-python-20140125-r1 app-misc/mime-types app-arch/bzip2:0/1= !sys-devel/gcc\[libffi(+)\] >=sys-libs/ncurses-5.2:0/6=

### Search ebuilds or eclasses for a pattern (qgrep)

qgrep can be used to find ebuilds that mention an ebuild's name ("libechonest" is used in the example below) which will list all packages (installed or not) which depend on some package:

`user $``qgrep -l libechonest`
media-libs/libechonest/libechonest-2.0.2.ebuild
media-libs/libechonest/libechonest-2.2.0-r1.ebuild
media-libs/libechonest/libechonest-2.3.0.ebuild
media-libs/libechonest/libechonest-2.3.1.ebuild
media-libs/libechonest/libechonest-2.3.1-r1.ebuild
media-libs/libechonest/libechonest-9999.ebuild
media-sound/clementine/clementine-1.2.3.ebuild
media-sound/clementine/clementine-1.2.3-r1.ebuild
media-sound/clementine/clementine-1.3.1-r1.ebuild
media-sound/tomahawk/tomahawk-0.8.4-r3.ebuild
media-sound/tomahawk/tomahawk-9999.ebuild

The `-J` option will limit the search to installed packages.  `-N` will print the atom instead of the filename.

`user $``qgrep -NJ net-print/cups````
app-office/libreoffice-6.1.5.2: cups? ( net-print/cups )
dev-qt/qtprintsupport-5.11.3:   cups? ( >=net-print/cups-1.4 )
net-print/hplip-3.18.6: net-print/cups
net-print/hplip-3.18.6: hpijs? ( net-print/cups-filters[foomatic] )
net-print/cups-2.2.7:PDEPEND=">=net-print/cups-filters-1.0.43"
net-print/cups-filters-1.21.6:  >=net-print/cups-1.7.3
net-print/cups-filters-1.21.6:  !<=net-print/cups-1.5.9999
net-wireless/bluez-5.50-r2:     cups? ( net-print/cups:= )
app-text/ghostscript-gpl-9.26:  cups? ( >=net-print/cups-1.3.8 )
x11-libs/gtk+-2.24.32-r1:       cups? ( >=net-print/cups-1.7.1-r2:=[${MULTILIB_USEDEP}] )
x11-libs/gtk+-3.24.4-r1:        cups? ( >=net-print/cups-1.2[${MULTILIB_USEDEP}] )
```
### Listing files that belong to an ebuild (qlist)

The qlist command gives a list of all files that belong to an ebuild.

`user $``qlist vim`
/usr/bin/gvim
/usr/bin/gvimdiff
/usr/bin/evim
/usr/bin/eview
/usr/bin/gview
/usr/bin/rgvim
\[...\]

### Looking for packages that use some USE flag (quse)

Listing used USE flags is done with quse. In its simplest form, it lists which ebuilds use a given USE-flag.

`user $``quse firefox`
app-misc/tracker/tracker-0.12.10-r1.ebuild applet doc eds elibc\_glibc exif firefox-bookmarks flac flickr gif
\[...\]

To display the description of a USE-flag, the `-D` option can be used.  This can be combined with the `-p` option, which takes an atom name as argument, to list all USE-flags for the given atom.

`user $``quse -Dvp autogen`
sys-devel/autogen-5.18.16-r1
  libopts       install the libopts tarball (a few packages want this for developing)
  static-libs   Build static versions of dynamic libraries as well

### Finding package sizes (qsize)

To show the size of a package, use the qsize application:

`user $``qsize vim-core`
app-editors/vim-core: 1846 files, 175 non-files, 28.5M 
\[...\]

### Searching ebuild repositories (qsearch)

One of the most powerful tools of [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) is qsearch. This tool allows searching ebuild repositories much faster than using the emerge -s command.

Here are some examples of its usage:

`user $``qsearch terminus`
media-fonts/terminus-font: A clean fixed font for the console and X11

The homepage of packages can be queried using the `-H` option:

`user $``qsearch -H terminus`
media-fonts/terminus-font: http://terminus-font.sourceforge.net/

In another example, let's look for a jabber client:

`user $``qsearch -S "jabber client"`
app-emacs/emacs-jabber: A Jabber client for Emacs
net-im/coccinella: Jabber Client With a Built-in Whiteboard and VoIP (jingle)
net-im/gajim: Jabber client written in PyGTK
net-im/tkabber: A jabber client written in Tcl/Tk
net-im/vacuum: Qt Crossplatform Jabber client

### Extracting information from emerge logs (qlop)

qlop allows to extract useful information from the emerge.log file. It can be useful when package compilation times need to be estimated or to compare build times with other systems. It also allows to check what is compiling at the moment and how long it will probably take - which is handy when working in the console and don't have any other means to check it.

Estimate how long a [dev-lang/perl](https://packages.gentoo.org/packages/dev-lang/perl) build takes:

`user $``qlop -a perl`
dev-lang/perl: 7′12″ average for 3 merges

See what is emerging at the moment and how long the process has been running already:

`user $``qlop -rt`
2019-12-31T03:07:16 >>> net-fs/samba: 6′19″... (82 of 85) ETA: 23s

### Install binary package (qmerge)

qmerge can quickly install binary packages (binpkgs):

`user $``qmerge sys-apps/sed`
\[R\] sys-apps/sed-4.8

### Create or manipulate binary package (qpkg)

**qpkg**  is used to create or clean up Gentoo binary packages.

### qsearch

- List the descriptions of every package in the cache
- `user $``qsearch --all .`

- Regex search package basenames
- `user $``qsearch --search <regex>`

- Regex search package descriptions (or homepage when using -H)
- `user $``qsearch --desc <arg>`

- Only show package name
- `user $``qsearch --name-only <package>`

- Show homepage info instead of description
- `user $``qsearch --homepage gentoo.org`

- Show repository the ebuild originates from
- `user $``qsearch --repo gentoo`

- Print matched atom using given format string
- `user $``qsearch --format <arg>`

- Set the ROOT env va
- `user $``qsearch --root <arg>`

- Report full package versions, emit more elaborate output
- `user $``qsearch --verbose syslog-ng` 

- Tighter output; suppress warnings
- `user $``qsearch --quiet <package>`

- Don't output color
- `user $``qsearch --nocolor package>`

- Force color in output
- `user $``qsearch --color <package>`

- Print current qsearch version
- `user $``qsearch --version`

### quse

- Print version and exit
- `user $``quse --version`

- Print this help and exit
- `user $``quse --help`

- Force color in output
- `user $``quse --color`

- Don't output color
- `user $``quse --nocolor`

- Tighter output; suppress warnings
- `user $``quse --quiet <package>`

- Report full package versions, emit more elaborate output
- `user $``quse --verbose <package>`

- Set the ROOT env var
- `user $``quse --root <arg>`

- Print matched atom using given format string
- `user $``quse --format <arg>` 

- Show repository the ebuild originates from
- `user $``quse --repo`

- Restrict matching to package or category
- `user $``quse --package <arg>`

- Only search installed packages
- `user $``quse --installed`

- Describe all USE flags
- `user $``quse --describe .`

- Use the LICENSE vs IUSE
- `user $``quse --license .`

- List all ebuilds, don't match anything
- `user $``quse --all`

- Show exact non regexp matching using strcmp
- `user $``quse --exact useflag`

## Troubleshooting

### Defining atom fields for applet commands

Certain q applet commands / argument combinations (such as qsearch --format) mention the use of atom formatting. The defintion of this formatting can be found on the qatom man page under the `--format` (`-F`) argument:

`user $``man 1 qatom`
## See also

- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.
- [Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) — a suite of tools to ease the administration of a Gentoo system, and [Portage](https://wiki.gentoo.org/wiki/Portage) in particular.
- [Useful Portage tools](https://wiki.gentoo.org/wiki/Useful_Portage_tools) — provides a list of Gentoo-specific system management tools, notably for [Portage](https://wiki.gentoo.org/wiki/Portage), available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).
