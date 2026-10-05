<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Software_installation_fails_with_open_wr_ACCESS_DENIED_on_SELinux_systems | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Software_installation_fails_with_open_wr_ACCESS_DENIED_on_SELinux_systems -->
---
title: Knowledge Base:Software installation fails with open wr ACCESS DENIED on SELinux systems
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Software_installation_fails_with_open_wr_ACCESS_DENIED_on_SELinux_systems
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-19"
fingerprint: f72c153e8cf3c9c8
license: CC BY-SA 4.0
---

# Knowledge Base:Software installation fails with open wr ACCESS DENIED on SELinux systems

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

After upgrade of the SELinux userspace utilities (like libselinux and libsemanage), the installation process of any software fails with the following error message:

`root #``emerge -u selinux-base-policy`
\>>> Install selinux-base-policy-2.20120215-r7 into /var/tmp/portage/sec-policy/selinux-base-policy-2.20120215-r7/image/ category sec-policy
 \* Installing targeted application policy package
ACCESS DENIED  open\_wr:      /sys/fs/selinux/context
/etc/selinux/targeted/contexts/files/file\_contexts:  invalid context system\_u:object\_r:portage\_tmp\_t
 \* Installing targeted authlogin policy package
ACCESS DENIED  open\_wr:      /sys/fs/selinux/context
/etc/selinux/targeted/contexts/files/file\_contexts:  invalid context system\_u:object\_r:portage\_tmp\_t
...

## Environment

This error occurs on a SELinux-enabled Gentoo system, after updating the SELinux libselinux package towards version 2.1.9 or higher and with a Portage version prior to 2.1.10.56 or 2.2.0\_alpha100:

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

`root #``emerge -pv libselinux`
\[ebuild  R  \] sys-libs/libselinux-2.1.9

`root #``emerge -pv portage`
\[ebuild  R  \] sys-apps/portage-2.1.10.49

## Analysis

Since libselinux version 2.1.9 or higher, the standard location for the SELinux file system (selinuxfs) has moved from /selinux to /sys/fs/selinux. As a result, the sandbox integration in Portage, which allows read/write access to /selinux/context is now invalid (as it should point to /sys/fs/selinux/context). This has been fixed in recent versions of Portage, but these have not been stabilized when libselinux-2.1.9 was.

## Resolution

Edit /etc/sandbox.conf to add /sys/fs/selinux/context to the `SANDBOX_WRITE` variable.

`root #``vim /etc/sandbox.conf`
SANDBOX\_WRITE="/sys/fs/selinux/context"

## External resources

- [Emerge fails with ACCESS DENIED on /sys/fs/selinux/context](https://bugs.gentoo.org/show_bug.cgi?id=410687) (Gentoo's Bugzilla).
