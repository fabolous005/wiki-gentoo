<!-- source: https://wiki.gentoo.org/wiki/License_groups | group: Gentoo Wiki (Main) | wiki-title: License groups -->
---
title: License groups
url: https://wiki.gentoo.org/wiki/License_groups
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-19"
fingerprint: ed455ad8cd0bbd98
license: CC BY-SA 4.0
---

# License groups

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**License groups** enable the system's package manager to allow or disallow the installation of certain categories of software based on license compatibility. License groups are generally divided into categories by license compatibility with legal standards. Organizations that advocate software freedom also maintain lists of software that meets certain key criteria.

For example, The Free Software Foundation maintains their own FSF-approved list<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> consisting of GPL-compatible<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, GPL-incompatible<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>, and nonfree<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> licenses.

## Existing license groups

Defined in [profiles/license\_groups](https://gitweb.gentoo.org/repo/gentoo.git/tree/profiles/license_groups) (history: [CVS](https://sources.gentoo.org/cgi-bin/viewvc.cgi/gentoo-x86/profiles/license_groups?view=log), [Git](https://gitweb.gentoo.org/repo/gentoo.git/log/profiles/license_groups)).

### Free software

- [GPL-COMPATIBLE](https://wiki.gentoo.org/wiki/License_groups/GPL-COMPATIBLE)
- GPL-compatible free software licenses approved by the Free Software Foundation [\[1\]](https://www.gnu.org/licenses/license-list.html#GPLCompatibleLicenses).
- [FSF-APPROVED](https://wiki.gentoo.org/wiki/License_groups/FSF-APPROVED)
- This includes all licenses from @GPL-COMPATIBLE, and GPL-incompatible free software licenses that are approved by the FSF [\[2\]](https://www.gnu.org/licenses/license-list.html#GPLIncompatibleLicenses).
- [OSI-APPROVED-FREE](https://wiki.gentoo.org/wiki/License_groups/OSI-APPROVED#OSI-APPROVED-FREE)
- Open-source licenses approved by the Open Source Initiative [\[3\]](https://opensource.org/licenses). (FSF-APPROVED and OSI-APPROVED-FREE may have elements in common.)
- [MISC-FREE](https://wiki.gentoo.org/wiki/License_groups/MISC-FREE)
- Misc licenses that are probably free software, i.e. follow the [free software definition](https://www.gnu.org/philosophy/free-sw.html) but are not approved by either FSF or OSI. Preferably on the long term these should be cleared up and moved to other sets. Licenses in this list should *not* appear directly or indirectly in FSF-APPROVED or OSI-APPROVED-FREE.

### Free documents

- [FSF-APPROVED-OTHER](https://wiki.gentoo.org/wiki/License_groups/FSF-APPROVED-OTHER)
- FSF-approved licenses for “free documentation” [\[4\]](https://www.gnu.org/licenses/license-list.html#FreeDocumentationLicenses) and “works of practical use besides software and documentation” (including fonts) [\[5\]](https://www.gnu.org/licenses/license-list.html#OtherLicenses).
- [MISC-FREE-DOCS](https://wiki.gentoo.org/wiki/License_groups/MISC-FREE-DOCS)
- Misc licenses for free documents and other works (including fonts) that follow the definition at [https://freedomdefined.org/](https://freedomdefined.org/) but are *not* listed in @FSF-APPROVED-OTHER.

### Metasets

- FREE-SOFTWARE
- Metaset for all free software: @FSF-APPROVED @OSI-APPROVED-FREE @MISC-FREE
- FREE-DOCUMENTS
- Metaset for all free documents: @FSF-APPROVED-OTHER @MISC-FREE-DOCS
- FREE
- Collection of all licenses with the freedom to use, share, modify and share modifications. This is a metaset including @FREE-SOFTWARE and @FREE-DOCUMENTS.

### Others

- [BINARY-REDISTRIBUTABLE](https://wiki.gentoo.org/wiki/License_groups/BINARY-REDISTRIBUTABLE)
- As proposed [\[6\]](https://archives.gentoo.org/gentoo-dev/msg_6c950b46c50fe72ebc5e650bbf70f77c.xml). Excerpt of the rules for this license group:
  - *MUST* permit redistribution in binary form.
  - *MUST NOT* require explicit approval (no items from @EULA).
  - *MUST NOT* restrict the cost of redistribution.
  - *MAY* require explicit inclusion of the license with the distribution.
  - IF (and only if) there is an explicit inclusion requirement, `USE=bindist` *MUST* cause a copy of the license to be installed in a file location compliant with the license.
- This group includes all licenses from @FREE.
- [OSI-APPROVED-NONFREE](https://wiki.gentoo.org/wiki/License_groups/OSI-APPROVED#OSI-APPROVED-NONFREE)
- Licenses approved by the Open Source Initiative that the FSF lists as nonfree.
- [OSI-APPROVED](https://wiki.gentoo.org/wiki/License_groups/OSI-APPROVED)
- Metaset for all licenses approved by the Open Source Initiative: @OSI-APPROVED-FREE @OSI-APPROVED-NONFREE
- [EULA](https://wiki.gentoo.org/wiki/License_groups/EULA)
- License agreements that try to take away your rights. These are more restrictive than "all-rights-reserved" or require explicit acceptance by the user.
- [Nonfree](https://wiki.gentoo.org/wiki/License_groups/Nonfree)
- Miscellaneous nonfree licenses that don’t belong to any license group.

## When is a license a *free software license?*

A free software license must grant the ”four freedoms“ to the program’s users, as described in the [free software definition](https://www.gnu.org/philosophy/free-sw.html):

1. The freedom to run the program, for any purpose (freedom 0).
2. The freedom to study how the program works, and change it so it does your computing as you wish (freedom 1). Access to the source code is a precondition for this.
3. The freedom to redistribute copies so you can help your neighbor (freedom 2).
4. The freedom to distribute copies of your modified versions to others (freedom 3). By doing this you can give the whole community a chance to benefit from your changes. Access to the source code is a precondition for this.

[DFSG FAQ](https://people.debian.org/~bap/dfsg-faq.html)):

- The **“Desert island”** test. Imagine a castaway on a desert island with a solar-powered computer. This would make it impossible to fulfill any requirement to make changes publicly available or to send patches to some particular place. This holds even if such requirements are only upon request, as the castaway might be able to receive messages but be unable to send them. To be free, software must be modifiable by this unfortunate castaway, who must also be able to legally share modifications with friends on the island.
- The **“Dissident”** test. Consider a dissident in a totalitarian state who wishes to share a modified bit of software with fellow dissidents, but does not wish to reveal the identity of the modifier, or directly reveal the modifications themselves, or even possession of the program, to the government. Any requirement for sending source modifications to anyone other than the recipient of the modified binary – in fact any forced distribution at all, beyond giving source to those who receive a copy of the binary – would put the dissident in danger. For Debian to consider software free it must not require any such ”excess“ distribution.
- The **“Tentacles of evil”** test. Imagine that the author is hired by a large evil corporation and, now in their thrall, attempts to do the worst to the users of the program: to make their lives miserable, to make them stop using the program, to expose them to legal liability, to make the program non-free, to discover their secrets, etc. The same can happen to a corporation bought out by a larger corporation bent on destroying free software in order to maintain its monopoly and extend its evil empire. The license cannot allow even the author to take away the required freedoms!

## See also

- [GLEP 23](https://www.gentoo.org/glep/glep-0023.html)
- [/etc/portage/license\_groups](https://wiki.gentoo.org/wiki//etc/portage/license_groups) — a file containing groups of licenses that may be specified in the `[ACCEPT_LICENSE](https://wiki.gentoo.org/wiki//etc/portage/make.conf#ACCEPT_LICENSE)` variable.
