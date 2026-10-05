<!-- source: https://wiki.gentoo.org/wiki/System_recovery | group: Gentoo Wiki (Main) | wiki-title: System recovery -->
---
title: System recovery
url: https://wiki.gentoo.org/wiki/System_recovery
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-31"
fingerprint: "120a1e51d45589ae"
license: CC BY-SA 4.0
---

# System recovery

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Linux is very easy to break if you know what you're doing (or if you don't). This article will focus on repairing common issues or defects that may occur due to operator, system, or other error.

## Creating a recovery environment

To start, please get a live environment ISO image that can be used to perform repairs with, often a USB stick or rewritable CD/DVD. This first chapter will cover a basic invocation of dd to flash or burn the image to the medium.

This tool is almost always found in any Linux environment as most use GNU coreutils, the common utilities run from a shell like cp, rm, and mv. dd is one of these. Therefore, it's the easiest way to quickly create a recovery environment.

First, check the block device list:

`user $``lsblk`
Please locate the target device that the image should be flashed to. Then, make sure to remember or note down the device node path, e.g. /dev/sda.

Then, change directory (cd) to where the image is located. Now, run the following command and be **very careful to insert the correct device node**.

`root #``dd if=the_ISO_image.iso of=/dev/target_device bs=1M status=progress`
## See also

- [Dd](https://wiki.gentoo.org/wiki/Dd) — a utility used to copy raw data from a source into sink, where source and sink can be a block device, file, or piped input/output.
- [Fix my Gentoo](https://wiki.gentoo.org/wiki/Fix_my_Gentoo) — rescuing an installation when a chroot is not possible
