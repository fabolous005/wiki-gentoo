<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_5_tentative_features | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/EAPI 5 tentative features -->
---
title: Future EAPI/EAPI 5 tentative features
url: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_5_tentative_features
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-17"
fingerprint: "8ed53b77ef84b7a8"
license: CC BY-SA 4.0
---

# Future EAPI/EAPI 5 tentative features

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a working page that contains references to all features that have been suggested for EAPI 5.

## Accepted

- Slot operator dependencies
  - [Specification](https://gitweb.gentoo.org/proj/pms.git/commit/?id=e383073de5bef8932f86d4f7d3dd09e7b5fd0c87), [Portage patch](https://gitweb.gentoo.org/proj/portage.git/commit/?id=e4ba8f36e6a4624f4fec61c7ce8bed0e3bd2fa01)
  - [bug #229521](https://bugs.gentoo.org/show_bug.cgi?id=229521)
    - Request to add further explanation for the :\* operator: [\[1\]](https://public-inbox.gentoo.org/gentoo-pms/20120910070727.GC8036@localhost/)

- Sub-slots

- Profile IUSE injection

- econf --disable-silent-rules

- At-most-one-of operator for REQUIRED\_USE

- EBUILD\_PHASE\_FUNC variable

- Mandate GNU find
  - [Specification](https://gitweb.gentoo.org/proj/pms.git/commit/?id=7c3d7eb05685a5eb2ca7e8459299bf3499933fea), no Portage patch needed
  - [bug #384157](https://bugs.gentoo.org/show_bug.cgi?id=384157)

- new\* commands can read from standard input

- Parsing of the EAPI assignment is mandatory

- src\_test support for parallel tests

- Stable use forcing and masking

- Option --host-root for {has,best}\_version

- doheader helper function
  - [Specification](https://gitweb.gentoo.org/proj/pms.git/commit/?id=e0bf16a23cde4c71eefb20e8b388d70aadc4269e), [Portage patch](https://gitweb.gentoo.org/proj/portage.git/commit/?id=e7e4c3720582a7ab938266e50e53d162f5248488)
  - [bug #21310](https://bugs.gentoo.org/show_bug.cgi?id=21310)
    - doinclude rejected by a previous council [\[2\]](https://www.gentoo.org/proj/en/council/meeting-logs/20090423-summary.txt)

- usex helper function

## Rejected

- econf --disable-silent-rules ([see above](https://wiki.gentoo.org#disable-silent-rules))
    - Apply retroactively to EAPI 4?
    - Apply retroactively to all EAPIs? (May be problematic because of additional configure call.)

- User patches
  - [Specification](https://gitweb.gentoo.org/proj/pms.git/commit/?h=archive/eapi-5&id=a8bf7862967cce36b7f1b408934a774126da2538), [Portage no-op dummy stub](https://gitweb.gentoo.org/proj/portage.git/commit/?id=6b4b621f1abcf21d3bfa54b323126a3ef11eb52c)
    - Intrusive.
    - Current wording of the spec requires that every ebuild includes a call to the apply\_user\_patches function in src\_prepare. An alternative would be to apply user patches after src\_prepare as a default, if the ebuild doesn't call the respective function.
    - The spec doesn't provide any kind of epatch function, so we will end up having two copies of epatch, one for user patches, and the other (from eclass) for ebuilds.
    - Are we happy with the name apply\_user\_patches? (epatch\_user? euserpatch?)

- License groups in ebuilds
  - [bug #287192](https://bugs.gentoo.org/show_bug.cgi?id=287192)
    - A simpler solution would be create separate license files like GPL-2+ for the few cases where this is needed. This would have the advantage that it could be applied to all EAPIs.

- EJOBS variable
  - [bug #273101](https://bugs.gentoo.org/show_bug.cgi?id=273101)
  - [gentoo-dev discussion](https://public-inbox.gentoo.org/gentoo-dev/m2wsef4rbg.fsf@gmail.com/)
    - Discussion was almost 4 years ago. Is there (still) consensus?

- Source eclasses only once

- Extended default list of extensions in dohtml
  - [bug #423245](https://bugs.gentoo.org/show_bug.cgi?id=423245)
    - Objections against inclusion of non-standard extensions like .ico have been raised.

- REPOSITORY variable
  - [bug #414813](https://bugs.gentoo.org/show_bug.cgi?id=414813)
    - Controversial, see bug.

- Repository dependencies
  - [bug #414815](https://bugs.gentoo.org/show_bug.cgi?id=414815)
    - Controversial, see bug.

- Cross-compile support

- Directories for use.\* and package.\* in profiles
  - [bug #282296](https://bugs.gentoo.org/show_bug.cgi?id=282296)
    - Need profiles EAPI bump

- make.defaults etc.in ${repository\_path}/profiles
  - [bug #414817](https://bugs.gentoo.org/show_bug.cgi?id=414817)
    - Need profiles EAPI bump

- HDEPEND: host dependencies for cross-compilation
