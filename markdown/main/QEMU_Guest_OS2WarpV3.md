<!-- source: https://wiki.gentoo.org/wiki/QEMU/Guest/OS2WarpV3 | group: Gentoo Wiki (Main) | wiki-title: QEMU/Guest/OS2WarpV3 -->
---
title: QEMU/Guest/OS2WarpV3
url: https://wiki.gentoo.org/wiki/QEMU/Guest/OS2WarpV3
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-20"
fingerprint: d74c2e59e1ad298d
license: CC BY-SA 4.0
---

# QEMU/Guest/OS2WarpV3

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## QEMU with OS/2 Warp V3 as guest

### Basic install and audio

#### Preparation

OS/2 Warp V3 has no support for booting from CDROM, so the floppy images, disk0.dsk and disk1\_cd.dsk, must be copied from the mounted CD. Refer to cdinst.bat for their location.

Fix the permissions of the images, unmount the CD and create a copy of it, and create hda.img:

`root #````
chmod 644 disk*
```
`root #````
dd if=/dev/sr0 of=os2v3_cd.iso
```
`root #````
qemu-img create hda.img 1000M
```
#### Installation

Start [QEMU](https://wiki.gentoo.org/wiki/QEMU) with:

`root #````
qemu-system-i386 -fda disk0.dsk -hda hda.img -cdrom os2v3_cd.iso -boot ac
```
When prompted to insert another disk, switch to the console with `Ctrl`-`Alt`-`2` 
and type:

change floppy0 disk1\_cd.dsk

This environment behaves quite like a normal shell, e.g. tab completion is available, and history is available via `Up`.

Return to the emulation screen with `Ctrl`-`Alt`-`1` and type `Enter` to proceed.

Perform the install as described in the OS/2 installation manual.

When prompted to remove the diskette, again switch to the console and type:

boot\_set c

Change back to the emulation. Make sure it says `Press ctrl-alt to exit mouse grab`
in the window title and then do `Ctrl`-`Alt`-`Delete`.

If a trap is encountered while booting, type:

system\_reset

into the console window.

After the first boot from hard disk, in the system configuration:

- Choose `not listed ide cdrom` yo make sure that you don't lose your CDROM.
- Choose `Sound Blaster 16` in multimedia, and verify that its configuration is:

dma8:1, dma16:5, irg:5, BaseIO:220, MPU-401:330

If prompted to push the ok button to reboot and doing so does not work, just push cancel and do a normal shutdown (right click on the desktop). Go to the console, type `q` to quit QEMU, then restart it with the following command line:

qemu-system-i386 -fda disk0.dsk -hda hda.img -cdrom os2v3\_cd.iso -boot c -device sb16,iobase=0x220,irq=5,dma=1,dma16=5

This should enable WAV audio.

Create an account by supplying a name and a password.

To insert another CD, type:

change ide1-cd0 image.iso

in the console window.

### Install video driver

#### Software

- xrgw040
- diunpack.exe from fastkick141.zip
- csg144.exe
- gradd97.zip

For video resolutions other than 640x480x16, gradd97.zip can be used, but this needs at least fixpack 35 to be installed, such as  xrgw040 (`g` for Germany) or its US version, `xr_w040`. Installation requires a so-called "Corrective Service" program of at least version 1.43, e.g. csg144.exe (again, `g` for Germany). As loaddskf.exe cannot be used to put the disk images on a diskette, and because those the images also cannot be used by QEMU, diunpack.exe must be used, available in fastkick141.zip.

#### Installation of the fixpack

Put all the files in a folder, unzip the zipped files and make an ISO file with [mkisofs(8)](https://man.archlinux.org/man/mkisofs.8.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

Make this ISO available in QEMU, copy the xrgw040 folder to c: and put diunpack.exe in that folder. Then create a folder fixpack on c:, open an OS/2 Window, go to {c:\xrgw040 and type:

diunpack xrgw040.1dk -d c:\fixpack

Repeat this for all the other xrgw040.?dk files. Copy csg144.exe to another folder, extract it by double clicking it, and copy the content to c:\fixpack. Find the file response.wp3 and replace the line `:SOURCE A:\` with `:SOURCE c:\fixpack`. Close all programs except the OS/2 window. In that window, change directory to c:\fixpack and type:

fpinst warp3

#### Installation of the video driver

After rebooting, install the driver in the gradd97 folder:

set lang=de\_DE
setup gen

After another reboot, it should be possible to choose a different resolution, such as 1024x768x16,7Mio

## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
