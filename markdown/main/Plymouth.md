<!-- source: https://wiki.gentoo.org/wiki/Plymouth | group: Gentoo Wiki (Main) | wiki-title: Plymouth -->
---
title: Plymouth
url: https://wiki.gentoo.org/wiki/Plymouth
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-30"
fingerprint: "7a03073f1ffea9de"
license: CC BY-SA 4.0
---

# Plymouth

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Plymouth** is a [bootsplash](https://en.wikipedia.org/wiki/bootsplash) used to show splash screens during system boot and shutdown.

Plymouth provides flicker-free animated boot splashes with support for progress bars, solar flares, and other nifty things. In addition to [OpenRC](https://wiki.gentoo.org/wiki/OpenRC), it has full [systemd](https://wiki.gentoo.org/wiki/Systemd) support.

## Installation

### Kernel

Specific kernel options must be altered in order to get Plymouth working properly. If using a distribution kernel, continue at [installing Plymouth](https://wiki.gentoo.org#USE_flags).

#### Bootup logo

It is *highly* advised to **disable** the Linux bootup logo. On some systems having the bootup logo displayed seems to cause problems.

**This example shows the correct way to disable the bootup logo:**

#### KMS for Intel cards

**Intel onboard GPUs set to use modesetting:**

#### KMS for Nvidia cards (Nouveau drivers)

**NVIDIAGPU set to use Nouveau:**

#### KMS for Nvidia cards (official drivers)

To use the official Nvidia drivers see the wiki's [official Nvidia-drivers article](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers).

#### KMS for Radeon cards

**Radeon GPU set to use modesetting:**

### USE flags


| [+drm](https://packages.gentoo.org/useflags/+drm) | Provides abstraction to the DRM drivers (intel, nouveau and vmwgfx at this moment) | 
| [+gtk](https://packages.gentoo.org/useflags/+gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [+pango](https://packages.gentoo.org/useflags/+pango) | Adds support for printing text on splash screen and text prompts, e.g. for password | 
| [+split-usr](https://packages.gentoo.org/useflags/+split-usr) | Enable this if /bin and /usr/bin are separate directories | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [freetype](https://packages.gentoo.org/useflags/freetype) | Build with freetype support (if enabled, used for encryption prompts) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

### Emerge

The [sys-boot/plymouth](https://packages.gentoo.org/packages/sys-boot/plymouth) package can be installed by running:

`root #``emerge --ask sys-boot/plymouth`
## Configuration

/etc/plymouth/plymouthd.conf - the sole configuration file for Plymouth. It can be left untouched. Selecting theme is described in  [further section](https://wiki.gentoo.org#Themes).

## Building Initramfs

Bootsplashes are loaded by [initramfs](https://wiki.gentoo.org/wiki/Initramfs) so they need to be included in it.

### Initramfs Generators

#### Dracut

[Dracut](https://wiki.gentoo.org/wiki/Dracut) ([sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut)) is an initramfs generator created by the Fedora development team. Fun fact: Plymouth and Dracut are both cities in Massachusetts. Though this is speculation, the creators of these programs might have taken this into consideration.

Dracut should enable Plymouth automatically if it is installed. See the [Dracut installation instructions](https://wiki.gentoo.org/wiki/Dracut#Installation).

### Manual initramfs creation

When creating a manual initramfs, for example using [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs), it is possible to bundle Plymouth.

First, disable `udev` USE flag for Plymouth (it will not display anything until udev is properly initialized). Otherwise udev will need to be added to the initramfs as well.

`root #``echo "sys-boot/plymouth -udev" >> /etc/portage/package.use``root #``emerge sys-boot/plymouth`
Then, populate the initramfs with the plymouth files and theme. While it may be done manually, the better way is utilizing /usr/libexec/plymouth/plymouth-populate-initrd. It will deliver all the binaries, config and themes needed (only the theme that is enabled, so do not forget to re-execute when changing theme).

The script will copy all the files to selected directory.

Add this code snippet to the initramfs generation script, just before the initramfs packing:

**`/path/to/the/generator.sh`**

**Initramfs generation script**

```
 
# populate plymouth if available
if [ -x /usr/libexec/plymouth/plymouth-populate-initrd ]
then
        /usr/libexec/plymouth/plymouth-populate-initrd -t $INITRAM_DIR
fi
 
....
```
Finally, update the actual init script in initramfs:

**`/path/to/init`**

**Example init file**

```
 
# Plymouth needs /dev/pts mounted
mount -t devtmpfs none /dev                                                                                                                                                                                      
mkdir /dev/pts                                                                                                                                                                                                   
mount -t devpts /dev/pts /dev/pts
 
....
 
# early in the process start plymouthd and show splash
if [[ -x /usr/sbin/plymouthd -a -x /usr/bin/plymouth ]]
then
        mkdir -p /run/plymouth
        /usr/sbin/plymouthd --attach-to-session --pid-file /run/plymouth/pid --mode=boot
        /usr/bin/plymouth show-splash
fi
 
....
```
### Init systems

#### systemd

Plymouth automatically registers itself with systemd to show splash screens during shutdown and restart. No additional configuration is required.

#### OpenRC

There is a plugin for Plymouth that extends a single line version of OpenRC's status to the framebuffer. It can be installed via:

`root #``emerge --ask sys-boot/plymouth-openrc-plugin`
Next, ensure OpenRC is not running interactively:

**`/etc/rc.conf`**

**RC configuration for Plymouth example**

```
rc_interactive="NO"
```
Plymouth should run next time the system boots. To remove this functionality simply uninstall the plugin.

## Bootloaders

### GRUB

When using [GRUB](https://wiki.gentoo.org/wiki/GRUB), it must be configured to enable the splash screen during early boot. Append the options `quiet splash` to the `GRUB_CMDLINE_LINUX_DEFAULT` variable. It may be desirable to adjust the resolution in the `GRUB_GFXMODE` variable to match the desired resolution for the monitor, and set `GRUB_GFXPAYLOAD_LINUX` to "keep" in order to preserve the graphics mode during the entire boot.

**`/etc/default/grub`**

**Configuring GRUB for Plymouth**

## Themes

After emerging Plymouth, a number of themes will be pulled in automatically, however more Plymouth themes can be downloaded from the web and installed manually. Extract the downloaded themes to the Plymouth theme directory: /usr/share/plymouth/themes

Make sure each new theme is contained in its own folder (just like the default themes that are installed) or they will not be detected by Plymouth.

Once the themes have been extracted, verify successful extraction by requesting Plymouth generate a list of all available themes. Do this using the plymouth-set-default-theme command:

`root #``plymouth-set-default-theme --list`
To get a preview of the individual themes the following command can be used. Running plymouth from within X can stay on top, preventing the usage of the desktop, hence the killall command after five seconds. It will set a theme, start plymouthd and show the theme, wait five seconds and then kill plymouthd to again allow access to the X session:

`root #``plymouth-set-default-theme solar; plymouthd; plymouth --show-splash; sleep 5; killall plymouthd`
Assuming the solar theme is desired as the system's theme, run:

`root #``plymouth-set-default-theme solar`
It is possible to create themes for Plymouth. See the [Theme creation article](https://wiki.gentoo.org/wiki/Plymouth/Theme_creation) for more information.

There is also a [Theming guide](https://wiki.gentoo.org/wiki/User:DerpDays/Plymouth/Theming) which provides a detailed introduction into creating themes.

### Usage

Check for the latest kernel using:

`root #``eselect kernel list`
Regenerate the initramfs using the dracut command:

`root #``dracut --kver <latest kernel> --force`
As long as the configuration has been performed properly, this will pack the selected Plymouth theme into the initramfs.

Finally, regenerate GRUB config to use the initramfs and apply the [GRUB graphical settings](https://wiki.gentoo.org#GRUB):

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Alternatively, portage can be used to used regenerate the initramfs on distribution kernels, if [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) has [initramfs](https://packages.gentoo.org/useflags/initramfs) [and the correct bootloader USE flags set:](https://wiki.gentoo.org/wiki/USE_flag)  

`root #``emerge --config gentoo-kernel`
For users using binary distribution kernels:

`root #``emerge --config gentoo-kernel-bin`
## Tips

According to the README file distributed with Plymouth, boot messages are dumped to /var/log/boot.log after the root filesystem has been mounted read-write.

## External resources

- [Plymouth on gentoo](https://anderse.wordpress.com/2009/11/05/plymouth-on-gentoo/) - Anders Evenrud's Blog (old)
- [Red Hat 7's Plymouth documentation](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Desktop_Migration_and_Administration_Guide/plymouth.html) - Describes how to create a theme using the two-step plugin.
- [Theming guide](https://wiki.gentoo.org/wiki/User:DerpDays/Plymouth/Theming) for creating themes using the script plugin.
