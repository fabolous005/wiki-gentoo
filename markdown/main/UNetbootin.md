<!-- source: https://wiki.gentoo.org/wiki/UNetbootin | group: Gentoo Wiki (Main) | wiki-title: UNetbootin -->
---
title: UNetbootin
url: https://wiki.gentoo.org/wiki/UNetbootin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-23"
fingerprint: "96fdea5f0773eda4"
license: CC BY-SA 4.0
---

# UNetbootin

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**UNetbootin** (**U**niversal **Netboot In**staller) is an easy to use [LiveUSB](https://wiki.gentoo.org/wiki/LiveUSB) flash key rescue disk generator with a GUI.

## Usage

There are two options to use ISOs in UNetbootin. The distribution option will use UNetbootin to automatically download a disk image for the distribution of choice. The diskimage option is used when the user already has a disk image that they would like to use.

### Normal Use

#### Using the Distribution Option

1. Select the Distribution option at the top.
2. Use the drop down menus to pick the distribution and the version.
3. Use the menu at the bottom to select the type of drive that will be used.
4. Select the drive to be used when flashing the disk image.
5. Click OK when ready.

#### Using the Diskimage Option

1. Select the Diskimage option on the bottom.
2. Select either ISO or Floppy depending on the type of bootable image being used.
3. Use the file browser to select the bootable image to be used.
4. If needed, select the amount of space needed to preserve files across reboots using the buttons provided.
5. Use the menu at the bottom to select the type of device that will be used.
6. Select drive to be used when flashing the disk image.
7. Click OK when ready.

### Windows 7 live USB

Making a Windows 7 bootable USB takes several steps that must be executed in a specific order.  This requires UNetbootin 494 binaries from SourceForge.  It is also a good idea to also have the regular [sys-boot/unetbootin](https://packages.gentoo.org/packages/sys-boot/unetbootin) package installed to satisfy all of the binary dependencies.

1. Install [sys-block/gparted](https://packages.gentoo.org/packages/sys-block/gparted) and [sys-fs/ntfs3g](https://packages.gentoo.org/packages/sys-fs/ntfs3g).
2. Using [GParted](https://wiki.gentoo.org/wiki/User:Maffblaster/Drafts/Gparted), format an 8GB or larger medium as [NTFS](https://wiki.gentoo.org/wiki/NTFS).
3. Mount the freshly formatted USB disk.
4. Get the unetbootin-linux-494 binary from [SourceForge](http://sourceforge.net/projects/unetbootin/files/UNetbootin/494/).
5. Run `user $``chmod a+x unetbootin-linux-494`
6. Run `root #``./unetbootin-linux-494`
7. Select diskimage, then locate the Windows ISO using the file browser.
8. Under Drive, select your USB drive's [NTFS](https://wiki.gentoo.org/wiki/NTFS) partition.
9. Select OK, and go have a cup of hot coco as this will take some time. Exit instead of rebooting. Then unmount and remove the flash drive.

## Troubleshooting

### UNetbootin Appearing Blank When Opened

Sometimes UNetbootin will appear as a blank window when opened. If this happens, it is a problem with the script that runs the executable. This can be fixed by running:

`root #``QT_X11_NO_MITSHM=1 /usr/bin/unetbootin.elf`
This can be run either as root or with sudo.
