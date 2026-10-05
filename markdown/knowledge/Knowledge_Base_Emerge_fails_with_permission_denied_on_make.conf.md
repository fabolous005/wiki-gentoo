<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_fails_with_permission_denied_on_make.conf | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Emerge_fails_with_permission_denied_on_make.conf -->
---
title: Knowledge Base:Emerge fails with permission denied on make.conf
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_fails_with_permission_denied_on_make.conf
hostname: gentoo.org
sitename: Knowledge Base:Emerge fails with permission denied on make.conf
date: "2026-02-04"
fingerprint: "3e1405521a8a41ad"
license: CC BY-SA 4.0
---

# Knowledge Base:Emerge fails with permission denied on make.conf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

Any activity using emerge, such as emerge --info, fails with the following error:

`root #``emerge --info`
Permission denied: '/etc/portage/make.conf'

## Environment

This article is applicable to Gentoo Linux systems using a *selinux* [profile](https://wiki.gentoo.org/wiki/Portage/Profiles):

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

SELinux profiles always end with `/selinux`.

## Analysis

This is to be expected when not using the sysadm\_r role. Any Portage related activity requires that the sysadm\_r role be used; this is because other roles have no access to the /etc/portage/make.conf file (which should be labeled `system_u:object_r:portage_conf_t`).

## Resolution

Verify that the current context is within the `sysadm_r` role (second part of the context):

`user $``id -Z`
staff\_u:sysadm\_r:sysadm\_t

If this is not the case, use `newrole` to switch roles after re-authenticating with a personal password:

`user $``newrole -r sysadm_r`
Password:
