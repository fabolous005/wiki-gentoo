<!-- source: https://wiki.gentoo.org/wiki/Revision_bump | group: Gentoo Wiki (Main) | wiki-title: Revision bump -->
---
title: Revision bump
url: https://wiki.gentoo.org/wiki/Revision_bump
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-10"
fingerprint: be4aba6fe4b94706
license: CC BY-SA 4.0
---

# Revision bump

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Revision bumping**, also known as *revbumping*, is the act of updating an [ebuild](https://wiki.gentoo.org/wiki/Ebuild) to a new revision. This article describes the procedure to be followed.

Revbumping may be required upon request from a package maintainer. Additional reasons to perform revbumps are outlined at the [Ebuild revisions](https://devmanual.gentoo.org/general-concepts/ebuild-revisions/index.html) page in the development guide.

Revbumps usually accompany changes to other files in the tree, such as files under files/.

When making changes that require a revbump, those changes should not be made in place. Instead, the following steps should be followed:

1. Copy the latest revisions of the files.
2. Increment their revision numbers.
3. Add any modifications to these copies.

For files that do not have revision numbers, append `-r1`:

These files are not used in any ebuilds yet. The next section explains how to create new ebuilds that use these files.

Similarly to files, ebuild changes that require a revbump should be applied to *copies* of those ebuilds, not the original ebuilds. The following steps should be followed:

1. For each package, copy the latest stable and unstable ebuild.
2. Increment the copies' revision numbers.
3. [Destabilize](https://wiki.gentoo.org/wiki/Stable_request) these copies.
4. Modify the copied ebuilds to use the bumped files from before.

These steps are explained in detail in the following sections.

Ideally, only the latest stable and unstable version of each package should be revbumped, in order not to clutter the [Portage tree](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

The ebuild files themselves need to be inspected to determine their stability. Unstable versions have `~` in front of each architecture in the [`KEYWORDS`](https://wiki.gentoo.org/wiki/KEYWORDS) variable:

**`app-editors/vim-core/vim-core-9.1.0366.ebuild`**

```
KEYWORDS="~alpha ~amd64 ~arm ~arm64 ~hppa ~ia64 ~loong ~m68k ~mips ~ppc ~ppc64 ~riscv ~s390 ~sparc ~x86 ~amd64-linux ~x86-linux ~arm64-macos ~ppc-macos ~x64-macos ~x64-solaris"
```
Stable versions lack `~` in front of some architectures:

**`app-editors/vim-core/vim-core-9.0.2167.ebuild`**

```
KEYWORDS="~alpha amd64 arm arm64 hppa ~ia64 ~loong ~m68k ~mips ppc ppc64 ~riscv ~s390 sparc x86 ~amd64-linux ~x86-linux ~arm64-macos ~ppc-macos ~x64-macos ~x64-solaris"
```
[Live ebuilds](https://wiki.gentoo.org/wiki/Live_ebuilds) should be modified in place; not copied.

The same rules apply as in section [Bumping files](https://wiki.gentoo.org#Bumping_files), except that the revision numbers belong *before* the file extensions.

The modifications to these new ebuilds and the live ebuilds are explained in the following two sections.

`~` should be present in front of each arch that does not already have some prefix in `KEYWORDS`.

Find lines that reference the files that were bumped. Modify these to reference the new files. For example:

Another example:
