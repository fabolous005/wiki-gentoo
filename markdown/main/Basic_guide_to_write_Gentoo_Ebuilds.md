<!-- source: https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds | group: Gentoo Wiki (Main) | wiki-title: Basic guide to write Gentoo Ebuilds -->
---
title: Basic guide to write Gentoo Ebuilds
url: https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-21"
categories: ['Michał Górny: Category: Ebuild writing']
fingerprint: "9fa93a1e94b923aa"
license: CC BY-SA 4.0
---

# Basic guide to write Gentoo Ebuilds

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a guide to getting started writing **[ebuilds](https://wiki.gentoo.org/wiki/Ebuild)**, to harness the power of [Portage](https://wiki.gentoo.org/wiki/Portage), to install and manage even more software.

Write an ebuild to install a piece of software on Gentoo, when there are no suitable preexisting ebuilds. It's a relatively straight forward task, and is the only way to cleanly install most "third party" software system-wide. The ebuild will allow the [package manager](https://wiki.gentoo.org/wiki/Project:Package_Manager_Specification#Implementation_in_package_managers) to track every file installed to the system, to allow clean updates and removal.

Once an ebuild is working, it can be shared by submitting it in a [pull request](https://wiki.gentoo.org/wiki/GitHub_Pull_Requests) or in a separate [ebuild repository](https://repos.gentoo.org/) and making it accessible publicly. With a little effort, ebuilds can be proposed and maintained in the [GURU](https://wiki.gentoo.org/wiki/GURU) repository.

In order for ebuilds to be available to [Portage](https://wiki.gentoo.org/wiki/Portage), they are placed in an [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) that is configured for Portage through [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) (see the section on [repository management](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_management) for general information about working with ebuild repositories).

[Create](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository#Creating_an_empty_repository) an ebuild repository to experiment in, while following on with this guide. The rest of the article will consider a repository in /var/db/repos/example\_repository.

[eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) makes creating a repository simple:

`root #``emerge --ask app-eselect/eselect-repository``root #``eselect repository create example_repository`
It is much easier to maintain your own repository using your user account. To do this, change the owner of the newly created repository (replace larry with your own username):

`root #``chown larry -R /var/db/repos/example_repository`
Ebuilds are simply text files, in their most basic form. All that is needed to start writing ebuilds is a [text editor](https://wiki.gentoo.org/wiki/Text_editor), to provide installable software packages for Gentoo.

Some editors have optional ebuild functionality. In that case, skip to the appropriate section, otherwise a skeleton ("template") may be used to get started quicker.

If the editor does not have integrated ebuild functionality to help to start off, there is a skeleton ebuild file (skel.ebuild) located in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository). To start with that file as a base, simply copy it to an appropriate location (nano is used as the text editor in this example):

`user $````
mkdir --parents /var/db/repos/example_repository/{CATEGORY}/{PN}
```
`user $````
cp /var/db/repos/gentoo/skel.ebuild /var/db/repos/example_repository/{CATEGORY}/{PN}/{P}.ebuild
```
`user $````
cd /var/db/repos/example_repository/{CATEGORY}/{PN}
```
`user $````
nano {P}.ebuild
```
There is a vim plugin to automatically start from a skeleton when creating an empty ebuild file.

After installing [app-vim/gentoo-syntax](https://packages.gentoo.org/packages/app-vim/gentoo-syntax), create the appropriate directory for the ebuild, then launch [vim](https://wiki.gentoo.org/wiki/Vim) with a new {P}.ebuild filename provided on the command line, to be automatically met with a basic skeleton that can be modified and saved:

`user $````
mkdir --parents /var/db/repos/example_repository/{CATEGORY}/{PN}
```
`user $````
cd /var/db/repos/example_repository/{CATEGORY}/{PN}
```
`user $``vim {P}.ebuild`
A similar tool is available for users of [Emacs](https://wiki.gentoo.org/wiki/Emacs), provided by [app-emacs/ebuild-mode](https://packages.gentoo.org/packages/app-emacs/ebuild-mode) or [app-xemacs/ebuild-mode](https://packages.gentoo.org/packages/app-xemacs/ebuild-mode), depending on Emacs distribution.

There is [a language server for gentoo ebuild](https://github.com/termux/termux-language-server).

This example will create an ebuild for [scrub](https://github.com/chaos/scrub), version 2.6.1 (if it didn't already exist), to show how a typical process might go.

Create a directory to house the ebuild, in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) created earlier:

`user $``mkdir -p /var/db/repos/example_repository/app-misc/scrub`
Change the shell working directory to the new path:

`user $``cd /var/db/repos/example_repository/app-misc/scrub`
This example will use Vim to create the ebuild file and provide a skeleton to serve as a basis to write the ebuild on, but use editor of choice (see previous section about using Emacs, or the skeleton file):

`user $``vim ./scrub-2.6.1.ebuild`
Add important information about the new package by setting the [ebuild-defined variables](https://devmanual.gentoo.org/ebuild-writing/variables/index.html#ebuild-defined-variables): DESCRIPTION, [HOMEPAGE](https://projects.gentoo.org/qa/policy-guide/other-metadata.html#pg0702), [SRC\_URI](https://projects.gentoo.org/qa/policy-guide/ebuild-format.html#pg0104), [LICENSE](https://projects.gentoo.org/qa/policy-guide/other-metadata.html#pg0704). Licenses like BSD-clause-3 which are not found in [the tree](https://gitweb.gentoo.org/repo/gentoo.git/tree/licenses) might be mapped in [metadata](https://gitweb.gentoo.org/repo/gentoo.git/tree/metadata/license-mapping.conf) :

**`scrub-2.6.1.ebuild`**

**vim editing a new file from template**

```
# Copyright 2026 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
 
EAPI=8
 
DESCRIPTION="Some words here"
HOMEPAGE="https://github.com/chaos/scrub"
SRC_URI="https://github.com/chaos/scrub/releases/download/2.6.1/scrub-2.6.1.tar.gz"
 
LICENSE="GPL-2"
SLOT="0"
KEYWORDS="~amd64 ~x86"
IUSE=""
 
DEPEND=""
RDEPEND="${DEPEND}"
BDEPEND=""
```
This — with the omission of those lines with `=""` — is the minimum information necessary to get something that will work.  Ebuilds inheriting certain eclasses might come with a different set of minimal information, e.g. [ant-jsch-1.10.9.ebuild](https://gitweb.gentoo.org/repo/gentoo.git/tree/dev-java/ant-jsch/ant-jsch-1.10.9.ebuild). Save the file - voila an ebuild, in its most basic form, it's that simple!

It is possible to test fetching and unpacking the upstream sources by the new ebuild, using the [ebuild](https://dev.gentoo.org/~zmedico/portage/doc/man/ebuild.1.html) command:

`user $``GENTOO_MIRRORS="" ebuild ./scrub-2.6.1.ebuild manifest clean unpack`
Appending /var/db/repos/customrepo to PORTDIR\_OVERLAY...
>>> Downloading 'https://github.com/chaos/scrub/releases/download/2.6.1/scrub-2.6.1.tar.gz'
--2023-03-03 23:35:13--  https://github.com/chaos/scrub/releases/download/2.6.1/scrub-2.6.1.tar.gz
Resolving github.com... 140.82.121.4
Connecting to github.com|140.82.121.4|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/23157201/405a65b8-2d4d-11e4-8f82-3e3a9951b650?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20230303%2Fus-east-1%2Fs3%2Faws4\_request&X-Amz-Date=20230303T223513Z&X-Amz-Expires=300&X-Amz-Signature=7d7d925ff8392ee2ba12028c73c8d8c3b3a7086b5aec11bbfae335222a4f2eb0&X-Amz-SignedHeaders=host&actor\_id=0&key\_id=0&repo\_id=23157201&response-content-disposition=attachment%3B%20filename%3Dscrub-2.6.1.tar.gz&response-content-type=application%2Foctet-stream \[following\]
--2023-03-03 23:35:13--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/23157201/405a65b8-2d4d-11e4-8f82-3e3a9951b650?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20230303%2Fus-east-1%2Fs3%2Faws4\_request&X-Amz-Date=20230303T223513Z&X-Amz-Expires=300&X-Amz-Signature=7d7d925ff8392ee2ba12028c73c8d8c3b3a7086b5aec11bbfae335222a4f2eb0&X-Amz-SignedHeaders=host&actor\_id=0&key\_id=0&repo\_id=23157201&response-content-disposition=attachment%3B%20filename%3Dscrub-2.6.1.tar.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 362536 (354K) \[application/octet-stream\]
Saving to: '/var/cache/distfiles/scrub-2.6.1.tar.gz.\_\_download\_\_'
 
/var/cache/distfiles/scrub-2.6.1. 100%\[============================================================>\] 354.04K  --.-KB/s    in 0.08s   
 
2023-03-03 23:35:13 (4.31 MB/s) - '/var/cache/distfiles/scrub-2.6.1.tar.gz.\_\_download\_\_' saved \[362536/362536\]
 
 \* scrub-2.6.1.tar.gz BLAKE2B SHA512 size ;-) ...                                                                               \[ ok \]
>>> Unpacking source...
>>> Unpacking scrub-2.6.1.tar.gz to /var/tmp/portage/app-misc/scrub-2.6.1/work
>>> Source unpacked in /var/tmp/portage/app-misc/scrub-2.6.1/work

This should download and unpack the source tarball, without error, as in the example output.

Also, there's [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) for creating Manifest files:

`user $``pkgdev manifest -d ~/a/folder/for/distfiles ./app-misc/scrub/scrub-2.6.1.ebuild`
For some exceptionally simple packages like this one, that do not need patching or other more advanced treatment, the ebuild may work just so - with no further adjustments needed.

For best practice, the test suite may be run at this stage - this is particularly true when starting out:

`root #``ebuild scrub-2.6.1.ebuild clean test install`
To actually install the new ebuild on the system, run:

`root #``ebuild scrub-2.6.1.ebuild clean install merge`
A patch can be created from the unpacked source code as explained in the [Creating a patch](https://wiki.gentoo.org/wiki/Creating_a_patch) article. Patches should then be put in the files directory and be listed in an array called `PATCHES` as explained in the [devmanual](https://devmanual.gentoo.org/ebuild-writing/functions/src_prepare/eapply/index.html):

Use [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) ([dev-util/pkgcheck](https://packages.gentoo.org/packages/dev-util/pkgcheck)) to check for QA errors in an ebuild:

`user $``pkgcheck scan`
- [GitHub Pull Requests](https://wiki.gentoo.org/wiki/GitHub_Pull_Requests) — how to contribute to Gentoo by creating [pull requests on GitHub](https://github.com/gentoo/gentoo/pulls).
- [java-ebuilder](https://wiki.gentoo.org/wiki/Java-ebuilder) — an experimental package being developed by Gentoo Java developers to generate initial ebuilds from [Maven](https://wiki.gentoo.org/wiki/Maven) `pom.xml` files.
- [Notes on ebuilds with GUI](https://wiki.gentoo.org/wiki/Notes_on_ebuilds_with_GUI)
- [Project:GURU](https://wiki.gentoo.org/wiki/Project:GURU) — an official repository of new Gentoo packages that are maintained collaboratively by Gentoo users
- [Project:Proxy\_Maintainers/User\_Guide/Style\_Guide](https://wiki.gentoo.org/wiki/Project:Proxy_Maintainers/User_Guide/Style_Guide)
- [Project:Python](https://wiki.gentoo.org/wiki/Project:Python) — the Python project pages have information on creating ebuilds for packages written in Python
- [Project:X11/Ebuild\_maintenance](https://wiki.gentoo.org/wiki/Project:X11/Ebuild_maintenance)
- [Proxied Maintainer FAQ](https://wiki.gentoo.org/wiki/Proxied_Maintainer_FAQ)
- [Test environment](https://wiki.gentoo.org/wiki/Test_environment)
- [Writing go Ebuilds](https://wiki.gentoo.org/wiki/Writing_go_Ebuilds) — a short reference, intended to be read alongside [Basic guide to write Gentoo Ebuilds] and the [go-module.eclass documentation](https://devmanual.gentoo.org/eclass-reference/go-module.eclass/index.html)
- [Ebuild\_guidance\_for\_ecosystems](https://wiki.gentoo.org/wiki/Ebuild_guidance_for_ecosystems)
