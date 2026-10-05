<!-- source: https://wiki.gentoo.org/wiki/Proxied_Maintainer_FAQ | group: Gentoo Wiki (Main) | wiki-title: Proxied Maintainer FAQ -->
---
title: Proxied Maintainer FAQ
url: https://wiki.gentoo.org/wiki/Proxied_Maintainer_FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-18"
fingerprint: f7a51a3fa231470e
license: CC BY-SA 4.0
---

# Proxied Maintainer FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Proxied maintainers and Gentoo developers are invited to work on this page and collect FAQ and common mistakes here. Please also see the [style guide](https://wiki.gentoo.org/wiki/Project:Proxy_Maintainers/User_Guide/Style_Guide).

### Adding acct-{group,user}/packages as a proxied maintainer

Check the latest state of [uid-gid.txt](https://gitweb.gentoo.org/data/api.git/tree/files/uid-gid.txt) file, and choose the first available number when counting downwards from 500. There's a helper script in data/api.git, `./bin/	used_free_uidgids.sh` to aid with picking free numbers. 
Creating a pull request to api.gentoo.org reserving an ID is not necessary - if it's taken while your submission is being reviewed, new first available number will be assigned while merging your contribution.

### Breaking tests in parallel building mode

Tests after an ebuild has successfully finished the compile phase run in the same mode (environment). Individual (source-) files can be translated and compiled concurrently and independently from each other. However test-commands can fail when run in parallel!

Parallelism for the [**make** utility](https://www.gnu.org/software/make/) is handled via the [**-j or --jobs Option to make**](https://www.gnu.org/software/make/manual/html_node/Parallel.html) i.e.: `MAKEOPTS`.

In those cases the tests and test-commands should be run with the **--jobs** option explicitly set to **1** i.e.: `-j1`.

Alternatively:

Using the [**cmake.eclass**](https://devmanual.gentoo.org/eclass-reference/cmake.eclass/index.html) allows for a direct setting of the **--jobs** Option `-j1`:

### "Copy/paste" style of contributions

If you commit new version bump and base your new ebuild on an old ebuild that uses old EAPI and has multiple modern QA issues, you are asked to update your new ebuild contribution to match today's Gentoo standards. However if you fix a bug on a current ebuild that requires a revbump and therefore a new ebuild copy, we will use common sense and accept the contribution to current ebuild revision.

Unfortunately ::gentoo tree is full of outdated ebuilds by modern standards. You can read the [devmanual](https://devmanual.gentoo.org/) and [QA Policy Guide](https://projects.gentoo.org/qa/policy-guide/), in addition to EAPI [6](https://blogs.gentoo.org/mgorny/2015/11/13/the-ultimate-guide-to-eapi-6/) & [7](https://dev.gentoo.org/~mgorny/articles/the-ultimate-guide-to-eapi-7.html) guides if unsure about state of the current ebuild you're working on.

### Handling an extra file which size exceeds 20 KiB

Per [devmanual](https://devmanual.gentoo.org/ebuild-maintenance/new-ebuild/index.html) a file inside `${FILESDIR}` mustn't exceed the file size of 20 KiB. These files get synced to everyone, yet not many users necessarily need them. They influence the overall size of portage tree, and worldwide bandwith caps.

You can host a file exceeding 20 KiB in your Github, Gitlab etc hosting service, add it to `SRC_URI` and call either `PATCHES=( "${DISTDIR}/${P}-fix-qt-5.15-headers.patch" )` or `eapply "${DISTDIR}/${P}-fix-qt-5.15-headers.patch"`. This is flagged by the [dev-util/pkgcheck](https://packages.gentoo.org/packages/dev-util/pkgcheck) tool.

### Keywording after EAPI bump

An EAPI bump requires usually that all keywords are set to \`\~\` (unstable). This can be done with:

`user $``ekeyword ~all foo-2.0.ebuild`
From the package [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit).

However there are some special cases where a stabilization can remain, if the developer can be 100% sure, that the EAPI change does not change anything. But this is not the case for usual packages.

### Retiring from proxied maintainer status

We get it, sometimes life happens, distro hop happens. If you have packages assigned to you and you wish to stop being a proxied maintainer for them, please open a pull request where you update metadata.xml files putting maintainer status to `<!-- maintainer-needed -->`, using a `Closes:` tag against your proxy-maint bug, and sending a ["packages up for grabs" e-mail to gentoo-dev mailing list](https://archives.gentoo.org/gentoo-dev/message/fc7652e75cbb7ee0c2a7e72de91acc04).

Or at least please inform us by messaging your updated status in your proxy-maint bug, and we will do the rest for you.

### Sorting of KEYWORDS

We usually sort the `KEYWORDS` values as ekeyword does. This makes comparison between packages easier. 
Simply run ekeyword on the ebuild to let it sort.

`user $` `ekeyword foo-2.0.ebuild`
### Use the latest EAPI for a pull request

We always try to use the latest EAPI if possible. If a required eclass is not yet compatible with the latest EAPI this can not be fulfilled directly.

### Using a sub-slot binder

[Refer to devmanual](https://devmanual.gentoo.org/general-concepts/slotting/index.html#Sub-Slots) in case sub-slots are not familiar to you. Sub-slot binders should only be used when your program links to specific SONAME of a library during build-time. It can be used even if the dependency doesn't have a subslot already defined, as long as your program properly links to a library.

### When an ebuild needs a revbump (-r1)

Refer to the [devmanual](https://devmanual.gentoo.org/general-concepts/ebuild-revisions/index.html).

### When and how to stabilize a package?

[Common rules for stabilization](https://wiki.gentoo.org/wiki/Stable_request). If you don't want to keep a calendar and count days for stabilization, [dev-util/pkgcheck](https://packages.gentoo.org/packages/dev-util/pkgcheck) tool can be utilized to help.

`user $``pkgcheck scan app-emulation/lxd dev-libs/efl dev-python/python-efl media-libs/rlottie -c StableRequestCheck`
app-emulation/lxd
  StableRequest: version 4.0.1: slot(0) no change in 30 days for unstable keyword: \[ \~amd64 \]
dev-python/python-efl
  StableRequest: version 1.24.0: slot(0) no change in 45 days for unstable keywords: \[ \~amd64, \~x86 \]

Stabilization is [requested through bugzilla](https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide#Request_stabilization) which uses [nattka tool](https://github.com/mgorny/nattka/#filing-keywordingstabilization-bugs) for its syntax. If nattka informs that sanity check succeeds, sit back and relax, arch teams will handle the stabilization next. If sanity check doesn't succeed, most likely some required dep is [not stabilized/keyworded](https://devmanual.gentoo.org/keywording/index.html#equal-visibility-requirement) yet, and you will either need to handle it in a new bug, or add it to package list.
