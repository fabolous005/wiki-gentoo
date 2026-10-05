<!-- source: https://wiki.gentoo.org/wiki/Kernel/Configuration/Kernel_Seeds | group: Gentoo Wiki (Main) | wiki-title: Kernel/Configuration/Kernel Seeds -->
---
title: Kernel/Configuration/Kernel Seeds
url: https://wiki.gentoo.org/wiki/Kernel/Configuration/Kernel_Seeds
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-09-28"
fingerprint: "1cd92f7e3523bd23"
license: CC BY-SA 4.0
---

# Kernel/Configuration/Kernel Seeds

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Kernel seeds are pre-made kernel configuration files. They are not necessary to use when configuring a kernel, but they, along with tools such as [genkernel](https://wiki.gentoo.org/wiki/Genkernel), are helpful for users who may be unfamiliar with configuring the Linux kernel.

## Concepts

Once upon a time a Gentoo user by the name of Pappy had an idea. Since configuring the Linux kernel can take much time out of a person's day, why not share generic configuration files with other users? Sharing a configuration file is as easy as putting a link to the file on the internet for any user to download. In Pappy's own words, Kernel Seeds are a "sane make defconfig for the real world."

What are the benefits of using Kernel Seed?

- Most default settings will be automatically configured.
- Time can be saved; using a pre-configured Seed is more efficient than starting without a .config file.

What are the hindrances of using Kernel Seeds?

- Negates learning the "tough stuff" for the user.
- In many cases users need to perform additional configuration steps in order to make the Seed bootable; seeds are not bootable "out-of-the-box".
- Usually the process (see below) takes longer than using Genkernel.

## Configuration

Setting up the environment to use a Kernel Seed is not difficult at all, it's as simple a copying the .config file off the web into the current set of Kernel sources.

Verify the symlink is set to the newest emerged Kernel sources:

`root #``eselect kernel list`
Substitute `<number>` in the next command with the desired Kernel sources number (left column) in the list printed by the previous command:

`root #``eselect kernel set <number>`
Grab the Kernel Seed configuration file off the internet by using a utility of choice. wget ([net-misc/wget](https://packages.gentoo.org/packages/net-misc/wget)) is used in the next example:

`root #``wget [Kernel-Seed-URL] -O /usr/src/linux/.config`
Figure out what additional drivers the system will need by inspecting the PCI and USB devices with the appropriate tools. Installing [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils) and [sys-apps/usbutils](https://packages.gentoo.org/packages/sys-apps/usbutils) will be required for this step, as they contain the utilities for the task at hand.

`root #````
lspci -nnk
```
`root #````
lsusb -vv
```
View the output generated from lspci and lsusb. Add any needed drivers to the kernel configuration by using the menu target of choice (menuconfig, xconfig, gconfig):

`root #``make menuconfig`
Once everything is set, run the the following make commands to built the new kernel:

`root #``make && make modules_install`
Verify /boot is mounted (if applicable) and then install the newly build sources to the /boot location:

`root #````
mount /boot
```
`root #````
make install
```
If needed, perform additional steps to add the new kernel to the system's boot loader. Supposing the system uses [GRUB](https://wiki.gentoo.org/wiki/GRUB):

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Reboot the system. After (if) the system reboots successfully look at the output of dmesg. A full log be can viewed by looking at the dmesg log file located at /var/log/dmesg with the pager of choice.
