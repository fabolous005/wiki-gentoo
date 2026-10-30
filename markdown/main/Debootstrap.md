<!-- source: https://wiki.gentoo.org/wiki/Debootstrap | group: Gentoo Wiki (Main) | wiki-title: Debootstrap -->
---
title: Debootstrap
url: https://wiki.gentoo.org/wiki/Debootstrap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-29"
fingerprint: c29dfd2e63d5d7c7
license: CC BY-SA 4.0
---

# Debootstrap

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**debootstrap** is a utility for installing a base Debian system in a directory.

## Installation

### Emerge

`root #``emerge --ask category/package`
## Usage

To bootstrap a Debian base system for **amd64** in /my/chroot:

`root #``debootstrap --arch amd64 stable /my/chroot/`
Use [chroot](https://wiki.gentoo.org/wiki/Chroot) to use the bootstrapped system.

### Invocation

`user $``debootstrap --help````
Usage: debootstrap [OPTION]... <suite> <target> [<mirror> [<script>]]
Bootstrap a Debian base system into a target directory.
      --help                 display this help and exit
      --version              display version information and exit
      --verbose              don't turn off the output of wget
      --download-only        download packages, but don't perform installation
      --print-debs           print the packages to be installed, and exit
      --arch=A               set the architecture to install (use if no dpkg)
                               [ --arch=powerpc ]
      --include=A,B,C        adds specified names to the list of base packages
      --exclude=A,B,C        removes specified packages from the list
      --extra-suites=A,B,C   also use packages from the listed suites of the
                             archive
      --components=A,B,C     use packages from the listed components of the
                             archive
      --variant=X            use variant X of the bootstrap scripts
                             (currently supported variants: buildd, fakechroot,
                              minbase)
      --merged-usr           make /{bin,sbin,lib}/ symlinks to /usr/
                             (default for newer Debian suites)
      --no-merged-usr        do not make /{bin,sbin,lib}/ symlinks to /usr/
                             (default for older Debian suites)
      --keyring=K            check Release files against keyring K
      --no-check-gpg         avoid checking Release file signatures
      --force-check-gpg      force checking Release file signatures
                             (also disables automatic fallback to HTTPS in case
                             of a missing keyring), aborting otherwise
      --no-resolve-deps      don't try to resolve dependencies automatically
      --log-extra-deps       record extra dependency info in debootstrap.log
      --cache-dir=DIR        Use specified directory as package cache directory
      --unpack-tarball=T     acquire .debs from a tarball instead of http
      --make-tarball=T       download .debs and create a gzipped tarball
      --second-stage-target=DIR
                             Run second stage in a subdirectory instead of root
                               (can be used to create a foreign chroot)
                               (requires --second-stage)
      --extractor=TYPE       override automatic .deb extractor selection
                               (supported: dpkg-deb ar)
      --debian-installer     used for internal purposes by debian-installer
      --private-key=file     read the private key from file
      --certificate=file     use the client certificate stored in file (PEM)
      --no-check-certificate do not check certificate against certificate authorities
      --inrelease-path       determine the path to the InRelease file of the main
                             archive relative to the normal position of an InRelease
                             file
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-util/debootstrap`
## See also

- [Chroot](https://wiki.gentoo.org/wiki/Chroot) — a Unix system utility used to change the apparent root directory to create a new environment logically separate from the main system's root directory.
