<!-- source: https://wiki.gentoo.org/wiki/Lightweight_CVS_Checkout | group: Gentoo Wiki (Main) | wiki-title: Lightweight CVS Checkout -->
---
title: Lightweight CVS Checkout
url: https://wiki.gentoo.org/wiki/Lightweight_CVS_Checkout
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-08"
fingerprint: "1b87918f2de5ab40"
license: CC BY-SA 4.0
---

# Lightweight CVS Checkout

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Gentoo developers willing to work with the ebuilds often check out the complete gentoo-x86 CVS repository. This page describes a method of creating and maintaining a *lightweight CVS checkout* backed up by regular (rsync or git) Gentoo repository.

## Why lightweight checkout?

Unlike standard CVS checkout which involves fetching all the files from the gentoo-x86 repository, the lightweight method allows you to get only the files you are working on. The remaining files are obtained from a regular repository — obtained through rsync, git, webrsync…

The advantages of this method are:

- bandwidth and space efficiency — only needed files are fetched and stored,
- improved performance — only modified files need to be recached, the supplied md5-cache is used for the remaining ebuilds,
- improved maintainability — there's no longer need to keep all ebuilds up-to-date in the CVS checkout. Instead, just remove them when done and let the regularly synced repository supply them.

The disadvantages of this method are:

- necessity of manually fetching additional files as they are worked on,
- inability to run tree-wide repoman scans without merging two repositories.

## How to create a lightweight checkout?

### Before starting

Ensure that you have a Gentoo repository synced via method other than CVS. If you used to sync against CVS, please switch your Package Manager to use rsync, git or another supported method instead.

### Automated script

The repository can be easily created using *lcvs-init* script from [lightweight-cvs-toolkit](https://bitbucket.org/mgorny/lightweight-cvs-toolkit).

`user $``cd lightweight-cvs-toolkit``user $``./lcvs-init /home/myuser/gentoo-x86 myuser`
The script takes three parameters:

1. location where the repository should be created,
2. CVS username (optional, defaults to current user's username),
3. Repository name (optional, defaults to *gentoo-cvs*).

Before creating the repository, the script will check system sanity, output the resulting configuration and ask the user for confirmation.

### Manual method

Then create a new CVS checkout using the *-l* option (local) to disable recursive fetching:

`user $``cvs -dmyuser@cvs.gentoo.org:/var/cvsroot co -l gentoo-x86`
(change *myuser* to your Gentoo username)

It is also convenient to fetch (non-recursively) directories for all ebuild categories. For example:

`user $``cd gentoo-x86``user $``cvs up -dl $(</usr/portage/profiles/categories)`
Afterwards, the metadata for repository should be fetched and layout.conf file updated to state a new repository name, and name *gentoo* as its master repository. This will allow the checkout to be used as a regular overlay on top of the rsync/git repository.

`user $``cvs up -dP metadata``user $``vim metadata/layout.conf`
**`metadata/layout.conf`**

**Additions to layout.conf**

It is recommended to consistently use the name *gentoo-cvs* since that provides a convenient way of locating the CVS checkout for scripts.

At this point the repository is ready. It is also a good idea to create a git repository on top of it and store the current state. This can be used to conveniently remove checked out ebuild later and restore the repository to vanilla state.

`user $``git init``user $``git add -A``user $``git commit -m "Initialize CVS checkout"`
Finally, add the repository to repos.conf:

**`/etc/portage/repos.conf/cvs.conf`**

**Configuration for repos.conf**

## Working with a partial checkout

Since partial checkout doesn't have any ebuilds, eclasses, licenses or profiles by default (inherits them all from *gentoo* implicitly), the files being worked need to be fetched manually. *cvs up -dP* can be used for this. For example, if you want to work on dev-foo/bar, you'd do:

`user $``cd ~/gentoo-x86``user $``cd dev-foo``user $``cvs up -dP bar`
You can also pass multiple directories to *cvs up*:

`user $``cd ~/gentoo-x86``user $``cvs up -dP dev-foo/bar dev-bar/baz profiles`
When done working with the files, it is useful to remove them. Otherwise, the old versions will confuse your Package Manager in the future.

`user $``rm -r dev-foo/bar dev-bar/baz profiles`
If you used git during the repository creation (both methods above do), you can also let it clean any new files:

`user $``git clean -df`
