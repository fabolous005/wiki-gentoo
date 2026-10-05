<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Conflicting_specifications_during_rlpkg | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Conflicting_specifications_during_rlpkg -->
---
title: Knowledge Base:Conflicting specifications during rlpkg
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Conflicting_specifications_during_rlpkg
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "1663ab4206a38689"
license: CC BY-SA 4.0
---

# Knowledge Base:Conflicting specifications during rlpkg

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When trying to relabel a package (using the rlpkg tool) a message similar to the following is displayed:

`root #``rlpkg -a -r`
filespec\_add: conflicting specifications for /usr/bin/getconf and 
/usr/lib64/misc/glibc/getconf/XBS5\_LP64\_OFF64, using
system\_u:object\_r:lib\_t

## Environment

This article is applicable to Gentoo Linux systems with a *selinux* [profile](https://wiki.gentoo.org/wiki/Portage/Profiles):

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

A SElinux profile ends with `/selinux`.

## Analysis

This is most likely caused by hard linked files. SELinux uses the extended attributes in the file system to store the security context of a file. If two separate paths point to the same file using hard links (i.e. the files share the same inode) then both files will have the same security context. rlpkg (and related applications) obtain the security context from a path value. As such, they may get two different results (different paths) for the same file (hardlinked files).

## Resolution

The solution depends on the particular case; in order of most likely to happen and resolve:

1. Although both files are the same, they are not used in the same context. In such cases, it is recommended to remove one of the files and then copy the other file back to the first. For example: `root #``rm file_B; cp file_A file_B` This way, both files have different inodes and can be labelled accordingly.
2. Both files are used for the same purpose; in this case, it might be better to label the file which would not be labelled correctly (say a binary somewhere in a /usr/lib64 location) using the semanage tool:

`root #``semanage fcontext -a -t correct_domain_t /usr/lib64/path/to/file`
It is also not a bad idea to report (after verifying if it has not been reported by someone else) this on [Gentoo's Bugzilla](https://bugs.gentoo.org) so that the default policies are updated accordingly.
