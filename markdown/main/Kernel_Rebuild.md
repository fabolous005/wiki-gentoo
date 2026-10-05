<!-- source: https://wiki.gentoo.org/wiki/Kernel/Rebuild | group: Gentoo Wiki (Main) | wiki-title: Kernel/Rebuild -->
---
title: Kernel/Rebuild
url: https://wiki.gentoo.org/wiki/Kernel/Rebuild
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-16"
fingerprint: "1793531a90b29f87"
license: CC BY-SA 4.0
---

# Kernel/Rebuild

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Exit the kernel configuration and rebuild the kernel using the [following command](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Compiling_and_installing):

`root #``make && make modules_install`


Do not forget to copy the newly compiled kernel image to the /boot location. If applicable mount /boot.

`root #``mount /boot``root #``make install`
When not changing kernel versions, it may not be necessary to update the system's boot loader. This depends on the system configuration; if the boot loader is pointed to a replaced binary file with the exact same name, secondary boot loader entries may not need updated. When in doubt re-run the boot loader configuration generator or inspect the boot loader's configuration file to avoid issues the next time the system reboots.

Update the boot loader configuration prior to rebooting the system. For instance, when using [GRUB](https://wiki.gentoo.org/wiki/GRUB#Main_configuration_file), these steps can be done by running the following command:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Users of [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub) follow the procedure in the [Installation](https://wiki.gentoo.org/wiki/EFI_stub#Installation) section.

If using systemd's EFI bootloader, review the [systemd-boot](https://wiki.gentoo.org/wiki/Systemd/systemd-boot) article.

Reboot for the new kernel configuration to take effect:

`root #``reboot`
