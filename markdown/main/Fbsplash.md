<!-- source: https://wiki.gentoo.org/wiki/Fbsplash | group: Gentoo Wiki (Main) | wiki-title: Fbsplash -->
---
title: fbsplash
url: https://wiki.gentoo.org/wiki/Fbsplash
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: "3703aaf70b86abdd"
license: CC BY-SA 4.0
---

# fbsplash

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

**fbsplash** was a Gentoo implementation of a bootsplash screen to provide some eye candy during boot up, shut down or while working on a terminal without X. It was available in Gentoo as [media-gfx/splashutils](https://packages.gentoo.org/packages/media-gfx/splashutils) but [has been removed](https://gitweb.gentoo.org/repo/gentoo.git/commit/media-gfx/splashutils?id=3119a14e237a11489eaa12ec120a8771111e98aa).

The rest of this article is for historical purposes only.

## Installation

### Emerge

Core fbsplash functionality is provided by [media-gfx/splashutils](https://packages.gentoo.org/packages/media-gfx/splashutils) package, which can be installed by the following command:

`root #``emerge --ask media-gfx/splashutils`
Themes are provided by following packages:

- [media-gfx/splash-themes-gentoo](https://packages.gentoo.org/packages/media-gfx/splash-themes-gentoo)
- [media-gfx/bootsplash-themes](https://packages.gentoo.org/packages/media-gfx/bootsplash-themes)
- [media-gfx/splash-themes-livecd](https://packages.gentoo.org/packages/media-gfx/splash-themes-livecd)
- [media-gfx/splash-themes-livedvd](https://packages.gentoo.org/packages/media-gfx/splash-themes-livedvd)

### Kernel

To use a splash image the kernel needs to support [initramfs](https://wiki.gentoo.org/wiki/Initramfs) and it must also support framebuffer console decorations.

Normally, [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) have already been patched to support framebuffer console decorations. If a kernel other than gentoo-sources is being used, it may need to be patched first using genpatches.

Enable framebuffer console decorations:

**Kernel framebuffer configuration**

Remember that selecting the type of framebuffer device is important and affects the system. When planning to use open source graphics drivers, the appropriate device should be enabled here. For example, for radeon drive, enable ATI Radeon display support. However, if you are going to install proprietary drivers for the graphics card, you will likely have to rely on either Userspace VESA VGA graphics support or Simple framebuffer support. There is an option for EFI-based Framebuffer Support, too, in case you need it.

Thus, using either [radeon](https://wiki.gentoo.org/wiki/Radeon), [intel](https://wiki.gentoo.org/wiki/Intel), or [nouveau](https://wiki.gentoo.org/wiki/Nouveau) drivers provides a framebuffer capable device which is well implemented into the operating system. For users with the binary nVidia or AMD driver, there are workarounds using simple framebuffer to get frambuffer support.

Next, enable [Event interface](https://wiki.gentoo.org/wiki/Libinput#Kernel) (`CONFIG_INPUT_EVDEV`) support for framebuffer themes:

**Enabling the Event Interface**

## Usage

General usage scenarios.

### Themes

The different themes are stored in /etc/splash where the folder represents the theme name. To test some of them on the console use:

`root #``splash_manager -c set --theme=natural_gentoo --tty=1`
This will change the theme on *tty1* to *natural\_gentoo*. Any theme in /etc/splash that matches the current display resolution can be selected. Now to see the theme go to tty1 by pressing `Ctrl`+`Alt`+`F1`.

To preview a theme in silent mode without reboot, try:

`root #``splash_manager -c demo -t natural_gentoo -m s --steps=100`
For more reference please refer to:

`root #``splash_manager --help`
### Creating and updating an initramfs image

Add the fbcondecor service to the boot runlevel (OpenRC):

`root #``rc-update add fbcondecor boot`
This way fbsplash would start when the system starts. However, the eye-candy would not be available immediately after boot and there would be some lag. Using an initramfs is the remedy to have it early at boot. Thus, we are going to make an initramfs.

Before you proceed, you must choose the theme and the resolution you want. While it seems trivial, the point is that not every theme you want might support your desired resolution. More important than this, the resolution that you want, might not be supported by the framebuffer device. So there are two steps to be done.

#### Finding the resolutions supported by the framebuffer device

Reveal the supported resolutions by your framebuffer device by issuing the following command:

`root #``cat /sys/class/graphics/fb0/modes`
If you have selected simple frame buffer device support in kernel, you do not need to do this step. Please refer to the troubleshooting section.

#### Finding a theme with the supported resolution

Next, choose a theme that supports the resolution you want. For example, to find out which themes support full HD resolution:

`root #``find /etc/splash/ -name *1080*.cfg`
#### Genkernel

If the initramfs is created using [genkernel](https://wiki.gentoo.org/wiki/Genkernel), the configuration in /etc/genkernel.conf should be changed to the theme used.

**`/etc/genkernel.conf`**

```
# Enable splashutils in early space (initrd). Default is "no".
SPLASH="yes"
# Use this splash theme. If commented out - the "default" name theme is used.
# Also, SPLASH="yes" needs to be enabled for this one to one work.
# This supersedes the "SPLASH_THEME" option of /etc/conf.d/splash (in early space).
SPLASH_THEME="emerge-world"
```
Alternatively the /etc/conf.d/splash could be used to configure the theme, which will be loaded after initramfs.

#### Updating an initramfs image without using genkernel

If you have an existing initramfs (like one you have created with [dracut](https://wiki.gentoo.org/wiki/Dracut)) you want to use this feature. To append the theme to an existing initramfs:

`root #``splash_geninitramfs --verbose --res NxN --append /path/to/initramfsimage theme`
#### Creating an initramfs image without using genkernel

To create a new initramfs for our theme:

`root #``splash_geninitramfs --verbose --res NxN --generate /boot/initramfs-bootsplash theme`
To create a new initramfs with the content of an existing one to add our theme:

`root #``splash_geninitramfs --verbose --res NxN --copy /path/to/initramfsdir theme`
### Bootloader

In [GRUB](https://wiki.gentoo.org/wiki/GRUB) the parameters can be added to the variable `GRUB_CMDLINE_LINUX_DEFAULT` in /etc/default/grub.

Then run:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
This enables the *verbose* version of the *emerge-world* theme, a full list of parameters can be found in /usr/share/doc/splashutils\*/kernel\_parameters. The parameter `video=1920x1080` is optional, but will choose the proper resolution for the image.

### Screenshots

To make screenshots of the framebuffer console, the tool [media-gfx/fbgrab](https://packages.gentoo.org/packages/media-gfx/fbgrab) can be used:

`user $``fbgrab fbscreen.png`
![Bootsplash.png](https://wiki.gentoo.org/images/thumb/7/7c/Bootsplash.png/1024px-Bootsplash.png)


## Troubleshooting

### The theme does not load at boot

First, check if you have built the initramfs with the supported resolution. Maybe the device does not support it, or maybe the theme does not have any configuration files for it. Another possibility is that the initramfs is not loaded at boot.

### The screen stays black until X starts

Disable every framebuffer device in kernel and enable only simple framebuffer one.

### The device does not support the desired resolution

This is a limitation in the device if the resolution you want is not listed among the supported modes. Fortunately, there is a workaround -- simply disable every framebuffer device and enable simple frame buffer device.

**Kernel framebuffer configuration**


Then, to the default GRUB configuration, add:

**`/etc/default/grub`**

```
GRUB_GFXMODE=1920x1080
```
Of course, replace the resolution with whatever you would like and rebuild GRUB configuration file by issuing:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Enjoy the eye-candy!
