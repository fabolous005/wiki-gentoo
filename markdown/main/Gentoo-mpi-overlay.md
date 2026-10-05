<!-- source: https://wiki.gentoo.org/wiki/Gentoo-mpi-overlay | group: Gentoo Wiki (Main) | wiki-title: Gentoo-mpi-overlay -->
---
title: Gentoo-mpi-overlay
url: https://wiki.gentoo.org/wiki/Gentoo-mpi-overlay
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-24"
fingerprint: "4948b67a6e257fa7"
license: CC BY-SA 4.0
---

# Gentoo-mpi-overlay

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Description

The MPI overlay serves as a set of tools and standards used when writing and building MPI software. The goal is to make it easier for users and developers of MPI software to write, build, and maintain their code. The current overlay consists of two eclasses- mpi-providers and mpi-select- which should be used in all MPI ebuilds currently in the Portage tree, and for development in the future.


| **mpi-providers** | : Allows for parallel installations of varied versions of a common MPI implementation, all installed to the same parent directory. | 
| **mpi-select** | : Enables MPI software to be built against selected MPI implementations at the users' discretion. | 

### Prerequisites

- Currently the eclasses provided require **EAPI 5 or 6** to run.
- Many MPI ebuilds require **updated gcc with proper fortran USE flags** in order to build.

### Usage

Both mpi-providers and mpi-select should be properly used in all MPI implementations.

#### mpi-providers.eclass

This is to support multiple MPI implementation's installations in parallel. Remove any "SLOT=" assignment from the ebuild, as this is handled by mpi-providers. Append 'sysconfdir="$(mpi-providers\_sysconfdir)" ' to your econf arguments. In the install phase, insert "mpi-providers\_safe\_mv" to the end of the installation function, as this will move the installation destination in such a way to support parallel installs in /usr/lib/mpi.

##### For Users

Things to note:

| **/usr/lib/mpi** | : Directory for all MPI implementations to be installed. | 
| **/etc/${mpi-ipmlementation}** | : Location where all files previously installed to /etc/\* are installed for a particular MPI package. | 
| **/usr/share/doc** | : Documentation directory, as usual. | 

##### For Developers

1. inherit mpi-providers
2. Any SLOT= declarations should be removed. The SLOT will be assigned automatically to ${PF}, i.e. mpich-3.2-r1.
3. The following should be appended to src\_configure() econf arguments, or changed if there is a current --sysconfdir declaration to the following:

--sysconfdir="$(mpi-providers\_sysconfdir)" \

#### mpi-select.eclass

This allows other mpi software to be built with multiple MPI implementations. For example, if you want to build HPL with mpich and openmpi, mpi-select will build mpich and openmpi against HPL.

##### For Users

Your settings will be stored in make.conf as MPI\_TARGETS. Please use eselect to access which MPI implementations you would like to build other MPI packages against:

`root #``eselect mpi list`
##### For Developers

1. inherit mpi-select

### mpi-select.eclass

This allows other mpi software to be built with multiple MPI implementations. For example, if you want to build HPL with mpich and openmpi, mpi-select will build mpich and openmpi against HPL.
