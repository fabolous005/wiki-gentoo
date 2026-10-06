<!-- source: https://wiki.gentoo.org/wiki//boot | group: Gentoo Wiki (Main) | wiki-title: /boot -->
---
title: "/boot"
url: https://wiki.gentoo.org/wiki//boot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-28"
fingerprint: "579b1afd7e2e7f67"
license: CC BY-SA 4.0
---

# /boot

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

/boot is the directory that holds kernels and other files required for booting.

It should contain the kernel, kernel files, and the initramfs (if present). Optionally, it may also contain microcode. If the ESP is not /efi, it may be at /boot in which case it will contain the bootloader. The ESP is also often placed at /boot/efi, however. If GRUB is used, its configuration will be in /boot/grub, regardless of where the ESP is located.
