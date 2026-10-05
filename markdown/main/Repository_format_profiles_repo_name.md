<!-- source: https://wiki.gentoo.org/wiki/Repository_format/profiles/repo_name | group: Gentoo Wiki (Main) | wiki-title: Repository format/profiles/repo name -->
---
title: Repository format/profiles/repo_name
url: https://wiki.gentoo.org/wiki/Repository_format/profiles/repo_name
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: "9b07909d0ca0bac4"
license: CC BY-SA 4.0
---

# Repository format/profiles/repo\_name

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The profiles/repo\_name file (*obligatory*) specifies the repository name. The name assigned there is used by the package manager to refer to the contents of the repository. In case a [repo-name](https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf#repo-name) key is given in [metadata/layout.conf](https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf) that one takes precedence.

## File format

The *repo\_name* file is a plain, ASCII text file containing the repository name on its only line.

**`profiles/repo_name`**

**Example for repository named*my-repo***

For configuration files or sections in  [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf#Format) it is recommended that `[repository_name]` is the same as the name specified here.

## eselect-repository repository list

It is required that repository names in repo\_name files are the same as repository names on the [eselect-repository](https://wiki.gentoo.org/wiki/Eselect-repository) repository list. In the future, an automated check will be added to enforce that as requested in [bug #382603](https://bugs.gentoo.org/show_bug.cgi?id=382603).
