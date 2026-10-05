<!-- source: https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide | group: Gentoo Wiki (Main) | wiki-title: Xorg/Hardware 3D acceleration guide -->
---
title: Xorg/Hardware 3D acceleration guide
url: https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: d6031d5d01ae3de6
license: CC BY-SA 4.0
---

# Xorg/Hardware 3D acceleration guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This document is a guide to getting 3D acceleration working using the DRM with Xorg in Gentoo.

## Introduction

### What is hardware 3D acceleration and why do I want it?

With hardware 3D acceleration, three-dimensional rendering uses the graphics processor on the video card instead of taking up valuable CPU resources drawing 3D images. It's also referred to as "hardware acceleration" instead of "software acceleration" because without this 3D acceleration the CPU is forced to draw everything itself using the Mesa software rendering libraries, which takes up quite a bit of processing power.

While Xorg typically supports 2D hardware acceleration, it often lacks hardware 3D acceleration. Three-dimensional hardware acceleration is valuable in situations requiring rendering of 3D objects such as games, 3D CAD, and modeling.

### Getting 3D acceleration

In many cases, both binary and open-source drivers exist. Open source drivers are preferable since we're using Linux and open source is one of its underlying principles. Sometimes, binary drivers are the only option, especially if the graphics card is so new that open source drivers have not yet been written to support its features. Binary drivers include [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) for NVIDIA cards and [x11-drivers/xf86-video-ati](https://packages.gentoo.org/packages/x11-drivers/xf86-video-ati) (used to be [x11-drivers/ati-drivers](https://packages.gentoo.org/packages/x11-drivers/ati-drivers)) for older AMD/ATI cards, [dev-libs/amdgpu-pro-opencl](https://packages.gentoo.org/packages/dev-libs/amdgpu-pro-opencl) for newer AMD cards.

### What is DRI?

The [Direct Rendering Infrastructure](https://dri.freedesktop.org/wiki/), also known as the DRI, is a framework for allowing direct access to graphics hardware in a safe and efficient manner. It includes changes to the Xorg server, to several client libraries and to the kernel. The first major use for the DRI is to create fast [OpenGL](https://wiki.gentoo.org/wiki/OpenGL) implementations.

### What is the DRM and how does it relate to regular Xorg?

The DRM (Direct Rendering Manager) is an *enhancement* to Xorg that adds 3D acceleration for cards by adding the kernel module necessary for direct rendering.

### Purpose

This guide is for people who can't get direct rendering working with just Xorg. The DRM works for the following drivers:

- 3dfx
- [amdgpu](https://wiki.gentoo.org/wiki/AMDGPU)
- [amdgpu-pro](https://wiki.gentoo.org/wiki/AMDGPU-PRO) (closed source)
- [intel](https://wiki.gentoo.org/wiki/Intel)
- matrox
- [nouveau](https://wiki.gentoo.org/wiki/Nouveau)
- [nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) (closed source)
- rage128
- [radeon](https://wiki.gentoo.org/wiki/Radeon)
- radeonhd (deprecated)
- mach64
- sis300
- via

See the [DRI homepage](https://dri.freedesktop.org/) for more info and documentation.

## Install Xorg and configure the kernel

### Install Xorg

Please read our [Xorg Configuration Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) to get Xorg up and running.

### Configure the kernel

Probe for the specifics of the relevant hardware.

`root #``emerge --ask sys-apps/pciutils``root #``lspci | grep -Ei "VGA|AGP|3D"`
\# 00:01.0 PCI bridge: Intel Corp. 440BX/ZX/DX - 82443BX/ZX/DX AGP bridge (rev 03)

The output may not match the above due to different hardware, whatever is returned will help inform the user as to what kernel options to consider.

Certain kernel options will be required, others may cause problems. Hardware specific resources concerning which kernel option for which hardware exist within this wiki and elsewhere.

It is possible however unlikely that the given command returns nothing at all. In this case, the whole output of lspci -nnk would be a good bit of information to review and/or to pass along when seeking help.

In the given example, where the chipset is an AGP chipset, but a different one unsupported by the kernel, some success may be obtained by passing `agp=try_unsupported` as a kernel parameter. This will use Intel's generic routines for AGP support. To add this parameter, edit the bootloader's configuration file. Again, required kernel config options and available bootloader parameters will vary based on hardware specifics.

Before proceeding to change any kernel options, if necessary, make sure /usr/src/linux links to the intended kernel:

`root #``ls -dl /usr/src/linux*` lrwxrwxrwx  1 root root   19 Feb 16 16:30 /usr/src/linux -> linux-5.4.18-gentoo
drwxr-xr-x 27 root root 4096 Feb 16 21:27 /usr/src/linux-4.19.103-gentoo

In order to update the symlink to another target, use these commands. The second command takes an argument from the options displayed in the output of the first:

`root #``eselect kernel list``root #``eselect kernel set`
With the symlink verified, to adjust kernel options:

`root #````
cd /usr/src/linux
```
`root #````
make menuconfig
```
Most, if not all, kernels should have these options set. In any case, options labeled \<M> are compiled as modules and loaded-as-necessary, thus being very unlikely to break anything if not required.

This was configured using a standard [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) kernel.

**Hardware 3D acceleration options**

Processor type and features --->
\<M/\*> MTRR (Memory Type Range Register) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MTRR\</code> to find this item.
Device drivers --->
  Graphics support --->
  \<M/\*> /dev/agpgart (AGP Support) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\</code> to find this item. --->
      \<M/\*> AMD Opteron/Athlon64 on-CPU GART support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_AMD64\</code> to find this item.
      \<M/\*> Intel 440LX/BX/GX, I8xx and E7x05 chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_INTEL\</code> to find this item.
      \<M/\*> SiS chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_SIS\</code> to find this item.
      \<M/\*> VIA chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_VIA\</code> to find this item.
      (Enable the appropriate chipset above. 32-bit kernels for i386 have additional chipset options:)
      \<M/\*> ALI chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_ALI\</code> to find this item.
      \<M/\*> ATI chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_ATI\</code> to find this item.
      \<M/\*> AMD Irongate, 761, and 762 chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_AMD\</code> to find this item.
      \<M/\*> NVIDIA nForce/nForce2 chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_NVIDIA\</code> to find this item.
      \<M/\*> Serverworks LE/HE chipset support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_SWORKS\</code> to find this item.
      \<M/\*> Transmeta Efficeon support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\_EFFICEON\</code> to find this item.
   \<M/\*> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\</code> to find this item.  --->
   \<M/\*> (Select at least one appropriate graphics card from the list in \[Device drivers --> Graphics support\])

### Compile and install the kernel

`root #``make && make install && make modules_install`
Don't forget to set up grub.conf or lilo.conf.

When using [LILO](https://wiki.gentoo.org/wiki/LILO), issue:

`root #``lilo`
When using [GRUB2](https://wiki.gentoo.org/wiki/GRUB2), run:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
### Add appropriate user(s) to the video group

Next, add the appropriate user(s) to the video group:

`root #``gpasswd -a $USER video`
## Configure direct rendering

### Configure Xorg

Hopefully just adding the appropriate user to the `video` group is sufficient to enable direct rendering. However, Xorg may need some additional configuration via the /etc/X11/xorg.conf.d/ directory. New configuration files created in this directory may be named any alpha-numeric file name, as long as the file suffix ends in .conf. Open up a favorite text editor and create a file with this inside it:

**`/etc/X11/xorg.conf.d/10-dri.conf`**

```
Section "Device"
  Identifier "AMD Radeon"
  Driver "radeon"
EndSection
Section "dri"
  Mode 0666
EndSection
```
Replace `radeon` with the name of the appropriate driver.
Replace `AMD Radeon` with a unique and descriptive string for the identifier. Usually this will be the graphics device name, which figured out in the beginning of this section, but it can be also any other unique string.

### Changes to automatic module loading

You will need to add the module name that your card uses to /etc/modules-load.d/video.conf to ensure that the module is loaded automatically when the system starts up.

**`/etc/modules-load.d/video.conf`**

## Test 3D acceleration

### Reboot to the new kernel

Reboot your computer to your new kernel and login as a normal user. It's time to see if you have direct rendering and how good it is. `glxinfo` and `glxgears` are part of the [x11-apps/mesa-progs](https://packages.gentoo.org/packages/x11-apps/mesa-progs) package, so make sure it is installed before you attempt to run these commands.

`user $``startx`
No need to load modules for your driver or agpgart, even if you compiled them as a module. They will be loaded automatically.

`user $``glxinfo | grep rendering`
direct rendering: Yes

If it outputs "No", you don't have 3D acceleration.

`user $``glxgears`
Test your frames per second (FPS) at the default size. The number should be significantly higher than before configuring DRM. Do this while the CPU is as idle as possible.

### Get the most out of direct rendering

If you want to set more features, for performance or other reasons, check out the [feature matrix](https://dri.freedesktop.org/wiki/FeatureMatrix) on the DRI web site or the [features listing](http://dri.sourceforge.net/doc/dri_driver_features.phtml) on Sourceforge.

## Troubleshooting

### Problem with rendering

Try `modprobe radeon` before you start the X server (replace `radeon` with the name of your driver). Also, try building agpgart into the kernel instead of as a module.

### Failed to load kernel module agpgart when running startx

error: "\[drm\] failed to load kernel module agpgart" after invoking startx is caused by presents of compiled agpgart in the kernel instead of as a module. Ignore it unless you're having problems.

### TV-Out on Radeon GPU

The drivers originally developed by the [GATOS](http://gatos.sourceforge.net/) project have been merged into Xorg's codebase. You don't need anything special for TV-Out; [x11-drivers/xf86-video-ati](https://packages.gentoo.org/packages/x11-drivers/xf86-video-ati) will work just fine.

### Compatibility for freshly released GPUs

Try out the binary drivers. For AMD cards, use `ati-drivers`. If those don't support it, use `fbdev`. It's slow, but it works.

### PCI card doesn't work properly

Create a config file in /etc/X11/xorg.conf.d/; name it anything you want as long as it ends in .conf. Add the following to it:

**`/etc/X11/xorg.conf.d/10-pcimode.conf`**

**Adding ForcePCI Mode**

```
Section "Device"
  Option "ForcePCIMode" "True"
EndSection
```
## External resources

- [Direct rendering (DRI) using X11-DRM HOWTO](https://forums.gentoo.org/viewtopic.php?t=46681) on the Gentoo forums
- [Radeon 7000-9700 DRI CVS Install Guide](https://forums.gentoo.org/viewtopic.php?t=29264) on the Gentoo forums
- [https://dri.freedesktop.org/](https://dri.freedesktop.org/)
