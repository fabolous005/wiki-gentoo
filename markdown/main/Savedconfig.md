<!-- source: https://wiki.gentoo.org/wiki/Savedconfig | group: Gentoo Wiki (Main) | wiki-title: Savedconfig -->
---
title: savedconfig
url: https://wiki.gentoo.org/wiki/Savedconfig
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: "1404321da3338988"
license: CC BY-SA 4.0
---

# savedconfig

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**savedconfig** is a USE flag that preserves the saved configuration files upon package updates. It allows for the updating of other files from a package update while, at the same time, guaranteeing no changes will be made to some specific files defined under Portage's /etc/portage/savedconfig/ directory.

## Prerequisites

To understand how Portage handles configuration files, it is wise to the review the **CONFIGURATION FILES** section of emerge's man page:

`user $``man 1 emerge`
By default, files under the /etc directory are protected from modification or deletion unless Portage has been explicitly instructed otherwise.

## Usage

Although it is possible to add `savedconfig` USE flag to the system's make.conf file, it is better to define it on per-package basis.

Setting `savedconfig` will restore the configuration files from, in order of priority, the [/etc/portage/savedconfig](https://wiki.gentoo.org/wiki//etc/portage/savedconfig)/${CATEGORY}/${PF}, [/etc/portage/savedconfig](https://wiki.gentoo.org/wiki//etc/portage/savedconfig)/${CATEGORY}/${P}, or [/etc/portage/savedconfig](https://wiki.gentoo.org/wiki//etc/portage/savedconfig)/${CATEGORY}/${PN} *file*. Make sure the USE flags allow for appropriate dependencies.

### Example

Enable the `savedconfig` USE flag on the [sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) package:

**`/etc/portage/package.use/gentoo-kernel`**

Emerge the package; note that after package installation a message similar to the following will be displayed:

`root #``emerge --ask --update --changed-use sys-kernel/gentoo-kernel````
 * Building using saved config directory "/etc/portage/savedconfig/sys-kernel/gentoo-kernel"
 * Your configuration for sys-kernel/gentoo-kernel-5.9.2 has been saved in 
 * "/etc/portage/savedconfig/sys-kernel/gentoo-kernel" for your editing pleasure.
 * You can edit these files by hand and remerge this package with
 * USE=savedconfig to customise the configuration.
 * You can rename this file/directory to one of the following for
 * its configuration to apply to multiple versions:
 * ${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/
 * [${CTARGET}|${CHOST}|""]/${CATEGORY}/[${PF}|${P}|${PN}]
```
On the *next* emerge of the package, Portage will use the saved configuration file located at /etc/portage/savedconfig/sys-kernel/gentoo-kernel as the .config file for compiling the kernel. This file may be modified as necessary, or, for example, can be replaced by another .config entirely.

## See also

## External resources

- [https://packages.gentoo.org/useflags/savedconfig](https://packages.gentoo.org/useflags/savedconfig) - An online view of packages containing the `savedconfig` USE flag.
- [https://devmanual.gentoo.org/eclass-reference/savedconfig.eclass/index.html](https://devmanual.gentoo.org/eclass-reference/savedconfig.eclass/index.html) - savedconfig eclass man page
- [https://gitweb.gentoo.org/repo/gentoo.git/tree/eclass/savedconfig.eclass](https://gitweb.gentoo.org/repo/gentoo.git/tree/eclass/savedconfig.eclass) - savedconfig eclass source
