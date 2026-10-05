<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:What_is_the_lost%2Bfound_directory%3F | group: Gentoo Knowledge | wiki-title: Knowledge_Base:What_is_the_lost%2Bfound_directory%3F -->
---
title: Knowledge Base:What is the lost+found directory?
url: https://wiki.gentoo.org/wiki/Knowledge_Base:What_is_the_lost%2Bfound_directory%3F
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-21"
fingerprint: "3b8fba1cb3a7072c"
license: CC BY-SA 4.0
---

# Knowledge Base:What is the lost+found directory?

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

After creating a file system, a directory called "lost+found" is created.

## Environment

Any Gentoo Linux system with an ext2, ext3, or ext4, or f2fs<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> file system.

## Analysis

The lost+found directory is used by file system check tools (fsck). When a system crashes and there is some inconsistency, fsck might be able to (partially) recover lost information (files or directories). It might not know where these files should reside, so it places them in the lost+found directory so that the administrator can move them back to their original location(s).

The lost+found directory is not vital. If the administrator decides to delete it, on the next run of fsck will be recreated.

## Resolution

Using ext or f2fs filesystem the lost+found directory is to be expected. Move away from ext or f2fs-based filesystems to avoid automatic creation of this directory.
