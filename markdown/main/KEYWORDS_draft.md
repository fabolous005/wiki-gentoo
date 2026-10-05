<!-- source: https://wiki.gentoo.org/wiki/KEYWORDS/draft | group: Gentoo Wiki (Main) | wiki-title: KEYWORDS/draft -->
---
title: KEYWORDS/draft
url: https://wiki.gentoo.org/wiki/KEYWORDS/draft
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-10-09"
fingerprint: "9fb304974fc1221c"
license: CC BY-SA 4.0
---

# KEYWORDS/draft

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

THIS IS A DRAFT TO REPLACE THE PAGE [KEYWORDS](https://wiki.gentoo.org/wiki/KEYWORDS). FEEL FREE TO EDIT!

In Gentoo *Keywords* are used to define which branch a software version is assigned to, similar to stable, unstable and testing in Debian. In Gentoo the branches are called stable and testing. Stable is described as *arch*, e.g. arm64, x86 and testing is described as \~*arch*, e.g. \~amd64, \~ppc. Versions of a package that are not part of stable or testing are called "not keyworded". The classification is independent for every [architecture](https://wiki.gentoo.org/wiki/Property:Architecture).

## Keywording and stabilization

If a new software gets part of a repository it is with only those arches keyworded which the maintainer has tested. After some short testing a keywording request can be made by users for further *architectures* in [https://bugs.gentoo.org](https://bugs.gentoo.org). The package version gets keyworded as \~*arch* by the maintainer. After some time and testing a [stabilization request](https://wiki.gentoo.org/wiki/Stable_request) can be made and the package version gets the keyword *arch*.

The exact requirements for keywording and stabilizations requests are defined by the architecture [projects](https://wiki.gentoo.org/wiki/Category:Gentoo_Projects).

## Example of the different keywords of a package

There are several ways to get details about the keywords of a software version, for example [equery](https://wiki.gentoo.org/wiki/Equery) and [packages.gentoo.org](https://packages.gentoo.org):

`user $``equery m app-editors/emacs````
 * app-editors/emacs [gentoo]
Upstream:    None specified
Homepage:    https://www.gnu.org/software/emacs/
Location:    /var/db/repos/gentoo/app-editors/emacs
Keywords:    18.59-r14:
Keywords:    23.4-r21:
Keywords:    24.5-r11:
Keywords:    25.3-r11:
Keywords:    26.3-r7:
Keywords:    27.2-r5:
Keywords:    28.1-r2:
Keywords:    28.1-r3:
Keywords:    28.2:
Keywords:    28.2.9999:
Keywords:    29.0.9999:
License:     GPL-3+ FDL-1.3+ BSD HPND MIT W3C unicode PSF-2
```
Maintainer:  gnu-emacs@gentoo.org (Gentoo GNU Emacs project)**18**: amd64 x86**23**: amd64 arm ppc ppc64 x86 ~alpha ~amd64-linux ~hppa ~ia64 ~mips ~ppc-macos ~sparc ~x86-linux**24**: amd64 arm ppc x86 ~alpha ~amd64-linux ~hppa ~ia64 ~mips ~ppc-macos ~ppc64 ~sparc ~x64-macos ~x86-linux**25**: amd64 arm arm64 ppc ppc64 sparc x86 ~alpha ~amd64-linux ~hppa ~ia64 ~m68k ~mips ~ppc-macos ~x64-macos ~x86-linux**26**: amd64 arm arm64 hppa ppc ppc64 sparc x86 ~alpha ~amd64-linux ~ia64 ~mips ~ppc-macos ~riscv ~x64-macos ~x86-linux**27**: amd64 arm arm64 hppa ppc ppc64 sparc x86 ~alpha ~amd64-linux ~ia64 ~m68k ~mips ~ppc-macos ~riscv ~x64-macos ~x86-linux**28**: amd64 arm arm64 hppa ppc ppc64 sparc x86**28**:**28**: ~alpha ~amd64 ~amd64-linux ~arm ~arm64 ~hppa ~ia64 ~m68k ~mips ~ppc ~ppc-macos ~ppc64 ~riscv ~sparc ~x64-macos ~x86 ~x86-linux**28-vcs**:**29-vcs**:

![](https://wiki.gentoo.org/images/thumb/f/f6/Screenshot_of_packages.gentoo.org_showing_emacs.png/300px-Screenshot_of_packages.gentoo.org_showing_emacs.png)



## Some possible values and wildcards for KEYWORDS

The following box contains some example values for the `KEYWORDS` variable:

**`example.ebuild`**

```
KEYWORDS="alpha amd64 arm arm64 hppa ia64 m68k ~mips ppc ppc64 s390 sh sparc x86 ~sparc-fbsd ~x86-fbsd"
```
See the /var/db/repos/gentoo/profiles/arch.list for a list of keywords.

The prefix `~` (a tilde character) placed in front of various architectures in the example above means that architecture is in a "testing phase" and is not ready for production usage.

### Special wildscards for keywords

In addition to the normal `KEYWORDS` values [Portage](https://wiki.gentoo.org/wiki/Portage) supports three special wildcards:

- `*` - Package is visible if it is stable on any architecture.
- `~*` - Package is visible if it is in testing on any architecture.
- `**` - Package is always visible (`KEYWORDS` are ignored completely).

### Using more then one wildcard or keyword

To use a recent version of fdftk which is marked stable or unstable on any arch use:

**`/etc/portage/package.accept_keywords`**

To use a recent version of fdftk which is marked unstable on your architecture or stable on any arch use:

**`/etc/portage/package.accept_keywords`**

To use a recent version of fdftk which is marked unstable on x86 or amd64 use:

**`/etc/portage/package.accept_keywords`**

## See also

- [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS)
- [Stable request](https://wiki.gentoo.org/wiki/Stable_request) — the procedure for moving an ebuild from testing to stable.
- [Package testing](https://wiki.gentoo.org/wiki/Package_testing) — provides information for ebuild developers on **testing ebuilds**.

## External resources

- [https://devmanual.gentoo.org/ebuild-writing/variables/index.html#keywords](https://devmanual.gentoo.org/ebuild-writing/variables/index.html#keywords)
- [Gentoo Development Guide: Keywording](https://devmanual.gentoo.org/keywording/)
- [Devmanual / Quickstart Ebuild Guide / Information Variables](https://devmanual.gentoo.org/quickstart/#information-variables) - The KEYWORDS variable is set to archs upon which this ebuild has been tested. We use \~ keywords for newly written ebuilds.
- [https://devmanual.gentoo.org/tools-reference/ekeyword/](https://devmanual.gentoo.org/tools-reference/ekeyword/)
- [https://github.com/mgorny/nattka/#filing-keywordingstabilization-bugs](https://github.com/mgorny/nattka/#filing-keywordingstabilization-bugs)
