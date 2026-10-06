<!-- source: https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS | group: Gentoo Wiki (Main) | wiki-title: ACCEPT KEYWORDS -->
---
title: ACCEPT_KEYWORDS
url: https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "1ebb1efe2ea0072e"
license: CC BY-SA 4.0
---

# ACCEPT\_KEYWORDS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



The `ACCEPT_KEYWORDS` variable informs the package manager which ebuilds' `[KEYWORDS](https://wiki.gentoo.org/wiki/KEYWORDS)` values it is allowed to accept. This variable is used to select either [stable](https://wiki.gentoo.org/wiki/Handbook:X86/Portage/Branches#Stable) or [testing](https://wiki.gentoo.org/wiki/Handbook:X86/Portage/Branches#Testing) branch as default.

## Where the variable is set?

The variable is usually set through the Gentoo [profile](https://wiki.gentoo.org/wiki/Portage/Profiles) but can be overruled *system wide* in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf), *per-package* in [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords), or even *for a single emerge* on the command line, though this is not recommended.

## Stable and unstable keywords

The default value of most profiles' `ACCEPT_KEYWORDS` variable is the architecture itself, like **amd64** or **arm**. In these cases, the package manager will only accept ebuilds whose `KEYWORDS` variable contains this architecture. If the user wants to be able to install and work with ebuilds that are not considered production-ready yet, they can add the same architecture but with the `~` prefix to it, like so:

```
ACCEPT_KEYWORDS="~amd64"
```
One should not specify the stable keyword (**amd64**) when adding the testing keyword (**\~amd64**) because `ACCEPT_KEYWORDS` is an incremental variable.

If the setting is not to be made system-wide, then it can be set per-package in the package.accept\_keywords file or directory:

```
# games
games-fps/doomsday ~amd64
```
In addition to the normal values from `ACCEPT_KEYWORDS`, package.accept\_keywords supports three special tokens<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

- `*` — Package is visible if it is stable on any architecture.
- `~*` — Package is visible if it is in testing on any architecture.
- `**` — Package is always visible (`KEYWORDS` are ignored completely).

The last choice is useful for live package versions (e.g. SVN/Git/Mercurial package versions) because live ebuilds don't have a `KEYWORDS` variable.

## See also

- [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) — files or directories of files containing definitions for per-package `ACCEPT_KEYWORDS` statements.
- [KEYWORDS](https://wiki.gentoo.org/wiki/KEYWORDS) — the `KEYWORDS` variable informs in which [architectures](https://wiki.gentoo.org/wiki/Handbook:Main_Page#Architectures) the ebuild is stable or still in testing phase.
- [Knowledge Base:Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package)
- [Knowledge Base:Accepting a keyword for all packages](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_all_packages)

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Gentoo Portage, [Manual page for Portage](https://dev.gentoo.org/~zmedico/portage/doc/man/portage.5.html). Retrieved on January 30th, 2015.
