<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Port_Fedora_UEFI_Support | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Port Fedora UEFI Support -->
---
title: Google Summer of Code/2012/Ideas/Port Fedora UEFI Support
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Port_Fedora_UEFI_Support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "302c853a0ab9b148"
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Port Fedora UEFI Support

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Port Fedora UEFI Support]

Computer manufacturers are adopting UEFI as a BIOS replacement on amd64 systems, but Gentoo is currently unable to boot on such systems using GRUB 0.97. Intel wrote patches for UEFI support that were adopted by Fedora's GRUB fork. Porting those patches from [Fedora's GRUB fork](https://pkgs.fedoraproject.org/gitweb/?p=grub.git;a=summary) to sys-boot/grub is necessary if sys-boot/grub is to remain a viable bootloader in Gentoo.

There are two existing issues in sys-boot/grub that must be addressed in conjunction with this port. The first is that sys-boot/grub does not compile correctly with GCC 4.6, which is [bug #360513](https://bugs.gentoo.org/show_bug.cgi?id=360513). The second is that sys-boot/grub's grub-probe utility relies on /dev/root, which is newer versions of udev remove. A proper port must compile properly with GCC 4.6 without any dependence on /dev/root.

In addition, sys-boot/grub is GPLv2 licensed, so these improvements may not involve the use of any GPLv3 code.



| Contacts | Required Skills | 
|---|---|
|  |  |
