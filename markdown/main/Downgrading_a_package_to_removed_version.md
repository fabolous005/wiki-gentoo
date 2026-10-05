<!-- source: https://wiki.gentoo.org/wiki/Downgrading_a_package_to_removed_version | group: Gentoo Wiki (Main) | wiki-title: Downgrading a package to removed version -->
---
title: Downgrading a package to removed version
url: https://wiki.gentoo.org/wiki/Downgrading_a_package_to_removed_version
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-15"
fingerprint: d7a91c8f84f80be2
license: CC BY-SA 4.0
---

# Downgrading a package to removed version

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Sometimes, an [ebuild](https://wiki.gentoo.org/wiki/Ebuild) or specific package version which has been removed from the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository) might still be needed (ebuilds get removed as part of normal post-stabilization cleanups).

It can, for example, be useful to test an older version to report an issue to upstream, or to keep things working when there's problem with a newer version.

## Steps

Fortunately, nothing is ever lost with [Git](https://wiki.gentoo.org/wiki/Git). An ebuild can be pulled from the git history and placed into an ebuild repository to allow installation of a previously-removed package version.

First, create an ebuild repository ("overlay") locally, where the rescued ebuilds will be stashed, with [eselect-repository](https://wiki.gentoo.org/wiki/Eselect-repository) (may need installation, see article):

`root #``eselect repository create local`
Grab a git clone of the Gentoo repository and store it somewhere convenient, like /home/larry/git/gentoo:

If a clone already exists, update it:

`user $``git pull`
It's helpful if the exact versions needed are known. In this case, suppose zfs-2.1.5.ebuild is needed.

Run git log to find the last commit before it was yanked:

`user $``git log -- sys-fs/zfs/zfs-2.1.5.ebuild````
commit d429ef63a97180e28e18b6bcaea7ca338674e371
Author: Georgy Yakovlev <gyakovlev@gentoo.org>
Date:   Fri Sep 16 16:17:10 2022 -0700
    sys-fs/zfs: drop 2.1.5
    Signed-off-by: Georgy Yakovlev <gyakovlev@gentoo.org>
commit 2f3182056174dace67bb1cd7e4a5c08336a467a8
Author: Sam James <sam@gentoo.org>
Date:   Wed Jun 22 23:35:53 2022 +0100
    sys-fs/zfs: add 2.1.5
    Signed-off-by: Sam James <sam@gentoo.org>
```
In this case, `d429ef63a97180e28e18b6bcaea7ca338674e371` is the commit which dropped sys-fs/zfs/zfs-2.1.5.ebuild, so `d429ef63a97180e28e18b6bcaea7ca338674e371~1` refers to the last commit before it was dropped.

Restore the ebuild:

`user $``git checkout d429ef63a97180e28e18b6bcaea7ca338674e371~1 sys-fs/zfs/zfs-2.1.5.ebuild`
The required ebuild will now be present at sys-fs/zfs/zfs-2.1.5.ebuild.

Copy it to the repository called "local", created earlier:

`root #``mkdir -p /var/db/repos/local/sys-fs/zfs``root #``cp -rv sys-fs/zfs/* /var/db/repos/local/sys-fs/zfs`
Regenerate the Manifest (or use the same git checkout trick to recover the old one):

`root #``ebuild /var/db/repos/local/sys-fs/zfs/zfs-2.1.5.ebuild manifest`
Once done, go back to the clone of `gentoo.git` and restore it to its original state:

`root #``git clean -fdx``root #``git reset --hard origin/master`
Emerge and enjoy!
