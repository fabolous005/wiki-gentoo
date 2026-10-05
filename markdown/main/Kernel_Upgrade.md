<!-- source: https://wiki.gentoo.org/wiki/Kernel/Upgrade | group: Gentoo Wiki (Main) | wiki-title: Kernel/Upgrade -->
---
title: Kernel/Upgrade
url: https://wiki.gentoo.org/wiki/Kernel/Upgrade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-12"
fingerprint: "5683113e90e23f27"
license: CC BY-SA 4.0
---

# Kernel/Upgrade

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the steps to upgrade to a newer version of the Linux [kernel](https://wiki.gentoo.org/wiki/Kernel).

Making a new kernel from the new sources is basically the same process as making a kernel when installing the system. The only difference is that one can save time by adjusting the old kernel configuration for the changes made in the new kernel instead of going through all the kernel options (like make menuconfig) again.

A new kernel may have had options or features added or removed since the old kernel. Hence the configuration file of the new kernel may have new entries in the configuration file of the old kernel doesn't have, and it might not have entries which are present in the configuration file of the old kernel anymore.

This article is a guide on how to deal with such changes of the configuration file by converting the old configuration to a configuration that can be used with the new kernel.

Kernel upgrade in Gentoo involves these steps:

1. Install the new kernel sources.
2. Setting the symlink to the (newly installed) kernel sources.
3. Moving to the new kernel's folder.
4. Adjusting the .config file for the options introduced to, or removed from, the new kernel's configuration.
5. Compiling the kernel and the initramfs.
6. Updating the bootloader.
7. Removing or keeping the old kernel.

## Emerging the new kernel sources

A kernel upgrade may be a good idea when new kernel sources are installed. New kernel sources are sometimes installed while updating the system by running the following command:

`root #``emerge --ask --update --deep --with-bdeps=y --newuse @world`
Of course, they can be installed directly using the next command (replace *gentoo-sources* with whatever version of the kernel that is in-use):

`root #``emerge --ask --update --deep --with-bdeps=y --newuse sys-kernel/gentoo-sources`
Installing new kernel sources does not provide the user with a new kernel. It is necessary to build and install a new kernel from the new sources and then reboot the system to actually run the new kernel.

## Set symlink to new kernel sources

The kernel configuration is saved in a file named .config in the directory that holds the kernel sources, a symlink is used to point to that directory.

The symlink /usr/src/linux should always point to the directory that holds the sources of the kernel which currently runs. This can be done in one of three ways:

1. Default: Setting the link with eselect.
2. Alternative 1: Manually updating the symbolic link.
3. Alternative 2: Installing the kernel sources with `USE="symlink"`.

### Default: Setting the link with eselect

To set the symlink with eselect:

`user $``eselect kernel list`
Available kernel symlink targets:
 \[1\] linux-3.14.14-gentoo \*
 \[2\] linux-3.16.3-gentoo

This outputs the available kernel sources. The asterisk indicates the chosen sources.

To change the kernel sources, e.g. to the second entry, do:

`root #``eselect kernel set 2`
### Alternative 1: Manually updating the symbolic link

To set the symbolic link manually:

`root #``ln -sf /usr/src/linux-3.16.3-gentoo /usr/src/linux``user $``ls -l /usr/src/linux`
lrwxrwxrwx 1 root root 19 Oct  4 10:21 /usr/src/linux -> linux-3.16.3-gentoo

### Alternative 2: Installing the kernel sources with the symlink USE flag

This will make the /usr/src/linux point to the newly installed kernel sources.

If necessary, it can still be modified later with one of the other two methods.

## Moving to the new kernel folder

Now that the symbolic link has been modified, change the working directory to the new kernel folder.

`user $``cd /usr/src/linux`
## Adjusting the .config file for the new kernel

The configuration of the old kernel needs to be copied to the new one. The old configuration can be found in several places:

- In the [procfs](https://wiki.gentoo.org/wiki/Procfs) filesystem, if the kernel option *Enable access to .config through /proc/config.gz* (CONFIG\_IKCONFIG\_PROC) was activated in the present kernel (it may be necessary to use modprobe to install the configs module if configs was built as a module):

`root #``zcat /proc/config.gz > /usr/src/linux/.config`
- From the old kernel. This will only work when the old kernel was compiled with CONFIG\_IKCONFIG:

`root #``/usr/src/linux/scripts/extract-ikconfig /path/to/old/kernel >/usr/src/linux/.config`
- In the /boot directory, if the configuration was installed there:

`root #``cp /boot/config-3.14.14-gentoo /usr/src/linux/.config`
- In the kernel directory of the currently-running kernel:

`root #``cp /usr/src/linux-3.14.14-gentoo/.config /usr/src/linux/.config`
- In the /etc/kernels/ directory, if `SAVE_CONFIG="yes"` is set in /etc/genkernel.conf and [genkernel](https://wiki.gentoo.org/wiki/Genkernel) was previously used:

`root #``cp /etc/kernels/kernel-config-x86_64-3.14.14-gentoo /usr/src/linux/.config`
### Update the .config file

A new kernel usually requires a new .config file to support new kernel features. The .config from the old kernel can be converted to be used with the new kernel. The conversion can be done several ways including running either make oldconfig or make olddefconfig.

#### make oldconfig

The following configuration is like the text based configuration with make config. For new configuration options, the user is asked for a decision. For example:

`root #``cd /usr/src/linux``root #``make oldconfig`
Anticipatory I/O scheduler (IOSCHED\_AS) \[Y/n/m/?\] (NEW)

The string *(NEW)* at the end of the line marks this option as new. Left to the string in square brackets are the possible answers: *Y*es, *n*o, *m*odule or *?* to show the help. The recommend (i.e. default) answer is capitalized (here *Y*). The help explains the option or driver.

Unfortunately make oldconfig doesn't show a lot more information for each option, such as the context, so it is sometimes difficult to give the right answer. In this case the best way to go is to remember the option name and revise it afterwards through one of the [graphical kernel configuration tools](https://wiki.gentoo.org/wiki/Kernel/Configuration#Configuration_tools). For listing new options and doing research about them, make listnewconfig can be used before running make oldconfig.

#### make olddefconfig

Running make olddefconfig will keep all of the options from the old .config and set the new options to their recommended (i.e. default) values:

`root #``cd /usr/src/linux``root #``make olddefconfig`
#### make help

Use make help to see other conversion methods available:

`user $``make help`
#### Observing the difference

A diff tool can be used to compare the old and new .config files to see what options have been added:

`user $````
comm -2 -3  <(sort .config) <(sort .config.old)
```
\# CONFIG\_BATTERY\_RT5033 is not set
# Compiler: gcc (Gentoo 12.2.1\_p20230428-r1 p2) 12.2.1 20230428
# Linux/x86 4.19.284-gentoo Kernel Configuration
CONFIG\_ARCH\_USE\_MEMREMAP\_PROT=y
CONFIG\_GCC\_VERSION=120201
CONFIG\_GENTOO\_PRINT\_FIRMWARE\_INFO=y
CONFIG\_INET\_TABLE\_PERTURB\_ORDER=16
CONFIG\_PLUGIN\_HOSTCC=""
CONFIG\_RANDOM\_TRUST\_BOOTLOADER=y
CONFIG\_UNIX\_SCM=y
CONFIG\_USER\_NS=y

And which have been removed:

`user $````
comm -1 -3  <(sort .config) <(sort .config.old)
```
\# CONFIG\_GCC\_PLUGINS is not set
# CONFIG\_NVM is not set
# CONFIG\_RANDOM\_TRUST\_CPU is not set
# CONFIG\_USER\_NS is not set
# Compiler: gcc (Gentoo 10.2.0-r5 p6) 10.2.0
# Linux/x86 4.19.184-gentoo Kernel Configuration
CONFIG\_GCC\_VERSION=100200
CONFIG\_PLUGIN\_HOSTCC="g++"

Alternatively, the kernel provides a script to cleanly compare two config files even if the options have moved in the file itself:

`user $``/usr/src/linux/scripts/diffconfig .config.old .config`
The options can then be researched and changed if necessary by running:

`root #``make menuconfig`
The menuconfig target is helpful because it safely handles kernel symbol dependency resolution.

### Manual build and installation

After configuring the new kernel as described in the previous sections, proceed to compile it. If external kernel modules (such as *nvidia* or *zfs*) are installed on your system, they must be rebuilt against the newly compiled kernel to ensure compatibility. Start by compiling the new kernel:

`root #````
make
```
`root #````
emerge --ask @module-rebuild
```
Having built both the kernel and the modules, both should be installed:

`root #````
make modules_install
```
`root #````
make install
```
Finally, the bootloader must be reconfigured to account for the new kernel filenames, as described [below](https://wiki.gentoo.org/wiki/Kernel/Upgrade#Update_the_bootloader). initramfs must be rebuilt if one is used as well.

### Automated build and installation

It is possible to automatically build and install the newly emerged kernel using Portage hooks. While other approaches are also possible, the following is based on genkernel and gentoo-sources package. It requires the following prerequisites:

1. genkernel all is able to build and install the kernel to which the /usr/src/linux symlink points into `$BOOTDIR` and the bootloader.
2. The `symlink` use flag is set for the kernel ebuild.

If those are fulfilled, simply install a `post_pkg_postinst` Portage hook as shown below.   Keep in mind this calls genkernel with --`no-module-rebuild`, since using module-rebuild would run emerge in emerge, and result in a deadlock waiting on the lock file.  Remember to run `emerge @module-rebuild` after any update that includes a kernel upgrade.

**`/etc/portage/env/sys-kernel/gentoo-sources`**

**Automated kernel build and installation portage hook**

```
() {
# Eselect the new kernel or genkernel will build the current one
	eselect kernel set linux-"${KV}"
	CURRENT_KV=$(uname -r)
# Check if genkernel has been run previously for the running kernel and use that config
	if [[ -f "${EROOT}/etc/kernels/kernel-config-${CURRENT_KV}" ]] ; then
		genkernel --no-module-rebuild --kernel-config="${EROOT}/etc/kernels/kernel-config-${CURRENT_KV}" all
# Use latest kernel config from current kernel
	elif [[ -f "${EROOT}/usr/src/linux-${CURRENT_KV}/.config" ]] ; then
		genkernel --no-module-rebuild --kernel-config="${EROOT}/usr/src/linux-${CURRENT_KV}/.config" all
# Use known running good kernel
	elif [[ -f /proc/config.gz ]] ; then
		zcat /proc/config.gz >> "${EROOT}/tmp/genkernel.config"
		genkernel --no-module-rebuild --kernel-config="${EROOT}/tmp/genkernel.config" all
		rm "${EROOT}/tmp/genkernel.config"
# No valid configs known, compile a clean one
	else
		genkernel --no-module-rebuild all
	fi
}
```
### Solving build problems

When experiencing build problems while rebuilding the current kernel, it might help to sanitize the kernel sources. Make sure to backup the .config file first, as the operation will remove it. Make sure not to use a .bak or \~ suffix as backup as make distclean will clean those up as well.

`root #````
cp .config /usr/src/kernel_config_bk
```
`root #````
make distclean
```
`root #``mv /usr/src/kernel_config_bk .config`
## Update the bootloader

Refer to the [bootloader](https://wiki.gentoo.org/wiki/Bootloader), EFI stub entry [Installation](https://wiki.gentoo.org/wiki/EFI_stub_kernel#Installation), or [Handbook](https://wiki.gentoo.org/wiki/Handbook) pages to use the new kernel.

### Systemd-boot

A [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot) configuration file for the new kernel is generated automatically when the kernel is installed. No manual action is required.

### Using grub-mkconfig

Make sure /boot partition is mounted.

The following command can be executed for updating [GRUB](https://wiki.gentoo.org/wiki/GRUB)'s configuration file:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
## Removing the old kernel

See the [kernel removal](https://wiki.gentoo.org/wiki/Kernel/Removal) article.

## See also

- [Dracut](https://wiki.gentoo.org/wiki/Dracut) — an [initramfs](https://wiki.gentoo.org/wiki/Initramfs) infrastructure and aims to have as little as possible hard-coded into the initramfs.
- [Genkernel](https://wiki.gentoo.org/wiki/Genkernel) — a tool created by Gentoo used to automate the build process of the [kernel](https://wiki.gentoo.org/wiki/Kernel) and [initramfs](https://wiki.gentoo.org/wiki/Initramfs).
- [Kernel/Configuration](https://wiki.gentoo.org/wiki/Kernel/Configuration) — describes the manual configuration and setup of the [Linux kernel](https://wiki.gentoo.org/wiki/Kernel).
- [Updating GRUB to a new kernel](https://wiki.gentoo.org/wiki/GRUB#Installing_a_new_kernel)
