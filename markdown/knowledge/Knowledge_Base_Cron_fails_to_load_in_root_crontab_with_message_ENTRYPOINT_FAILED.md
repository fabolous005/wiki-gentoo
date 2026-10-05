<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Cron_fails_to_load_in_root_crontab_with_message_ENTRYPOINT_FAILED | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Cron_fails_to_load_in_root_crontab_with_message_ENTRYPOINT_FAILED -->
---
title: Knowledge Base:Cron fails to load in root crontab with message ENTRYPOINT FAILED
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Cron_fails_to_load_in_root_crontab_with_message_ENTRYPOINT_FAILED
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: fc1f7d124a9b8388
license: CC BY-SA 4.0
---

# Knowledge Base:Cron fails to load in root crontab with message ENTRYPOINT FAILED

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

Inside the cron.log file the following error message can be found:

**`cron.log`**

**Error message regarding ENTRYPOINT on root crontab**

Also notice that the root users' crontab is not used.

## Environment

This article is applicable on Gentoo Linux systems with a *selinux* [profile](https://wiki.gentoo.org/wiki/Portage/Profiles):

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

The user also has `ubac` set in the system's USE flags:

`root #``emerge --info | grep ^USE | grep ubac`
USE="... ubac ..."

Finally, the users' installed cron system is [sys-process/vixie-cron](https://packages.gentoo.org/packages/sys-process/vixie-cron):

`root #``qlist -I vixie-cron`
sys-process/vixie-cron

## Analysis

When [sys-process/vixie-cron](https://packages.gentoo.org/packages/sys-process/vixie-cron) wants to execute a users' crontab (including the root user), it first checks the SELinux user owner of the crontab file to make sure that it is safe to execute. But if some user other than root created the root crontab as a regular user (with su or sudo) then the ownership of the file will, SELinux-wise, still be that of the user that created it (most likely `staff_u`). Hence, cron (well, actually SELinux) refuses to load in the file.

With `USE="ubac"` set, access from specific domains towards resources (like files) are only allowed if the source context owner is the same as the target, or when `sysadm_t` is involved (this domain is *exempt* from the User Based Access Controls).

## Resolution

Verify that the root user file is indeed not owned by the root SELinux user:

`root #``ls -Z /var/spool/cron/crontabs/root`
staff\_u:object\_r:user\_cron\_spool\_t  /var/spool/cron/crontabs/root

Correct the SELinux owner of the file:

`root #``chcon -u root /var/spool/cron/crontabs/root`
Another solution would be to disable user-based access control by setting `USE="-ubac"`, but this reduces one of the access controls in place and as thus not recommended.

## See also

- [What is UBAC exactly?](https://wiki.gentoo.org/wiki/SELinux/FAQ#What_is_UBAC_exactly) - Gentoo Hardened SELinux FAQ.
