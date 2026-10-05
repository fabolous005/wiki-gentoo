<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_all_packages | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Accepting_a_keyword_for_all_packages -->
---
title: Knowledge Base:Accepting a keyword for all packages
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_all_packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-11"
fingerprint: "2ab93a7e03a08b0a"
license: CC BY-SA 4.0
---

# Knowledge Base:Accepting a keyword for all packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Developers or end users might want to [use the latest available packages](https://wiki.gentoo.org/wiki/Handbook:X86/Portage/Branches#Testing) for their entire system, regardless if these packages are already considered production-ready or not. This will result in a system that has more recent software on it, but also receives a much higher update cycle and has a higher risk of system malfunction due to bugs.

## Analysis

By default, Portage will only consider ebuilds whose [KEYWORDS](https://wiki.gentoo.org/wiki/KEYWORDS) variable contains the users' architecture (without a `~` prefix). However, many ebuilds do have a later version for the same architecture, but these versions are not considered production-ready yet or have dependencies that are not production-ready yet. In these cases, the ebuild `KEYWORDS` variable will contain the architecture with a `~` prefix, like so:

\# Example of an ebuilds' KEYWORDS setting for a production-ready usage on amd64/x86 architecture
KEYWORDS="alpha amd64 arm \~sparc x86"
# Example of an ebuilds' KEYWORDS setting for non-production ready usage on amd64/x86 architect
KEYWORDS="\~alpha \~amd64 \~arm \~sparc \~x86"

As can be seen, this prefix may be used on a per-architecture basis (the examples above refer to the prefix on **amd64** and **x86**).

## Resolution

To have the package manager install testing ebuilds by default, add the prefixed architecture to the [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS) setting in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

```
ACCEPT_KEYWORDS="~amd64"
```
By default, this variable will not be declared in /etc/portage/make.conf so users will need to add it themselves.

Now upgrade the system:

`root #``emerge --ask --update --deep --newuse --with-bdeps=y @world`
## See also

- [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS)
- [KEYWORDS](https://wiki.gentoo.org/wiki/KEYWORDS) — the `KEYWORDS` variable informs in which [architectures](https://wiki.gentoo.org/wiki/Handbook:Main_Page#Architectures) the ebuild is stable or still in testing phase.
-  [Knowledge Base:Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package)
