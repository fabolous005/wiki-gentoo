<!-- source: https://wiki.gentoo.org/wiki/Fglrx | group: Gentoo Wiki (Main) | wiki-title: Fglrx -->
---
title: fglrx
url: https://wiki.gentoo.org/wiki/Fglrx
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-25"
fingerprint: "960fda1fd9a6398d"
license: CC BY-SA 4.0
---

# fglrx

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Deprecated article**

This article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

*Not to be confused with[Catalyst](https://wiki.gentoo.org/wiki/Catalyst).*

**Resources**

**AMD Catalyst** (previous **fglrx**: **F**ire**GL** and **R**adeon for **X** ) is the proprietary graphics driver for *older* AMD/ATI graphic cards. The open source alternative is [radeon](https://wiki.gentoo.org/wiki/Radeon).

If a user needs to use this graphics card then the best way to get a working display will be to follow the [Xorg/Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) and set the `VIDEO_CARDS="radeon"` for [x11-base/xorg-drivers](https://packages.gentoo.org/packages/x11-base/xorg-drivers).

## Hardware detection

To choose the right driver, first detect the graphics card. You can use [lspci](https://wiki.gentoo.org/wiki/Hardware_detection) for this task:

`root #``lspci | grep -i VGA`
If you have an AGP card, also detect the chipset supporting AGP:

`root #``lspci | grep -i AGP`
## Hardware support

Development of the Catalyst driver package, including fglrx, has stopped. AMD continues to support the development of the open source [radeon](https://wiki.gentoo.org/wiki/Radeon) driver module and the newer [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) driver, with [AMDGPU-PRO](https://wiki.gentoo.org/wiki/AMDGPU-PRO) as the proprietory closed source extension.

Consequently fglrx will not be updated for use with newer versions of X.org Xserver, as well as Wayland support won't be added.

| Product name | Driver version | X server (max) | Bus | Note | 
|---|---|---|---|---|
| Radeon based on GCN | [amdgpu](http://packages.gentoo.org/package/x11-drivers/amdgpu) | latest |  | Also includes [Wayland](https://wiki.gentoo.org/wiki/Wayland) support. fglrx is deprecated upstream.  GCN based Radeons are supported by [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU), and older cards by [radeon](https://wiki.gentoo.org/wiki/Radeon) | 
| Radeon HD 5000 and newer | [15.12](http://packages.gentoo.org/package/x11-drivers/ati-drivers) | 1.17 | PCIe |  | 
| Radeon HD 5000 and newer | [14.12-r4](http://packages.gentoo.org/package/x11-drivers/ati-drivers) | 1.16 | PCIe |  | 
| Radeon HD 5000 and newer | [13.12](http://packages.gentoo.org/package/x11-drivers/ati-drivers) | 1.14.49 | PCIe |  | 
| Radeon HD 5000 and newer | [13.4](http://packages.gentoo.org/package/x11-drivers/ati-drivers) | 1.13 | PCIe | Broken link, no longer in portage.  Use [radeon](https://wiki.gentoo.org/wiki/Radeon) driver instead. | 
| Radeon HD 2000 - 4000 | [13.1\_pre897](http://packages.gentoo.org/package/x11-drivers/ati-drivers) | 1.12.49 | PCIe or AGP | Broken link. Mask ati-drivers slot 1 and newer, see the instructions under the table. | 
| Radeon HD 1000 and older |  |  |  | Use [radeon](https://wiki.gentoo.org/wiki/Radeon) driver | 

- Force legacy driver when you have Radeon HD 2000 - 4000
- [Mask](https://wiki.gentoo.org/wiki/Knowledge_Base:Masking_a_package) >=ati-drivers-13.1 and >=xorg-server-1.13:
- `root #``echo "# Force legacy driver" >> /etc/portage/package.mask``root #``echo ">=x11-base/xorg-server-1.13" >> /etc/portage/package.mask``root #``echo "x11-drivers/ati-drivers:1" >> /etc/portage/package.mask`

## Installation

### Kernel

You need [USB](https://wiki.gentoo.org/wiki/USB) support. Kernel version 3 is needed for legacy ATI drivers (i.e. vanilla-sources 3.16.57).

Also you need to activate the following kernel options (using genkernel --menuconfig all in the /usr/src/linux):

```
[*] Enable loadable module support --->
Processor type and features  --->
    [*] MTRR (Memory Type Range Register) support
Bus options (PCI etc.)  --->
    [*] PCI Express support
    [*] Message Signaled Interrupts (MSI and MSI-X)
Device Drivers  --->
    Graphics support  --->
        < > Direct Rendering Manager (XFree86 4.1.0 and higher DRI support) --->
    Graphics support  --->
        [ ] Support for frame buffer devices -- not necessary? If you set your kernel this way, you will see no console on bootup, so if running headless, you have nothing.
```
If you have an AGP card, enable AGP support. If you want to use the ATI internal AGP support, you must enable kernel support as a module or not at all:

```
Device Drivers  --->
    Graphics support  --->
        <*> /dev/agpgart (AGP Support)  --->
            Choose your AGP driver, e.g.:
            <*> AMD Opteron/Athlon64 on-CPU GART support
```
If you use a hybrid system with Intel integrated video card, you should also activate KMS and Intel driver. Make sure radeon is disabled.

```
Device Drivers  --->
    Graphics support  --->
        <*> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support) --->
            < > ATI Radeon
            <*> Intel 8xx/9xx/G3x/G4x/HD Graphics
                [*]   Enable modesetting on intel by default
```
### Driver

**`/etc/portage/make.conf`**

**Set`VIDEO_CARDS` to fglrx**

```
VIDEO_CARDS="... fglrx ..."
```
After setting or altering `VIDEO_CARDS` values remember to update the system using the following command so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
If you are using a hybrid system, enable intel driver but disable **sna** USE flag on it.  See 
[bug #430000](https://bugs.gentoo.org/show_bug.cgi?id=430000).

**`/etc/portage/make.conf`**

```
VIDEO_CARDS="fglrx intel"
```
**`/etc/portage/package.use`**

```
x11-drivers/xf86-video-intel uxa -sna
```


After setting this you want to update your system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
### acpid

Some cards need acpid running to handle events. See the [ACPI](https://wiki.gentoo.org/wiki/ACPI) article.

## Configuration

### Initial setup

This will generate an initial [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf) to /root/xorg.conf.new:

`root #``X -configure`
Copy the file /root/xorg.conf.new to the default location:

`root #``cp xorg.conf.new /etc/X11/xorg.conf`
This will modify an **existing** [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf) to use the fglrx driver with a single screen:

`root #``aticonfig --initial --input=/etc/X11/xorg.conf`
For dual-head configuration use this instead (where the second screen is \[left|right|above|below\]):

`root #``aticonfig --initial=dual-head --input=/etc/X11/xorg.conf --screen-layout=[left|right|above|below]`
Set the OpenGL driver to use fglrx:

`root #``eselect opengl set ati`
### Permissions

If you have the USE flag acl enabled globally and are using elogind or systemd (i.e you're using a Desktop profile) permissions to video cards will be handled automatically. You can check the permissions using getfacl:

`user $````
getfacl /dev/ati/card0 | grep larry
```
user:larry:rw-

A broader solution is to add the user you want to be able to access the video card to the video group:

`root #``gpasswd -a larry video`
Note that you will still be able to run X without permission to the fglrx subsystem, but usually not with acceleration enabled.

### Settings

The most comfortable way for most users is to use opt/bin/amdcccle as a graphical UI to configure the driver.

## Troubleshooting

### Unexplained segmentation faults and kernel crashes

If you experience unexplained segmentation faults and kernel crashes with this driver and multi-threaded applications such as [Wine](https://wiki.gentoo.org/wiki/Wine) set *UseFastTLS* in [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf) to either *0* or *1*, but not *2*.

### X -configure fails with a no device found error

If X -configure fails, you must create a stub xorg.conf file:

**`/etc/X11/xorg.conf`**

```
Section "Device"
        Identifier "ATI radeon xxxx"
        Driver     "fglrx"
EndSection
```
Where "xxxx" is your card model (example: 7770 for HD7770). Name this file xorg.conf and place it in /etc/X11.

### aticonfig fails with no suitable screens error

Create stub file as above, run aticonfig command again as:

1. aticonfig --initial -f --input=/etc/X11/xorg.conf

### Trouble with integrated graphics (A8 or similar)

If you are having trouble getting an on chip integrated graphic core to work (strange visuals/black screen) you may want to double check your BIOS settings.

In some cases it appears the 'auto' settings don't work correctly, so make sure to explicitly enable the integrated graphics.

In my case (MSI A88XM-E45 motherboard):

Settings > Advanced > Integrated Graphics Configuration > Initiate Graphics Devices - I changed this from 'auto' to 'Dual graphics'

Settings > Advanced > Integrated Graphics Configuration > Initiate Graphics Shared Memory - This then appeared which I gave a setting

If you don't find the options above, try to use the "Sideport" only memory option instead of UDMA or Sideport and UDMA.

After this all problems cleared up.

## See also

- [fglrx Quick Switch](https://wiki.gentoo.org/wiki/Fglrx_Quick_Switch) - Quickly switch between this driver and the open [Radeon](https://wiki.gentoo.org/wiki/Radeon) driver using GRUB 2 without downgrading xorg-server.
- [Hprofile](https://wiki.gentoo.org/wiki/Hprofile) — an application that can be used to manage multiple profiles be it hardware or software.
- [ATI FAQ](https://wiki.gentoo.org/wiki/ATI_FAQ) — Frequently Asked Questions (FAQ) to help users avoid some common installation and configuration issues related to DRI and X11 for AMD/ATI boards.
