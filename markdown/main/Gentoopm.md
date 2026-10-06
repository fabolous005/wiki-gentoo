<!-- source: https://wiki.gentoo.org/wiki/Gentoopm | group: Gentoo Wiki (Main) | wiki-title: Gentoopm -->
---
title: gentoopm
url: https://wiki.gentoo.org/wiki/Gentoopm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-12"
fingerprint: "10b19e8c5ee85f74"
license: CC BY-SA 4.0
---

# gentoopm

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**gentoopm** is a common interface to Gentoo package managers written in Python. It currently supports [Portage](https://wiki.gentoo.org/wiki/Portage) and [pkgcore](https://wiki.gentoo.org/wiki/Pkgcore).

The project provides a gentoopmq tool providing basic lookups into the package manager data. Most importantly, it provides an IPython-friendly gentoopmq shell command that can be used to play with the API.

## Installation

### USE flags


### USE flags for
            [app-portage/gentoopm](https://packages.gentoo.org/packages/app-portage/gentoopm)
            
            A common interface to Gentoo package managers

| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Install gentoopm:

`root #``emerge --ask app-portage/gentoopm`
