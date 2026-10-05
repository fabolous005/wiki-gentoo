<!-- source: https://wiki.gentoo.org/wiki/Portage_with_Git | group: Gentoo Wiki (Main) | wiki-title: Portage with Git -->
---
title: Portage with Git
url: https://wiki.gentoo.org/wiki/Portage_with_Git
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-17"
fingerprint: "1a0db78c0eabf9ec"
license: CC BY-SA 4.0
---

# Portage with Git

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article will explain how to use [Git](https://wiki.gentoo.org/wiki/Git) to synchronize the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

## Prerequisites

[app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) both need to be installed. By default [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) is installed with the [git](https://packages.gentoo.org/useflags/git)[. USE flag.](https://wiki.gentoo.org/wiki/USE_flag)

`root #``emerge --ask app-eselect/eselect-repository`
To reduce the risk of issues, it's recommended to sync the Gentoo repository before starting this process, and ensure that the system is using the latest [profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>).

## Setup

Disable and remove the old Gentoo ebuild repository:

`root #``eselect repository remove -f gentoo`
Removing /var/db/repos/gentoo ...
Updating repos.conf ...
1 repositories removed

If the obsolete variable [PORTDIR](https://wiki.gentoo.org/wiki/PORTDIR) is defined in [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) (which may be the case on older Gentoo installations), remove that definition as well.

Configure a new repository:

Adding gentoo to /etc/portage/repos.conf/eselect-repo.conf ...
Repository gentoo added


Sync the repository with the new Git configuration:

`root #``emaint sync -r gentoo`
Run again to test.

## Troubleshooting

### emerge --sync failed

If you run emerge --sync or eix-sync after switching to the git repo and it fails, this could be because the old rsync repo is still populated with rsync data. This can be confirmed with this sync error:

`root #``emerge --sync`
Syncing repository 'gentoo' into '/var/db/repos/gentoo'...
/usr/bin/git clone --depth 1 https://github.com/gentoo-mirror/gentoo .
fatal: destination path '.' already exists and is not an empty directory.
!!! git clone error in /var/db/repos/gentoo
fatal: not a git repository (or any of the parent directories): .git

To solve this issue, back up the directory /var/db/repos/gentoo using the following command:

`root #``mv /var/db/repos/gentoo /var/db/repos/gentoo.rsync-backup`
After the backup has been finished re-sync the gentoo repository using *one* of the following commands:

- `root #``emaint sync -r gentoo`
- `root #``emerge --sync`
- `root #``eix-sync`

## Local Mirrors

As a matter of courtesy, it is considered best practice that if you have multiple Gentoo hosts, you should set up a local portage mirror to reduce the number of external syncs that you have to perform. Syncing all of your internal hosts to an external mirror wastes bandwidth at both ends, and increases load on the public mirrors.

Please refer to [Local\_Mirror](https://wiki.gentoo.org/wiki/Local_Mirror) for instructions for setting up a local Portage mirror.
