<!-- source: https://wiki.gentoo.org/wiki/Lmod | group: Gentoo Wiki (Main) | wiki-title: Lmod -->
---
title: Lmod
url: https://wiki.gentoo.org/wiki/Lmod
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-03-03"
fingerprint: ce535a5867e73114
license: CC BY-SA 4.0
---

# Lmod

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Lmod is a [Lua](https://wiki.gentoo.org/wiki/Lua)-based module system that easily handles the MODULEPATH Hierarchical problem. Environment Modules provide a convenient way to dynamically change the users' environment through modulefiles. A modulefile contains the necessary information to manipulate the users' environment; such as information to add or remove directories from the `PATH`, `LD_LIBRARY_PATH`, `CPATH` and other environment variables. All popular [shells](https://wiki.gentoo.org/wiki/Shell) are supported, including [Bash](https://wiki.gentoo.org/wiki/Bash), csh, [fish](https://wiki.gentoo.org/wiki/Fish), ksh, sh, tcsh, [zsh](https://wiki.gentoo.org/wiki/Zsh), as well as some scripting languages such as [Tcl/Tk](https://wiki.gentoo.org/wiki/Tkinter), [Perl](https://wiki.gentoo.org/wiki/Perl) and [Python](https://wiki.gentoo.org/wiki/Python).

Lmod is used in HPC clusters, research labs and scientific computing environments all over the world. It is an alternative implementation for the classic Tcl/TK [environment modules](https://modules.readthedocs.io/en/latest/index.html) and improves upon it by creating module hierarchies, which allow setting of proper dependency structures for more stringent regulation of module loading and unloading.

## Installation

### USE flags


| [+auto-swap](https://packages.gentoo.org/useflags/+auto-swap) | enable auto swapping of compiler | 
| [+cache](https://packages.gentoo.org/useflags/+cache) | enable caching of modules | 
| [duplicate-paths](https://packages.gentoo.org/useflags/duplicate-paths) | allow duplicate entries in path | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask sys-cluster/lmod`
## Configuration

### Files

- /etc/modulefiles/ - Default location for modulefiles.
- /etc/lmod\_cache/spider\_cache - Default spider cache directory.
- /etc/lmod\_cache/system.txt - Default spider cache file.

## Usage

The default installation enables the module command for all users, by sourcing /etc/profile.d/lmod.sh.


`user $````
 man module
```
`user $````
 module -h
```
## Spider Cache

By default, Lmod enables spider cache. It is highly recommended to keep the spider cache enabled and up to date. The following command updates the spider cache.

`root #``/usr/share/Lmod/libexec/update_lmod_system_cache_files $MODULEPATH`
