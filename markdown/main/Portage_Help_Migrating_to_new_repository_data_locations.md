<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Migrating_to_new_repository_data_locations | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Migrating to new repository data locations -->
---
title: Portage/Help/Migrating to new repository data locations
url: https://wiki.gentoo.org/wiki/Portage/Help/Migrating_to_new_repository_data_locations
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-09"
fingerprint: "3e0d305c9616ff8b"
license: CC BY-SA 4.0
---

# Portage/Help/Migrating to new repository data locations

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## About

This page gives a set of instructions for how to migrate from the old repository/data locations to the new locations used in e.g. stage3s going forward.

See [bug #662982](https://bugs.gentoo.org/show_bug.cgi?id=662982) for background and motivation.

## Old/new locations

| Gentoo paths |  |  | 
|---|---|---|
| Purpose | Old path | New path | 
|---|---|---|
| Storing the tree / ebuilds | /usr/portage | /var/db/repos/gentoo | 
| Distfiles (downloaded source code/files) | /usr/portage/distfiles | /var/cache/distfiles | 
| Binary packages | /usr/portage/packages | /var/cache/binpkgs | 

## Instructions

1\. Remove (or comment out) any `PORTDIR`, `DISTDIR`, `PKGDIR` entries in /etc/portage/make.conf

2\. Note the output of eselect profile show for later

3\. If /var/db/repos/ does not exist, create it:

`root #``ls -1 /var/db/repos`
4\. Move /usr/portage/distfiles to /var/cache/distfiles

`root #``mv /usr/portage/distfiles/* /var/cache/distfiles`
5\. Move /usr/portage/packages to /var/cache/binpkgs

`root #``mv /usr/portage/packages/* /var/cache/binpkgs`
6\. Move /usr/portage to /var/db/repos/gentoo

`root #``mv /usr/portage /var/db/repos/gentoo`
6\. Edit the 'location' variable in /etc/portage/repos.conf/gentoo.conf (or equivalent) to /var/db/repos/gentoo

`root #````
sed -i -e 's:/usr/portage/packages:/var/cache/binpkgs:' /etc/portage/*
```
`root #````
sed -i -e 's:/usr/portage/distfiles:/var/cache/distfiles:' /etc/portage/*
```
`root #````
sed -i -e 's:/usr/portage:/var/db/repos/gentoo:' /etc/portage/*
```
7\. Use eselect profile set \_\_\_\_ with the profile noted earlier

8\. Reinstall portage: DISTDIR=/var/cache/distfiles PKGDIR=/var/cache/binpkgs emerge --oneshot portage

`root #``DISTDIR=/var/cache/distfiles PKGDIR=/var/cache/binpkgs emerge --oneshot portage`
### Additional steps

9\. If you run a local rsync mirror, edit the path entry in /etc/rsyncd.conf to point to /var/db/repos/gentoo

10\. If you share your distfiles, remember to update whatever symlink or config file you need for that
