<!-- source: https://wiki.gentoo.org/wiki/Systemd/Installing_Gnome3_from_scratch | group: Gentoo Wiki (Main) | wiki-title: Systemd/Installing Gnome3 from scratch -->
---
title: systemd/Installing Gnome3 from scratch
url: https://wiki.gentoo.org/wiki/Systemd/Installing_Gnome3_from_scratch
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-31"
fingerprint: "403d607d2f430386"
license: CC BY-SA 4.0
---

# systemd/Installing Gnome3 from scratch

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Article status**

- Instructions for configuring a static IP

**2019-06-19**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=Systemd/Installing_Gnome3_from_scratch&action=edit).

To install a desktop environment such as GNOME 3 requires the use of systemd but the present [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) is based on the use of OpenRC. It just indicates that changes have to be made when using systemd but never tells the user where the changes occur.

As for the systemd page, it mainly concentrate on an update starting from an existing system. Thus, it can be a kind of challenge, especially for a new comer to Gentoo, to install a GNOME 3 desktop from scratch. The purpose of this page is to indicate where changes have to be made to the current [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) to do so.

The indications given refer mainly to an **amd64** install.

## Base system installation

Hereafter are the modifications to bring to the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) for an **amd64** platform.

### Part 1 to 5 of the Gentoo Handbook

No changes here.

### Installing the Gentoo base system

#### Choosing the right stage3 tarball

A stage3 tarball already configured for systemd is available for most architectures. You will find it along with the other stage3 tarballs in the autobuilds directory of your preferred mirror. To save time and effort, use this tarball when installing the base system.

#### Choosing the right profile

`root #``eselect profile list`
Available profile symlink targets:
  \[1\]   default/linux/amd64/23.0 (stable)
  \[2\]   default/linux/amd64/23.0/selinux (stable)
  \[3\]   default/linux/amd64/23.0/hardened (stable)
  \[4\]   default/linux/amd64/23.0/hardened/selinux (stable)
  \[5\]   default/linux/amd64/23.0/desktop (stable)
  \[6\]   default/linux/amd64/23.0/desktop/gnome (stable)
  \[7\]   default/linux/amd64/23.0/desktop/gnome/systemd (stable)
  \[8\]   default/linux/amd64/23.0/desktop/plasma (stable)
  \[9\]   default/linux/amd64/23.0/desktop/plasma/systemd (stable)
  \[10\]  default/linux/amd64/23.0/desktop/systemd (stable)

Select the profile ending in gnome/systemd:

`root #``eselect profile set default/linux/amd64/23.0/desktop/gnome/systemd`
On the **amd64** platform there are also two other profiles ending in systemd. Select systemd if a desktop environment is not required for the system (servers, etc). A KDE option plasma/systemd is also available.

An asterisk indicates the currently selected profile. Verify the correct profile has been selected:

`root #``eselect profile list`
Available profile symlink targets:
  \[1\]   default/linux/amd64/23.0 (stable)
  \[2\]   default/linux/amd64/23.0/selinux (stable)
  \[3\]   default/linux/amd64/23.0/hardened (stable)
  \[4\]   default/linux/amd64/23.0/hardened/selinux (stable)
  \[5\]   default/linux/amd64/23.0/desktop (stable)
  \[6\]   default/linux/amd64/23.0/desktop/gnome (stable)
  \[7\]   default/linux/amd64/23.0/desktop/gnome/systemd (stable) \*
  \[8\]   default/linux/amd64/23.0/desktop/plasma (stable)
  \[9\]   default/linux/amd64/23.0/desktop/plasma/systemd (stable)
  \[10\]  default/linux/amd64/23.0/desktop/systemd (stable)

#### Updating the system

The choice of the desktop/gnome/systemd profile implies a lot of changes in the list of packages to be installed. Perform a full system update before going on:

`root #``emerge --ask --update --deep --newuse @world`
### Configuring the kernel

When planning to include support for a graphic card that requires proprietary binary blobs be sure to emerge the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package. Otherwise the kernel will not compile.

`root #``emerge --ask sys-kernel/linux-firmware`
It may not be necessary at this stage, but it does not hurt to add the `VIDEO_CARDS` variable, set with an appropriate video card value, to /etc/portage/make.conf.

For a quick and dirty kernel, use option *menuconfig* with genkernel, and optionally *lvm* and/or *mdadm* when using LVM and/or RAID. Do not forget to install [sys-fs/lvm2](https://packages.gentoo.org/packages/sys-fs/lvm2) and/or [sys-fs/mdadm](https://packages.gentoo.org/packages/sys-fs/mdadm) if you intend to use LVM and/or RAID. Please see [LVM#Software](https://wiki.gentoo.org/wiki/LVM#Software) for details about LVM.

`root #``emerge --ask sys-fs/lvm2``root #``genkernel [--lvm] [--mdadm] --menuconfig all`
Select the systemd init system. You may leave the Openrc option set (if you wish to enable both Openrc and systemd), or you can disable Openrc here, to make the kernel smaller.

**Quick setup using gentoo-sources**

When using LVM activate the following kernel options:

Include settings for your graphic card referring to the ad hoc page on this wiki, most currently [radeon](https://wiki.gentoo.org/wiki/Radeon) and [nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers).

Alternatively, if you want to tailor the kernel yourself, use "make menuconfig", choose systemd (and optionally Openrc) in the Gentoo Linux options as shown above, and select the rest of the kernel options yourself.

`root #``make menuconfig`
When you have configured the kernel, save the configuration, exit the menuconfig dialog, and proceed as explained in the Handbook.

`root #``make && make modules_install``root #``make install`
If you need an initramfs file (and you probably will need one), dracut works very well.

`root #``emerge sys-kernel/dracut``root #``dracut --hostonly --force`
The --force option is necessary because the generated initramfs file will have the same name as the initramfs file used to boot the installation system.

### Configuring the system

#### Defining the keyboard

With systemd, setting `keyboard=fr` into /etc/conf.d/keymaps will be effectless. Instead, you should set this into /etc/vconsole.conf.

The various keymaps are stored into subdirectories of /usr/share/keymaps/ and can be listed with the following command

`root #``localectl list-keymaps`
Once you have you keymap listed, select it with the following command, replacing `fr-dvorak-bepo` with your own keymap:

`root #``localectl set-keymap --no-convert fr-dvorak-bepo`
Double check by running:

`root #``cat /etc/vconsole.conf`
### Installing necessary system tools

#### System logger

syslog-ng conflicts with systemd [Systemd#syslog-ng\_conflicts\_with\_systemd](https://wiki.gentoo.org/wiki/Systemd#syslog-ng_conflicts_with_systemd) systemd creates /dev/log as datagram socket [\[1\]](https://bugzilla.redhat.com/show_bug.cgi?id=770810#c10) [\[2\]](https://fedoraproject.org/wiki/Common_F16_bugs#systemd-syslog-ng-problems) so you will need to tell syslog-ng to read from a *unix-dgram* instead of a *unix-stream* if you are hitting problems and are using "wrong" stream:

**`/etc/syslog-ng/syslog-ng.conf`**

Should be replaced with:

**`/etc/syslog-ng/syslog-ng.conf`**

In order to use the syslog-ng service in systemd.

To enable syslog-ng use the following command:

`root #``systemctl enable syslog-ng`
To disable it, use:

`root #``systemctl disable syslog-ng`
#### Networking tools

dhcpcd will not be started at boot unless you enable dhcpcd.service with systemd.

Refer to the  [systemd](https://wiki.gentoo.org/wiki/Systemd) page to know how to do it before rebooting. Otherwise you will be able to do so after boot with:

`root #``systemctl enable dhcpcd.service``root #``systemctl start dhcpcd.service`
#### Installing systemd itself

`root #``emerge --ask sys-apps/systemd`
### Configuring the bootloader

If you are using lvm, add the `device-mapper` USE flag before emerging grub.

After emerging [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub), edit the **/etc/default/grub** configuration file and add the following line (if using ext4 for root partition):

**`/etc/default/grub`**

**Example grub2-mkconfig config for systemd**

```
# Append parameters to the linux kernel command line
GRUB_CMDLINE_LINUX="rootfstype=ext4 real_init=/usr/lib/systemd/systemd dolvm domdadm"
```
Then run:

`root #````
mkdir /boot/grub
```
`root #````
grub2-mkconfig -o /boot/grub/grub.cfg
```
### Finalizing the installation

It may be necessary to enable and start dcpcd.service and to use systemd to define keyboard layout (see the [systemd](https://wiki.gentoo.org/wiki/Systemd) page).

## X server installation

### Install xorg

#### /etc/portage/make.conf

Don't forget to add the following lines to the /etc/portage/make.conf file:

**`/etc/portage/make.conf`**

```
VIDEO_CARDS="radeon"
```
Then emerge the drivers:

`root #``emerge --ask x11-base/xorg-server`
#### Kernel settings

Follow the page X server to set the kernel parameters for evdev and the kernel parameters for your graphic card if not already done. Recompile the kernel

### Check startx

In order to check the x server install [x11-terms/xterm](https://packages.gentoo.org/packages/x11-terms/xterm) and [x11-terms/twm](https://packages.gentoo.org/packages/x11-terms/twm):

`root #``emerge --ask x11-terms/xterm x11-wm/twm`
Then use the following command:

`root #``startx`
If everything is correct, a graphic page with some terminals inside will appear. Quit by pressing `Ctrl`+`Alt`+`Del` and remove [x11-terms/xterm](https://packages.gentoo.org/packages/x11-terms/xterm) and [x11-terms/twm](https://packages.gentoo.org/packages/x11-terms/twm):

`root #``emerge --ask --depclean --verbose x11-terms/xterm x11-wm/twm`
## GNOME 3 installation

You are now ready to install GNOME 3:

`root #``emerge --ask gnome`
In order to have the GNOME Display Manager after boot, enable it:

`root #``systemctl enable gdm.service`
Reboot and enjoy GNOME 3!
