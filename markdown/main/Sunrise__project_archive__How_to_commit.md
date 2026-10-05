<!-- source: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/How_to_commit | group: Gentoo Wiki (Main) | wiki-title: Sunrise (project archive)/How to commit -->
---
title: Sunrise (project archive)/How to commit
url: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/How_to_commit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-20"
fingerprint: "37b91caf80f943cb"
license: CC BY-SA 4.0
---

# Sunrise (project archive)/How to commit

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

# How To Commit

## What needs to be done beforehand

- You need a bug for (each) ebuild. If there is no bug, file one. Just use [Bugzilla](https://bugs.gentoo.org/) or look at \[wiki:PostingABugWithPybugz\] for more details on how to do this using [PyBugz](https://code.google.com/p/pybugz/).
- Make sure the ebuild is not already in portage.
- Make sure the 'Default Assignee for new Packages' ('maintainer-wanted') is the assignee of the bug. Otherwise you need the agreement of the assignee or to get the assignee or herd to reassign it to 'maintainer-wanted'. The exception to this is for games. Such bugs don't need to be assigned to 'maintainer-wanted' in order to be committed to the Sunrise overlay. Note that you can't change the assignee of a bug, this is done by Bug Wranglers. If you just filed your ebuild bug and it hasn't been wrangled (assigned to 'maintainer-wanted') just yet, you may still add it to Sunrise.
- Make sure the ebuild is not already in the Project Sunrise overlay.
- Write an ebuild following the instructions in WritingTestingEbuilds.
- Read our CodingStandards.

- Post your ebuild in [#gentoo-sunrise](irc://chat.freenode.net/%23gentoo-sunrise) and/or [#gentoo-dev-help](irc://chat.freenode.net/%23gentoo-dev-help) for review. You can also get feedback through other channels—the more review, the better. Substitute **${PF}.ebuild** with your ebuild's filename.

```
  # emerge wgetpaste
  $ wgetpaste ${PF}.ebuild
```
- If you desire to use [dpaste](http://dpaste.com/) as your pastebin and some syntax highlighting:

```
  $ echo DEFAULT_SERVICE=dpaste >> ~/.wgetpaste.conf
  $ wgetpaste -l Bash ${PF}.ebuild
```
- Improve the ebuild based on the suggestions you receive and repost the updated version on [#gentoo-sunrise](irc://chat.freenode.net/%23gentoo-sunrise) after testing your fixes.



## Make sure you have the required software installed

$ emerge -av git

Git is used for checking in/out the overlay.

## Committing with Git

### Clone the Overlay

The branch of the sunrise you will clone here is **not reviewed**. This means that any other sunrise user who has been granted push access to the repository can push changesets here, you will find unreviewed updates to ebuilds which might possibly include harmful code. Handle with extreme care and **do not** use this as your PORTDIR\_OVERLAY! Continue using the reviewed copy of sunrise managed by layman for PORTDIR\_OVERLAY.

$ cd \~ # you can place this where you want of course
  $ git clone git+[ssh://git@git.gentoo.org/proj/sunrise.git](ssh://git@git.gentoo.org/proj/sunrise.git)
  $ cd sunrise

*WARNING: You can not use layman's copy because layman uses the reviewed repo which is not user-writable.*

Before you make any changes to the the repo, first remember to pull new changes. This will reduce the chance that your commits conflict with changesets made by other users:

$ cd \~/sunrise # cd into your local checkout
  $ git pull --rebase # pull new changesets and enforce a serial history (avoiding the need for merge commits)

### Add the Ebuild

We'll use the package app-text/xpdf as an example.

If your working copy of the Sunrise repository doesn't contain a directory corresponding to your package's category, create the category directory:

$ mkdir app-text

Once the category directory is in place, copy your package to it as follows. The following assumes that you were maintaining app-text/xpdf in your local personal overlay before hearing about Sunrise. If you have written your ebuild inside of the Sunrise, ignore the following line.

$ cp -r /usr/local/portage/app-text/xpdf app-text

If your ebuild uses custom USE flags not described in the official Portage profiles' use.desc, you must add descriptions for these flags to your ebuild's metadata.xml. This file must be placed in the same directory as the ebuild you want to commit. If this file is missing, do

$ cd \~/sunrise # Enter your clone of sunrise
  $ cp skel.metadata.xml app-text/xpdf/metadata.xml

to get the basic file.

Using our example app-text/xpdf, suppose that it has custom USE flags called 'foo' and 'bar'. This is a complete metadata.xml with USE flag descriptions:

#!xml
  \<?xml version="1.0" encoding="UTF-8"?>
  \<!DOCTYPE pkgmetadata SYSTEM "[http://www.gentoo.org/dtd/metadata.dtd](http://www.gentoo.org/dtd/metadata.dtd)">
  \<pkgmetadata>
     \<maintainer>
        \<email>maintainer-wanted@gentoo.org\</email>
     \</maintainer>
     \<use>
        \<flag name='bar'>Use the bar interface\</flag>
        \<flag name='foo'>Enables foo provided by \<pkg>app-text/foo\</pkg>\</flag>
     \</use>
  \</pkgmetadata>

If you mention another ebuild in the description, put it inside a \<pkg/> element just like \<pkg>app-text/foo\</pkg> shown above.

*NOTE: Please keep the entries in alphabetical order.*

Do not manually add your useflag description to profiles/use.local.desc — this is handled automatically by scripts when a developer performs a review.

### Generate ChangeLog and Manifest and Check for QA Problems

Sunrise uses a special tool called **sunrise-commit** to make commits more convenient. This tool is bundled in the package app-portage/overlay-utils which can be found in the Sunrise overlay. Install it now:

\# GENTOO\_MIRRORS= emerge app-portage/overlay-utils

The sunrise-commit script wraps several Portage utilities so, to ensure you have the proper permissions, add yourself to the *portage* group. (Replace 'username' with your local login name)

\# gpasswd -a username portage

**Important: Every new ebuild and every major change to an existing ebuild needs a review in [#gentoo-sunrise](irc://chat.freenode.net/%23gentoo-sunrise) and a developer ok before commit.** Version bumps where nothing changes but the filename of the ebuild generally do not require a review. However, if the ebuild has not been touched for a long time it does not hurt to re-review it.

Set your git user name (first time only). Please use your full real name and a valid email address. If your ircnick is the same as your email address's username component, omit the (ircnick). Try to match the values you use in ECHANGELOG\_USER below:

$ git config --global user.name "Your Name (ircnick)"
  $ git config --global user.email "user@example.org"

Set ECHANGELOG\_USER for future login sessions by appending the following line to your \~/.bash\_profile. Please try to match the values you configured with git above:

#!sh
  export ECHANGELOG\_USER="Your Name (ircnick) \<user@example.org>"

Continuing our app-text/xpdf example from Step 2:

$ cd app-text/xpdf
  $ source \~/.bash\_profile # Ensure that ECHANGELOG\_USER is set if you added it to your .bash\_profile just now
  $ sunrise-commit -c "New Ebuild for bug #1313 thanks to Max, Tom, Bruce and Diane."

*NOTE: The -c switch creates the ChangeLog before committing. For detailed instructions on using the sunrise-commit tool, see SunriseCommitHowTo.*

In Step 1 you were instructed not to add the non-reviewed Sunrise overlay to your PORTDIR\_OVERLAY, so expect to see the following message when committing:

\* Digesting ebuilds ...
  Appending /var/sunrise to PORTDIR\_OVERLAY...

This is a temporary modification needed by the *ebuild \*.ebuild manifest* command called by sunrise-commit. Once the commit is finished your PORTDIR\_OVERLAY will be reverted to its original value.

### Expected repoman warnings

Warnings and errors in repoman's output indicate issues that you should resolve before committing your work to the repository. The only repoman warning you can safely ignore and, in fact, you are required to have this warning for Sunrise is the following (using dev-tcltk/tkdnd as an example):

ebuild.allmasked               1
   dev-tcltk/tkdnd

If you have problems understanding sunrise-commit's output, please ask for explanation in [#gentoo-sunrise](irc://chat.freenode.net/%23gentoo-sunrise).

### Update the Bug Tracker

Make sure you are either the reporter of the bug or have your name in the CC list. This is crucial so that you can receive an email when any changes or comments are made on the bug. It is good to act responsible for the ebuild you have committed to Sunrise.

You should mention the Sunrise Overlay with a URL to the reviewed repo in a comment on the bug.

This is now in the sunrise overlay. You can find it at:
 [http://git.gentoo.org/gitweb/?p=proj/sunrise-reviewed.git;a=tree;f=app-misc/my-ebuild](http://git.gentoo.org/gitweb/?p=proj/sunrise-reviewed.git;a=tree;f=app-misc/my-ebuild)


You should also update the status of the bug (assuming there is an attached ebuild):

Status Whiteboard: \[sunrise-overlay\]
  Keywords: EBUILD, InOverlay

*NOTE: This will only work, if you are the reporter of the bug.* If you are unable to change these bug fields, simply ask in [\[1\]](irc://chat.freenode.net/%23gentoo-sunrise#gentoo-sunrise) and a dev or other user with bugedit priviliges will do this for you.

### Future Updates

When making further changes or updates to the package in the overlay, creating new attachments to the Bugzilla entry isn't necessary. Instead, post notifications of the changes you make and provide links to where they're located in the overlay. This is important for any changes which fix bugs, change functionality, or make the ebuild more usable. You do not need to update the bug for every little whitespace change you make—please use good common sense here.

Before you make any changes to the the repo, remember to first pull new changesets into it to avoid conflicts with changesets committed by other users:

$ cd \~/sunrise # cd into your local checkout
  $ git pull --rebase

For version bumps, please use

$ git mv foo-a.b.c.ebuild foo-a.b.d.ebuild

and edit the new version afterwards if needed. If making an additional new version and not removing the old one, please use

$ cp foo-a.b.c.ebuild foo-d.e.f.ebuild

and edit the new version as necessary.

Note that version bumps should replace older versions unless there is a good reason to keep particular old versions around. If there is a good reason for the old version to stay in sunrise, state it in the ChangeLog/commit message, otherwise we will assume you forgot to remove the old version.

If you use some other way to add a new version, remove the old version before commiting with

git rm foo-a.b.c.ebuild

After you have done this, you can commit the new version with

sunrise-commit -c "Version bump for foo, thanks to Lee."

**Important: Every new ebuild and every major change to an existing ebuild needs a review in [#gentoo-sunrise](irc://chat.freenode.net/%23gentoo-sunrise) and a developer ok before commit.** (A mere version bump wherein the contents of the ebuild do not change (i.e., you are only using 'git mv' followed by 'sunrise-commit' is generally exempt from review.)

## Common Errors

There are some common errors that you may encounter on occasion when committing. These are addressed in HowToFixCommonErrors.

## Access

Now that you read through the whole page, you deserve your access. Just ask any Sunrise Administrator to get one and provide, after you have been asked for it, the following string after replacing the text in square brackets with the appropriate commands:

\[the command (from this HowToCommit, modified for your actual ebuild) you used to upload your ebuild to a pastebin\]-\[the command (from this HowToCommit, modified for your actual ebuild) which you will use to commit your ebuild to the sunrise overlay\]

This serves as evidence that you have read this guide carefully.
