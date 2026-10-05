<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Adjusting_GRUB_settings_for_a_single_boot_session | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Adjusting_GRUB_settings_for_a_single_boot_session -->
---
title: Knowledge Base:Adjusting GRUB settings for a single boot session
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Adjusting_GRUB_settings_for_a_single_boot_session
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2015-03-03"
fingerprint: d499a3241f36b3fd
license: CC BY-SA 4.0
---

# Knowledge Base:Adjusting GRUB settings for a single boot session

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When troubleshooting boot failures or trying to make changes for kernel settings, users might want to boot their system with a particular change in the GRUB bootloader configuration *for a single boot session* (i.e. not persistent).

## Environment

Gentoo Linux systems using GRUB as the bootloader.

## Analysis

With GRUB, it is possible to boot with different settings for just a single boot session. Changes made herein are not persisted, so rebooting afterwards will boot back using the "old" configuration settings.

## Resolution

1. During bootup, wait until the GRUB boot menu is displayed. The default timeout for GRUB is 30 seconds (if necessary press any key other than `Enter` or `→` to halt the timeout);
2. Navigate to the entry desired boot from, but do not press the `Enter` key. Instead, press the `E` key (which stands for *edit*);
3. Navigate to the line needed to be modified (which is most likely the one starting with `kernel`, but can also be the `root` or `initrd` line) and press `E` again to edit this specific line. When finished changing the line, press the `Enter` key;
4. When all required changes have been made to the line(s), press the `B` key to *boot* the entry.

If happy with the result, do not forget to update /boot/grub/grub.conf with changes made in the steps above for persistent result.
