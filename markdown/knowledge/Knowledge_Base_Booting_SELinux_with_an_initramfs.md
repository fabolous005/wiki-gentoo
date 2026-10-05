<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Booting_SELinux_with_an_initramfs | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Booting_SELinux_with_an_initramfs -->
---
title: Knowledge Base:Booting SELinux with an initramfs
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Booting_SELinux_with_an_initramfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2012-05-13"
fingerprint: e32d822e06168f85
license: CC BY-SA 4.0
---

# Knowledge Base:Booting SELinux with an initramfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

Trying to boot in SELinux enforcing mode with an initramfs fails.

\* Mounting /dev
mknod: '/dev/console': Permission denied
mknod: '/dev/tty1': Permission denied
mknod: '/dev/tty': Permission denied
mknod: '/dev/null': Permission denied
mknod: '/dev/kmsg': Permission denied
 \* Starting udev
 \* Failed to start udev
 \* ERROR: udev failed to start

## Environment

SELinux-enabled systems, booting in enforcing mode, whereas the system is prepared using an initramfs first.

## Analysis

The initramfs tries to prepare the system, but by doing so creates files that are wrongly labeled. When the initramfs handles over control of the system to the **init** application, this application loads in the SELinux policy, enables SELinux enforcing mode and continues. As the files (and directories) are wrongly labeled, the applications do not have the proper privileges to continue and the system freezes.

## Resolution

Right now, we are in the process of figuring out which privileges are needed and if we can update the initramfs to load the policy earlier (so that newly created files or directories are immediately labeled correctly). If you do need to boot in enforcing mode, we recommend the following steps:

1. Boot the system in permissive mode. For production systems, this might mean that you need to enable SELinux development mode
2. During the boot-up sequence, switch on enforcing mode. You can do this through the */etc/local.d* files if you want, or in an init script called earlier. `root #``setenforce 1`
3. After switching enforcing mode, toggle the **secure\_mode\_policyload** SELinux boolean. This will prohibit switching back to permissive mode as long as the system is operational (in other words, until you reboot again) `root #``setsebool secure_mode_policyload on`
