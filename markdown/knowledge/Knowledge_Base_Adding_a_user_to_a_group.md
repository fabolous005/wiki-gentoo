<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Adding_a_user_to_a_group | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Adding_a_user_to_a_group -->
---
title: Knowledge Base:Adding a user to a group
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Adding_a_user_to_a_group
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-05-24"
fingerprint: fa099fa28ba18914
license: CC BY-SA 4.0
---

# Knowledge Base:Adding a user to a group

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

This article describes how to add a Linux user (account) to a group.

## Environment

Any Gentoo Linux environment not using a centralized account management tool (like LDAP).

`root #``grep ^group /etc/nsswitch.conf`
group:   compat

## Analysis

Unless defined otherwise, groups and group membership are managed through the /etc/group file. Some systems have accounts managed through a centralized resource (such as an LDAP directory). The /etc/nsswitch.conf file defines where the system looks for group membership.

When using a centralized user repository, the resolution given below might not work.

## Resolution

Although you can directly edit /etc/group, it is much easier (and less error prone) to use the `gpasswd` command.

For instance, to add the user "[larry](https://wiki.gentoo.org/wiki/Larry_the_cow)" to the `wheel` group:

`root #``gpasswd -a larry wheel`
