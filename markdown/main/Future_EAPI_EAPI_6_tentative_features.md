<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_6_tentative_features | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/EAPI 6 tentative features -->
---
title: Future EAPI/EAPI 6 tentative features
url: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_6_tentative_features
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-14"
fingerprint: be0f0a67d0b2ab70
license: CC BY-SA 4.0
---

# Future EAPI/EAPI 6 tentative features

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a working page that contains references to features that have been suggested for EAPI 6.

The following list of features accepted for or rejected from EAPI 6 is based on Gentoo Council meetings of [2014-06-10](https://projects.gentoo.org/council/meeting-logs/20140610-summary.txt), [2014-06-17](https://projects.gentoo.org/council/meeting-logs/20140617-summary.txt), [2014-06-24](https://projects.gentoo.org/council/meeting-logs/20140624-summary.txt), [2014-09-09](https://projects.gentoo.org/council/meeting-logs/20140909-summary.txt), [2014-10-14](https://projects.gentoo.org/council/meeting-logs/20141014-summary.txt), and [2014-11-11](https://projects.gentoo.org/council/meeting-logs/20141111-summary.txt).

## Accepted

### New features

- get\_libdir()
  - [Specification](https://gitweb.gentoo.org/proj/pms.git/commit/?id=2377e8895cbf0787220c1468e8d8cec50b65b714)
  - [bug #463586](https://bugs.gentoo.org/show_bug.cgi?id=463586)
    - Used in econf, but so far not available as separate PM function.

- einstalldocs()

- Query function for IUSE\_EFFECTIVE

- Patch applying function in package manager
  - Specification [1](https://gitweb.gentoo.org/proj/pms.git/commit/?id=440b0ee22c50bfd1c7f0b1038213a282c8bbbfa8), [2](https://gitweb.gentoo.org/proj/pms.git/commit/?id=a8196cb8791b2eba41e27e9adae2c591fcefa04c)
  - [bug #463768](https://bugs.gentoo.org/show_bug.cgi?id=463768)
    - Needed for PATCHES support and user patches.
    - This duplicates epatch() from eutils, in simplified form.
    - Name "eapply" has been suggested.

- User patches
  - Specification [1](https://gitweb.gentoo.org/proj/pms.git/commit/?id=aa7ce7631a20c0a1369c32f8e207f4fb40edb33c), [2](https://gitweb.gentoo.org/proj/pms.git/commit/?id=0d2ad45e5083cea349f4c4e4a596e6b03396f24b), [3](https://gitweb.gentoo.org/proj/pms.git/commit/?id=c399c49b71762ed969e29146f5f85d071905296d)
  - [bug #475288](https://bugs.gentoo.org/show_bug.cgi?id=475288)
    - Name "eapply\_user" has been suggested.
    - Will be called from default\_src\_prepare().

- PATCHES support in default src\_prepare

### Enhancements of existing features

- nonfatal die()

- Allow empty DOCS variable

- Directory support for DOCS

- Unpack .txz

- Case-fold extensions in unpack

- unpack() accept absolute paths

- Pass --docdir and --htmldir options to configure

### Other changes

- Bash 4.2

- failglob in global scope
  - [Specification](https://gitweb.gentoo.org/proj/pms.git/commit/?id=5e5b02033ff3f2a8fa0783668c11960277aa68bd)
  - [bug #463822](https://bugs.gentoo.org/show_bug.cgi?id=463822)
    - Only in global scope, not in local scope of functions

- Ensure sane settings for LC\_CTYPE and LC\_COLLATE

- Ban einstall
  - [Specification](https://gitweb.gentoo.org/proj/pms.git/commit/?id=e43e6aac9178f1364d9d5b265dcd7de0bd859c6e)
  - [bug #524112](https://bugs.gentoo.org/show_bug.cgi?id=524112)
    - Current einstall will break when --docdir and --htmldir options are passed to configure (which has been accepted for EAPI 6).
    - Can be easily replaced by an emake call, and is used scarcely in the tree.

## Deferred to future EAPI

- Runtime-switchable USE flags

- Variant of || ( ) with defined runtime behaviour

- Ban dohtml
  - [bug #520546](https://bugs.gentoo.org/show_bug.cgi?id=520546)
    - Will be kept (deprecated) in EAPI 6.

- Directory support for package\* and use\*
  - [bug #282296](https://bugs.gentoo.org/show_bug.cgi?id=282296)
    - Not intended for gentoo-x86 tree, only to be used in overlays.

## Rejected

- EJOBS variable
  - [bug #273101](https://bugs.gentoo.org/show_bug.cgi?id=273101)
  - [gentoo-dev discussion](https://public-inbox.gentoo.org/gentoo-dev/m2wsef4rbg.fsf@gmail.com/)
    - makeopts\_jobs() and makeopts\_loadavg() in multiprocessing.eclass provide similar functionality.

- Source eclasses only once
  - [bug #422533](https://bugs.gentoo.org/show_bug.cgi?id=422533)
  - [gentoo-dev discussion](https://public-inbox.gentoo.org/gentoo-dev/20120814114449.0db3d120@pomiocik.lan/)
    - Alternative solution is already in place in eclasses.

- HDEPEND: host dependencies for cross-compilation

- dohtml additional default suffixes
