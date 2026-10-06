<!-- source: https://wiki.gentoo.org/wiki/Genkernel | group: Gentoo Wiki (Main) | wiki-title: Genkernel -->
---
title: genkernel
url: https://wiki.gentoo.org/wiki/Genkernel
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-02"
fingerprint: "561b1b3e13a23fa1"
license: CC BY-SA 4.0
---

# genkernel

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

*Not to be confused with[gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit).*


**Resources**

**Article status**

- Article must be updated for genkernel-4 in general

- Add documentation for --boot-font feature

- Add documentation for --bcache feature

- Add documentation for gk.keep feature (including /etc/initramfs.mounts)

genkernel is a tool created by Gentoo used to automate the build process of the [kernel](https://wiki.gentoo.org/wiki/Kernel) and [initramfs](https://wiki.gentoo.org/wiki/Initramfs). Some of the general features include:

- Configuring the kernel sources.
- Building the compressed kernel bzImage and copying it to /boot.
- Creating an initramfs and copying it to /boot.
- Creating symlinks in /boot.
- Adding custom content to the initramfs such as encryption related files, boot splash images, extra modules, and more.
- Configuring the [bootloader](https://wiki.gentoo.org/wiki/Bootloader) to boot the newly created kernel and initramfs.

## Installation

### USE flags


### USE flags for
            [sys-kernel/genkernel](https://packages.gentoo.org/packages/sys-kernel/genkernel)
            
            Gentoo automatic kernel building scripts

### Emerge

Initiate the installation of genkernel:

`root #``emerge --ask sys-kernel/genkernel`
## Usage

The general form of genkernel invocation is as follows:

`root #``genkernel [options ...] action`
### Options

The actual behavior of genkernel depends on a large variety of options, the majority of which can be set/unset in the /etc/genkernel.conf file or passed via the genkernel command on each invocation. Options passed over the command line take precedence over options defined in /etc/genkernel.conf. The configuration file is very well documented, however some of the most commonly used options will be explored in this article. The goal is for the reader to be familiar with common genkernel invocations. For a more complete set of explanations refer to the comments in /etc/genkernel.conf itself or to the output of man genkernel.

#### Options acting on user interactivity

The configuration options listed below help the user decide how to interact with the configuration process. Users can even choose whether or not the configuration file created in the process should be saved. The following are considered primary configuration options:

- `--config=/path/to/genkernel.conf`
- Points to the Genkernel configuration file to use (defaults to /etc/genkernel.conf).

- `--kernel-config=/path/to/kernel.config`
- Points to the kernel configuration file to use. To re-use the kernel configuration from the currently running kernel, it is possible to specify /proc/config.gz if it exists. By default, genkernel uses the config from the previous build of the same kernel version or a default kernel config if there isn't a previous config. Use the special value *default* to force usage of genkernel's generic kernel config. This location **must not** be /usr/src/linux/.config as it risks being lost or causing errors.

- `--[no-]menuconfig`
- Activates (or deactivates) the make menuconfig command (which invokes an interactive configuration menu) before building the kernel.

- `--gconfig`
- Provides a kernel configuration utility which depends on the GTK libraries. The advantage of this option is that most users find it easier and clearer to configure the kernel using this tool, since it relies on the X-windowing system. The disadvantage of this option is that the X-windowing system is required to use it, so it will not work on the command line.

- `--xconfig`
- Provides a kernel configuration utility which depends on the QT libraries. The advantage of this option is that most users find it easier and clearer to configure the kernel using this tool, since it relies on the X-windowing system. The disadvantage of this option is that the X-windowing system is needed to use it, so it will not work on the command line.

- `--[no-]save-config`
- Saves \[or does not save\] the kernel configuration to a file in the /etc/kernels directory for later use.

- `--kernel-append-localversion=-mycfg42`
- Allows the modification of kernel's `LOCALVERSION` value which will affect kernel's and and initramfs' filename in /boot and modules in /lib/modules/ directory. This is especially helpful to build a new independent revision with its own kernel binary, initramfs and modules. The example above, with [=sys-kernel/gentoo-sources-5.4.2](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources), would create /boot/vmlinuz-5.4.2-gentoo-x86\_64-mycfg42, /boot/initramfs-5.4.2-gentoo-x86\_64-mycfg42.img and /lib/modules/4.19.88-gentoo-x86\_64-mycfg42 on **amd64** with default settings.

#### Options acting on the resulting system

The configuration options listed here defines which features will or will not be enabled in the resulting kernel and initrd/[initramfs](https://wiki.gentoo.org/wiki/Initramfs).

- `--[no-]splash`
- Activates (or deactivates) support for [Fbsplash](https://wiki.gentoo.org/wiki/Fbsplash) framebuffer splash in the genkernel-built initrd image. To override the default theme used by fbsplash, use `--splash=PreferredTheme` (where `PreferredTheme` is the title of one of the directories inside the /etc/splash directory).

- `--splash-res=PreferredResolution`
- This option allows to select which splash screen resolutions will be supported in the initrd during the start-up of the system. This is useful for two reasons. First, to be able to select only the splash screen resolution(s) relevant to the system. Second, to avoid the unnecessary increase in the disk space required by initrd (since the initrd does not have to support resolutions that are irrelevant for the system's configuration.) However, the option is best omitted if the kernel is being compiled for an Installation CD; this allows splash support for all possible resolutions.

- `--do-keymap-auto`
- Force keymap selection during the boot sequence.

- `--keymap`
- Enables keymap selection at boot (e.g. for LUKS container password input). The desired keymap can be passed in boot options as the value of `keymap` (name of the keymap file without .map). Available keymap files can be found in /usr/share/genkernel/defaults/keymaps/.
- If a needed keymap is not available in genkernel, it is possible to convert one of the existing [sys-apps/kbd](https://packages.gentoo.org/packages/sys-apps/kbd) keymaps (available in /usr/share/genkernel/defaults/keymaps/) using `loadkeys --bkeymap`. See loadkeys man page for details.

- `--lvm`
- Includes support for storage using via [Logical Volume Management (LVM2)](https://wiki.gentoo.org/wiki/LVM) from static binaries, if available to the system. Relevant (static) LVM2 binaries are compiled when unavailable. Be sure to install the [sys-fs/lvm2](https://packages.gentoo.org/packages/sys-fs/lvm2) package on the system (emerge sys-fs/lvm2) before enabling this option.

- `--dmraid`
- Includes support for DMRAID; the utility which creates RAID mappings using the kernel device-mapper subsystem. [DMRAID](http://people.redhat.com/~heinzm/sw/dmraid/readme) discovers, activates, deactivates and displays properties of software RAID sets (ATARAID, for example) and contained DOS partitions.

- `--luks`
- Includes support for [Linux Unified Key Setup](https://wiki.gentoo.org/wiki/LUKS). This will allow to use a device encrypted by LUKS which contains the root filesystem. On the bootloader, set that encrypted device as the value of `crypt_root`, and `root` shall be the unencrypted device LUKS creates ( i.e /dev/mapper/root will be path to mount, and proper boot parameters line will look, like this: `GRUB_CMDLINE_LINUX="... crypt_root=UUID=<crypto_LUKS uuid>` **root=/dev/mapper/root** ..."
).

- `--iscsi`
- Adds support for iSCSI to the initrd.

- `--multipath`
- Adds support for [Multipath](https://wiki.gentoo.org/wiki/Multipath) to the initrd.

- `--linuxrc=/path/to/the/linuxrc_file`
- Specifies a user-created linuxrc — a script that is initialized during the start-up stage of the kernel, prior to the actual boot process. A default linuxrc script can be found in the /usr/share/genkernel/ directory. This script allows booting into a small, modularized kernel; it tries to load the minimum amount of drivers that are needed (as modules) by the system.

- `--cachedir=/path/to/alt/dir`
- Overrides the default cache location used while compiling the kernel.

- `--tmpdir=/path/to/new/tempdir`
- Specifies the location of the temporary directory used by genkernel while compiling the kernel.

- `--unionfs`
- Includes support for the [Unification File System](http://www.fsl.cs.sunysb.edu/project-unionfs.html) in the initrd image.

- `--mountboot`
- Detects whether or not the /boot directory needs to be mounted on a separate partition. It will check /etc/fstab script for instructions on how to mount the boot partition on a file system (if needed).

- `--microcode`
- Creates an early [cpio](https://wiki.gentoo.org/wiki/Cpio) filled with [microcode](https://wiki.gentoo.org/wiki/Microcode) for Intel/AMD processors for Linux early microcode support.

#### Options acting on the choice of the tools used for building

The following options are supported by genkernel, and are passed to the relevant applications while the kernel is being assembled. The options affect low level kernel compilation tools.

- `--kernel-cc=someCompiler`
- Specifies the compiler employed during the kernel compilation process.

- `--kernel-ld=someLinker`
- Specifies the linker employed during the kernel compilation process.

- `--kernel-as=someAssembler`
- Specifies the assembler employed during the kernel compilation process.

- `--kernel-make=someMake`
- Specifies an alternative to the GNU make utility employed during the kernel compilation process.

- `--utils-cc=someCompiler`
- Specifies the compiler employed during the compilation of support utilities.

- `--utils-ld=someLinker`
- Specifies the linker employed during the compilation of support utilities.

- `--utils-as=someAssembler`
- Specifies the assembler employed during the compilation of support utilities.

- `--utils-make=someMake`
- Specifies an alternative to the GNU make utility employed during the compilation of support utilities.

- `--makeopts=-jX`
- Specifies the number of concurrent threads that the make utility can implement while the kernel (and utilities) are being compiled. The variable `X` is a number to be freely chosen, although the most common values are obtained by adding one (1) to the number of cores used by the system, or just use the number of cores on the system. So, for a system with one core, most common option values are `-j2` or `-j1`; a system with two cores most likely uses the `-j3` or `-j2` options, and so on. (A system with one processor that supports Hyper-Threading™ (HT) Technology can be assumed to have 2 cores, provided Symmetric Multi-Processing (SMP) support is enabled in the kernel.)

#### Options acting on the compilation process

The following options usually take effect during the actual compilation:

- `--kerneldir=/path/to/sources/`
- Specifies an alternative kernel source location, rather than the default /usr/src/linux/ location.

- `--kernel-config=/path/to/config-file`
- Specifies which kernel configuration will be used. By default, genkernel will determine kernel version found in `--kerneldir` and look for kernel config (KV) in /etc/kernels/kernel-config-$KV. If no kernel configuration was found, genkernel will look for a generic kernel configuration in /usr/share/genkernel/$ARCH. A special value `default` can be used to force usage of genkernel's generic kernel configuration.

- `--module-prefix=/path/to/prefix-directory/`
- Specifies a prefix to the directory where kernel modules will be installed (default path is the /lib/modules directory.)

- `--[no-]clean`
- Activates (or deactivates) the make clean command before compiling the kernel. The make clean command removes all object files and dependencies from the kernel's source tree.

- `--[no-]mrproper`
- Activates (or deactivates) the make mrproper command before kernel compilation. Like the make clean command listed above, make mrproper removes all object files and dependencies from the kernel's source tree. However, any previous configuration files (in /path/to/sources/.config or /path/to/sources/.config.old) will also be purged from the kernel's source tree. If it is undesirable that the kernel's .config file keeps disappearing, be sure to disable this option!

- `--oldconfig`
- Issues the make oldconfig command, which attempts to collect configuration information for the system's architecture from a generic script in /usr/share/genkernel. This is a non-interactive process; no user input is entertained. Also, if `--oldconfig` is used in conjunction with `--clean`, the latter option is negated, resulting in the activation of the `--no-clean` option.

- `--[no-]module-rebuild`
- Runs, or does not run emerge @module-rebuild to build out-of-tree modules after kernel and kernel modules have been built. This option is enabled by default.

- `--callback="echo hello"`
- Calls the specified arguments (echo hello, in this case) after the kernel and the relevant modules have been built, but before building the initramfs.

- `--[no-]install`
- Activates (or deactivates) the make install command, which installs the new kernel image, configuration file, initrd image and system map onto the boot partition. Any compiled modules will be installed as well. By default genkernel will attempt mount /boot if it is on a separate partition before running the install command.

- `--no-ramdisk-modules`
- Refrains from copying any modules to the genkernel-created initrd image. This option is an exception to the rule about the `no-` prefix; omission of this prefix creates an invalid genkernel option.

- `--all-ramdisk-modules`
- Copies all available modules to the genkernel-created initrd image.

- `--genzimage`
- Creates the initrd image, prior to the kernel image (this hack currently applies only to PPC Pegasos systems).

#### Debugging options

The use of debugging options during the kernel compilation process controls the amount of information reported, as well as the presentation of said data.

- `--loglevel=<0|1|2|3|4|5>`
- Controls the level of verbosity for information printed by genkernel. The variable `LOGLEVEL` is an integer between 0 and 5. The level '0' represents minimal output (=no output), while '5' provides as much information as possible about genkernel's activities during the kernel compilation and initramfs assemble process.

- `--logfile=/path/to/output_file`
- Log file where genkernel outputs to by default. Defaults to /var/log/genkernel.log.

- `--[no-]color`
- Activates (or deactivates) colored output.

- `--[no-]cleanup`
- Activates (or deactivates) the full post-run cleanup for debug purposes.

### Actions

The action passed on the command line with the genkernel \[options …\] *action* tells genkernel what action to perform. The following actions are supported:

| Action | Description | 
|---|---|
| all | Builds all stages — the initrd, kernel image and modules. | 
| bzImage | Only builds the kernel image. | 
| kernel | Only builds the kernel image and modules. | 
| initramfs | Only builds the initramfs/ramdisk image. | 
| ramdisk | Only builds the initramfs/ramdisk image. | 

## Configuration

### Starting off

Although there are several ways to run genkernel, the least-intrusive approach recommended for most users is provided by genkernel all. Here, a generic configuration which works well for most systems is used. As was mentioned earlier, this approach is not without drawbacks; most of the modules created are useless to the average user and may increase compile time. Below is an illustration of a more efficient approach, achieved by passing certain options to genkernel as root:

`root #``genkernel --luks --no-install --no-clean --menuconfig all`
The above operation causes genkernel to create a kernel capable to open LUKS and LUKS2 encrypted volumes, compile and embed cryptsetup into initramfs (`--luks`), both kernel and initramfs will have to be manually installed (`--no-install`). While preparing the kernel source tree, genkernel will refrain from cleaning out any preexisting object files present in the source tree (`--no-clean`). A menu-driven kernel configuration utility will be displayed that allows the user to select which modules will be built for the system (`--menuconfig`).

Replacing `--no-install` with the `--install` option allows genkernel to automatically install the new kernel in the /boot directory, and will create symlinks if `--symlink` is specified. Using the `--mountboot` option allows genkernel to mount the /boot partition automatically, if necessary.

### Changing the boot directory to /efi

It is now recommended to mount the UEFI system boot partition (ESP) under /efi instead of /boot.

By default, the kernel is written to /boot, which used to be the ESP in certain configurations, but is now on the root partition. Reasons to write the kernel to /efi include:

- Secure Boot expecting a hard coded ESP kernel path
- Making GRUB2 independent of the root partition for stability or security
- GRUB2 not supporting the root file-system (bcachefs)
- Using a grub.cfg not generated by grub-mkconfig


To point genkernel to /efi, either add `--bootdir=/efi` to the `genkernel` command or `BOOTDIR="/efi"` to /etc/genkernel.conf.

**`/etc/genkernel.conf`**

**Changing the boot directory to /efi in /etc/genkernel.conf**

```
# Set the boot directory, default is /boot
BOOTDIR="/efi"
```
### Changing the kernel

The first thing that should be done is to allow the triggering of make menuconfig in the /etc/genkernel.conf file:

**`/etc/genkernel.conf`**

**Forcing the use of the configuration menu in /etc/genkernel.conf**

```
# Run 'make menuconfig' before compiling this kernel?
MENUCONFIG="yes"
```
#### File management

While using genkernel, the user has to be aware of some aspects relating to kernel configuration and kernel image files management and the way the kernel sources are handled by the system.

##### Source files

After issuing emerge -u gentoo-sources, a directory is created under /usr/src/ to store new sources. Normally, the active kernel sources directory is pointed to by the /usr/src/linux symlink.

The /usr/src directory might look like this:

`user $``ls -l /usr/src`
total 24
drwxr-xr-x  6 root root 4096 Dec 16 00:56 .
drwxr-xr-x 12 root root 4096 Dec 15 12:42 ..
-rw-r--r--  1 root root    0 Mar 19  2015 .keep
lrwxrwxrwx  1 root root   22 Dec 16 00:56 linux -> linux-5.3.14-gentoo-r1
drwxr-xr-x 26 root root 4096 Nov 24 03:33 linux-4.19.85-gentoo
drwxr-xr-x 27 root root 4096 Dec  9 15:10 linux-4.19.88-gentoo
drwxr-xr-x 27 root root 4096 Dec 16 00:51 linux-4.19.89-gentoo
drwxr-xr-x 25 root root 4096 Dec 16 00:58 linux-5.3.14-gentoo-r1

The /usr/src/linux symlink can be changed in different ways.

- If the `symlink` USE is set the /usr/src/linux symlink is automatically updated to point to the newly emerged sources.

- If the `symlink` USE is *not* set, the user can change the destination of the symlink using the eselect kernel list followed by the eselect kernel set command.

genkernel will *always* (exclusively) use the sources pointed by the /usr/src/linux symlink.

##### Kernel configuration file

If a kernel compilation has already been run with the active kernel sources, there might be a file inside the /etc/kernels directory that contains the kernel configuration that has been applied while creating the last bzImage of the kernel. This file is named, for example kernel-config-5.3.14-gentoo-r1-x86\_64-wifitest2 where `x86_64` might be substituted with the system's architecture, `5.3.14-gentoo-r1` might be substituted with the [package/version of the sources used](https://gitweb.gentoo.org/repo/gentoo.git/tree/sys-kernel/gentoo-sources/gentoo-sources-5.3.14-r1.ebuild?id=9557f104aa85b65c7d394c52c5c8d8727a111651), and `wifitest2` with the `LOCALVERSION` appended user value.

It is this kernel-config-5.3.14-gentoo-r1-x86\_64-wifitest2 file that is used as a starting configuration when running genkernel --menuconfig all.

If it is the first time that genkernel is run with the new kernel sources, or if the previous result has not been saved, this file is substituted with a default configuration file that resides at usr/share/genkernel/arch/x86\_64/generated-config where x86\_64 is substituted with the actual architecture.

.

##### Saving the compiled configuration

If the `--save-config` genkernel option is activated, either from the command line or inside /etc/genkernel.conf, the compiled kernel configuration is saved (with the name given above) into the /etc/kernels directory. At the same time, the configuration is saved in the .config file in /usr/src/linux directory but this file is not reused on the next genkernel all run.

##### Installing the kernel and initramfs into the /boot directory

Specifying the `--install` option when invoking genkernel, will ask genkernel to install the kernel image and the initramfs into the /boot directory. In order to run `--install` a convenient manner, set the following in the /etc/genkernel.conf file:

**`/etc/genkernel.conf`**

**Setting up kernel and initramfs auto-saving**

```
# Mount BOOTDIR automatically if it isn't mounted
MOUNTBOOT="yes"
  
# Save the new configuration in /etc/kernels upon
# successful compilation
SAVE_CONFIG="yes"
  
# Make symlinks in BOOTDIR automatically
SYMLINK="yes"
  
# Add new kernel to grub
# Possible values: empty/"no", "grub", "grub2"
BOOTLOADER="grub2"
```
- The first parameter speaks for itself.

- The second parameter tells genkernel to save the compiled kernel configuration into /etc/kernels.

- The last two options tell genkernel to automatically update the grub configuration. In practice, the following happens:
  - If a previous kernel image with the same name already exist, it is renamed by appending .old to its name. A symlink kernel.old is automatically created that points to it.
  - The new kernel takes the place of any kernel with the same name into /boot. If it is the first time a kernel is compiled, a symlink kernel is automatically created that points to the new kernel.

After running genkernel --menuconfig all, the /boot directory might look like this:

`user $``ls -al /boot`
total 69388
lrwxrwxrwx 1 root root      44 Dec 16 00:58 System.map -> System.map-5.3.14-gentoo-r1-x86\_64-wifitest2
-rw-r--r-- 1 root root 3465443 Dec  9 15:05 System.map-4.19.88-gentoo-x86\_64
-rw-r--r-- 1 root root 3465554 Dec 16 00:45 System.map-4.19.89-gentoo-x86\_64
-rw-r--r-- 1 root root 3645309 Dec 15 16:07 System.map-5.3.14-gentoo-r1-x86\_64
-rw-r--r-- 1 root root 3645309 Dec 15 16:07 System.map-5.3.14-gentoo-r1-x86\_64.old
-rw-r--r-- 1 root root 3645309 Dec 16 00:58 System.map-5.3.14-gentoo-r1-x86\_64-wifitest2
drwxr-xr-x 2 root root    4096 Mar 21  2015 efi
drwxr-xr-x 6 root root    4096 Dec 16 01:02 grub
lrwxrwxrwx 1 root root      47 Dec 16 01:02 initramfs -> initramfs-5.3.14-gentoo-r1-x86\_64-wifitest2.img
-rw-r--r-- 1 root root 3631084 Dec  9 15:34 initramfs-4.19.88-gentoo-x86\_64.img
-rw-r--r-- 1 root root 3637148 Dec 16 00:54 initramfs-4.19.89-gentoo-x86\_64.img
-rw-r--r-- 1 root root 3670176 Dec 15 17:41 initramfs-5.3.14-gentoo-r1-x86\_64.img
-rw-r--r-- 1 root root 3669692 Dec 16 01:02 initramfs-5.3.14-gentoo-r1-x86\_64-wifitest2.img
-rw-r--r-- 1 root root  168960 Nov 16 19:10 intel-uc.img
lrwxrwxrwx 1 root root      41 Dec 16 00:58 kernel -> vmlinuz-5.3.14-gentoo-r1-x86\_64-wifitest2
-rw-r--r-- 1 root root 7509552 Dec  9 15:05 vmlinuz-4.19.88-gentoo-x86\_64
-rw-r--r-- 1 root root 7509552 Dec 16 00:45 vmlinuz-4.19.89-gentoo-x86\_64
-rw-r--r-- 1 root root 7784496 Dec 15 16:07 vmlinuz-5.3.14-gentoo-r1-x86\_64
-rw-r--r-- 1 root root 7784496 Dec 15 16:07 vmlinuz-5.3.14-gentoo-r1-x86\_64.old
-rw-r--r-- 1 root root 7784496 Dec 16 00:58 vmlinuz-5.3.14-gentoo-r1-x86\_64-wifitest2

#### Configuring the bootloader

##### extlinux

TODO

##### GRUB Legacy

The symlinks presented above in the bootloader's configuration can be used so that, even if the new kernel is not bootable, the user can always boot on the old one.

To allow the kernel and intird provided by genkernel to run correctly, provide a minimum information in bootloader's configuration file:

- Add `root=/dev/sdaN` to the kernel parameters passed to the kernel image, where /dev/sdaN points to the root partition (`N` is the number of the partition if a partition exists).
- If splash is used, add a suitable mode line such as `vga=0x317` to the parameters passed to the kernel and also add `splash=verbose` or `splash=silent` depending on the verboseness required through the boot process.
- Add the initrd information as required by the bootloader. Consult the [Bootloader Configuration Chapter](https://wiki.gentoo.org/wiki/Handbook:X86/Installation/Bootloader) of the Gentoo Handbook for details on how to make the bootloader initrd-aware.

Here is how the grub.conf file might look.

**`/boot/grub/grub.conf`**

**An example of grub.conf file**

```
# This is a sample grub.conf for use with Genkernel, per the Gentoo handbook 
# http://www.gentoo.org/doc/en/handbook/handbook-x86.xml?part=1&chap=10#doc_chap2
# When not using Genkernel, see the handbook.
# Alternatively, see grub.conf.sample in the GRUB documentation.
  
default 0 
timeout 5
splashimage=(hd1,0)/boot/grub/splash.xpm.gz 
   
title Gentoo Linux 
root (hd0,6) 
kernel /boot/kernel initrd=/dev/ram0 root=/dev/sda7 rootfstype=ext4  
initrd /boot/initramfs
  
title Gentoo Linux old kernel 
root (hd0,6) 
kernel /boot/kernel.old initrd=/dev/ram0 root=/dev/sda7 rootfstype=ext4 
initrd /boot/initramfs.old
```
##### GRUB

TODO

##### systemd-boot

TODO

#### Preserving the working files

The genkernel application automatically saves new changes to the files. If previous changes are to be preserved, then the following actions need to be taken.

- The first file to preserve is the kernel configuration file in /etc/kernels/ If the source has not changed prior to the recompilation of the kernel, the previously used name for this file will be used. So copying the previous configuration file under a different name helps in preserving the information while keeping the file available as a starting point for a new configuration.

- The second important thing is to preserve the already bootable kernel and initramfs images. The way to accomplish this depends on the context:

  1. If the last kernel compiled is bootable, running genkernel will rename this kernel (and similarly initramfs) image to vmlinuz-$KV.old and create a new vmlinux-$KV. This mean that even if the new kernel is not bootable, users will always be able to boot the old one.
  2. If the last kernel compiled is not bootable and sources haven't changed since the user compiled a bootable one, prior to running genkernel, first delete the new kernel image and remove the .old suffix from the last bootable one. Without this, if the newly compiled kernel is not bootable for the second time, the bootable vmlinuz-$KV.old will be kicked out by the renaming of the non bootable vmlinuz-$KV, giving the user an unbootable system. Use the same reasoning for initramfs.

#### Using previous kernel configuration while changing the sources

The previous configuration can be used through the `MENUCONFIG` variable in /etc/genkernel.conf as follows:

**`/etc/genkernel.conf`**

**Setting up make menuconfig**

```
# Run 'make menuconfig' before compiling this kernel
MENUCONFIG="yes"
```
#### Checking that initramfs includes necessary modules/utilities before booting

Before booting the system, it might be wise checking that initramfs includes necessary utilities and modules. For example, to utilize remote unlock capabilities for a headless system using [LUKS](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption), ensure that kernel modules for the network interface card, dropbear, and cryptsetup have been included.

##### Using lsinitrd

Since genkernel-4, created initramfs can be processed using the lsinitrd command from the [sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut) package:

`user $````
lsinitrd /boot/initramfs-5.3.14-gentoo-r1-x86_64-wifitest2.img
```
Image: /boot/initramfs-5.3.14-gentoo-r1-x86\_64-wifitest2.img: 4,5M
========================================================================
Version: Genkernel 4.0.1 (2019-12-16 00:48:10 UTC)
 
Arguments: --boot-font=none --keymap --compress-initramfs --no-microcode-initramfs --ramdisk-modules --busybox --no-btrfs --no-iscsi --no-multipath --no-dmraid --mdadm --lvm --no-unionfs --no-zfs --no-splash --no-strace --no-gpg --luks --no-firmware --firmware-dir=/lib/firmware --ssh --no-e2fsprogs --no-xfsprogs
 
dracut modules:
========================================================================
drwxr-xr-x  16 root     root            0  Dec 16 01:49 .
drwxr-xr-x   2 root     root            0  Dec 16 01:49 bin
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/ash -> busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/\[ -> busybox
-rwxr-xr-x   1 root     root      2351376  Dec 16 01:49 bin/busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/cat -> busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/cut -> busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/echo -> busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/mknod -> busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/mount -> busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/sh -> busybox
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 bin/uname -> busybox
drwxr-xr-x   2 root     root            0  Dec 16 01:49 dev
drwxr-xr-x   8 root     root            0  Dec 16 01:49 etc
-rw-r--r--   1 root     root           24  Dec 16 01:49 etc/build\_date
-rw-r--r--   1 root     root           16  Dec 16 01:49 etc/build\_id
drwxr-xr-x   2 root     root            0  Dec 16 01:49 etc/dropbear
-rw-------   1 root     root          140  Dec 16 01:49 etc/dropbear/dropbear\_ecdsa\_host\_key
-rw-------   1 root     root          806  Dec 16 01:49 etc/dropbear/dropbear\_rsa\_host\_key
prw-r--r--   1 root     root            0  Dec 16 01:49 etc/dropbear/fifo\_root
prw-r--r--   1 root     root            0  Dec 16 01:49 etc/dropbear/fifo\_swap
-rw-r--r--   1 root     root           97  Dec 16 01:49 etc/fstab
-rw-r--r--   1 root     root           14  Dec 16 01:49 etc/group
-rw-r--r--   1 root     root         3742  Dec 16 01:49 etc/initrd.defaults
-rw-r--r--   1 root     root        69232  Dec 16 01:49 etc/initrd.scripts
-rw-r--r--   1 root     root          441  Dec 16 01:49 etc/ld.so.cache
-rw-r--r--   1 root     root           78  Dec 16 01:49 etc/ld.so.conf
drwxr-xr-x   2 root     root            0  Dec 16 01:49 etc/ld.so.conf.d
-rw-r--r--   1 root     root           81  Dec 16 01:49 etc/ld.so.conf.d/05gcc-x86\_64-pc-linux-gnu.conf
-rw-r--r--   1 root     root         2298  Dec 16 01:49 etc/localtime
drwxr-xr-x   3 root     root            0  Dec 16 01:49 etc/lvm
drwxr-xr-x   2 root     root            0  Dec 16 01:49 etc/lvm/cache
-rw-r--r--   1 root     root        95231  Dec 16 01:49 etc/lvm/lvm.conf
-rw-r--r--   1 root     root         2882  Dec 16 01:49 etc/mdadm.conf
drwxr-xr-x   3 root     root            0  Dec 16 01:49 etc/mdev
-rw-r--r--   1 root     root         1172  Dec 16 01:49 etc/mdev.conf
drwxr-xr-x   2 root     root            0  Dec 16 01:49 etc/mdev/helpers
-rwxr-xr-x   1 root     root          666  Dec 16 01:49 etc/mdev/helpers/nvme
-rwxr-xr-x   1 root     root         1295  Dec 16 01:49 etc/mdev/helpers/storage-device
drwxr-xr-x   2 root     root            0  Dec 16 01:49 etc/modprobe.d
-rw-r--r--   1 root     root         1186  Dec 16 01:49 etc/modprobe.d/aliases.conf
-rw-r--r--   1 root     root          122  Dec 16 01:49 etc/modprobe.d/i386.conf
drwxr-xr-x   2 root     root            0  Dec 16 01:49 etc/modules
-rw-r--r--   1 root     root           24  Dec 16 01:49 etc/modules/ataraid
-rw-r--r--   1 root     root           21  Dec 16 01:49 etc/modules/block
-rw-r--r--   1 root     root          180  Dec 16 01:49 etc/modules/crypto
-rw-r--r--   1 root     root           26  Dec 16 01:49 etc/modules/dmraid
-rw-r--r--   1 root     root           23  Dec 16 01:49 etc/modules/firewire
-rw-r--r--   1 root     root          123  Dec 16 01:49 etc/modules/fs
-rw-r--r--   1 root     root           86  Dec 16 01:49 etc/modules/hyperv
-rw-r--r--   1 root     root           40  Dec 16 01:49 etc/modules/iscsi
-rw-r--r--   1 root     root          437  Dec 16 01:49 etc/modules/lvm
-rw-r--r--   1 root     root          194  Dec 16 01:49 etc/modules/mdadm
-rw-r--r--   1 root     root           75  Dec 16 01:49 etc/modules/multipath
-rw-r--r--   1 root     root          214  Dec 16 01:49 etc/modules/net
-rw-r--r--   1 root     root           56  Dec 16 01:49 etc/modules/nvme
-rw-r--r--   1 root     root          519  Dec 16 01:49 etc/modules/pata
-rw-r--r--   1 root     root           83  Dec 16 01:49 etc/modules/pcmcia
-rw-r--r--   1 root     root          158  Dec 16 01:49 etc/modules/sata
-rw-r--r--   1 root     root          523  Dec 16 01:49 etc/modules/scsi
-rw-r--r--   1 root     root          350  Dec 16 01:49 etc/modules/usb
-rw-r--r--   1 root     root          133  Dec 16 01:49 etc/modules/virtio
-rw-r--r--   1 root     root           15  Dec 16 01:49 etc/modules/waitscan
-rw-r--r--   1 root     root           47  Dec 16 01:49 etc/passwd
-rw-r-----   1 root     root           22  Dec 16 01:49 etc/shadow
-rw-r--r--   1 root     root           25  Dec 16 01:49 etc/shells
-rwxr-xr-x   1 root     root        32331  Dec 16 01:49 init
drwxr-xr-x   2 root     root            0  Dec 16 01:49 .initrd
drwxr-xr-x   6 root     root            0  Dec 16 01:49 lib
lrwxrwxrwx   1 root     root            3  Dec 16 01:49 lib32 -> lib
lrwxrwxrwx   1 root     root            3  Dec 16 01:49 lib64 -> lib
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/console
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/dracut
-rw-r--r--   1 root     root          312  Dec 16 01:49 lib/dracut/build-parameter.txt
-rw-r--r--   1 root     root           42  Dec 16 01:49 lib/dracut/dracut-gk-version.info
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/keymaps
lrwxrwxrwx   1 root     root            9  Dec 16 01:49 lib/keymaps/10.map -> croat.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/11.map -> cz.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/12.map -> de.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/13.map -> dk.map
lrwxrwxrwx   1 root     root           10  Dec 16 01:49 lib/keymaps/14.map -> dvorak.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/15.map -> es.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/16.map -> et.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/17.map -> fi.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/18.map -> fr.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/19.map -> gr.map
lrwxrwxrwx   1 root     root           10  Dec 16 01:49 lib/keymaps/1.map -> azerty.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/20.map -> hu.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/21.map -> il.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/22.map -> is.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/23.map -> it.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/24.map -> jp.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/25.map -> la.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/26.map -> lt.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/27.map -> mk.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/28.map -> nl.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/29.map -> no.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/2.map -> be.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/30.map -> pl.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/31.map -> pt.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/32.map -> ro.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/33.map -> ru.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/34.map -> se.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/35.map -> sf.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/36.map -> sg.map
lrwxrwxrwx   1 root     root            8  Dec 16 01:49 lib/keymaps/37.map -> sk-y.map
lrwxrwxrwx   1 root     root            8  Dec 16 01:49 lib/keymaps/38.map -> sk-z.map
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 lib/keymaps/39.map -> slovene.map
lrwxrwxrwx   1 root     root            8  Dec 16 01:49 lib/keymaps/3.map -> bepo.map
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 lib/keymaps/40.map -> trf.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/41.map -> ua.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/42.map -> uk.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/43.map -> us.map
lrwxrwxrwx   1 root     root           10  Dec 16 01:49 lib/keymaps/44.map -> wangbe.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/4.map -> bg.map
lrwxrwxrwx   1 root     root            8  Dec 16 01:49 lib/keymaps/5.map -> br-a.map
lrwxrwxrwx   1 root     root            8  Dec 16 01:49 lib/keymaps/6.map -> br-l.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/7.map -> by.map
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 lib/keymaps/8.map -> cf.map
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 lib/keymaps/9.map -> colemak.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/azerty.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/be.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/bepo.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/bg.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/br-a.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/br-l.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/by.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/cf.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/colemak.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/croat.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/cz.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/de.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/dk.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/dvorak.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/es.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/et.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/fi.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/fr.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/gr.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/hu.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/il.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/is.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/it.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/jp.map
-rw-r--r--   1 root     root          518  Dec 16 01:49 lib/keymaps/keymapList
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/la.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/lt.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/mk.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/nl.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/no.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/pl.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/pt.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/ro.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/ru.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/se.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/sf.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/sg.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/sk-y.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/sk-z.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/slovene.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/trf.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/ua.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/uk.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/us.map
-rw-r--r--   1 root     root         2823  Dec 16 01:49 lib/keymaps/wangbe.map
-rwxr-xr-x   1 root     root       169376  Dec 16 01:49 lib/ld-linux-x86-64.so.2
-rwxr-xr-x   1 root     root      1913648  Dec 16 01:49 lib/libc.so.6
-rwxr-xr-x   1 root     root        26800  Dec 16 01:49 lib/libnss\_dns.so
lrwxrwxrwx   1 root     root           13  Dec 16 01:49 lib/libnss\_dns.so.2 -> libnss\_dns.so
-rwxr-xr-x   1 root     root        51536  Dec 16 01:49 lib/libnss\_files.so
lrwxrwxrwx   1 root     root           15  Dec 16 01:49 lib/libnss\_files.so.2 -> libnss\_files.so
-rwxr-xr-x   1 root     root        88736  Dec 16 01:49 lib/libresolv.so.2
drwxr-xr-x   3 root     root            0  Dec 16 01:49 lib/modules
drwxr-xr-x   3 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2
drwxr-xr-x   5 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/crypto
-rw-r--r--   1 root     root         7152  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/crypto/algif\_rng.ko
drwxr-xr-x   6 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers
drwxr-xr-x   3 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/hid
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/hid/usbhid
-rw-r--r--   1 root     root        66448  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/hid/usbhid/usbhid.ko
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/md
-rw-r--r--   1 root     root        19024  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/md/dm-log.ko
-rw-r--r--   1 root     root        27256  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/md/dm-mirror.ko
-rw-r--r--   1 root     root        49200  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/md/dm-raid.ko
-rw-r--r--   1 root     root        16536  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/md/dm-region-hash.ko
drwxr-xr-x   3 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/net
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/net/intel
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/net/intel/e1000
-rw-r--r--   1 root     root        70480  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/net/intel/e1000/e1000.ko
drwxr-xr-x   6 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/common
-rw-r--r--   1 root     root         6584  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/common/usb-common.ko
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/core
-rw-r--r--   1 root     root       308944  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/core/usbcore.ko
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/host
-rw-r--r--   1 root     root        60416  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/host/ehci-hcd.ko
-rw-r--r--   1 root     root        10616  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/host/ehci-pci.ko
-rw-r--r--   1 root     root        46072  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/host/ohci-hcd.ko
-rw-r--r--   1 root     root        35896  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/host/uhci-hcd.ko
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/storage
-rw-r--r--   1 root     root       126512  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/drivers/usb/storage/usb-storage.ko
drwxr-xr-x   3 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/fs
drwxr-xr-x   2 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/fs/fat
-rw-r--r--   1 root     root        95664  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/fs/fat/fat.ko
-rw-r--r--   1 root     root        16104  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/kernel/fs/fat/msdos.ko
-rw-r--r--   1 root     root        32434  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.alias
-rw-r--r--   1 root     root        42356  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.alias.bin
-rw-r--r--   1 root     root         8132  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.builtin
-rw-r--r--   1 root     root        11529  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.builtin.bin
-rw-r--r--   1 root     root        15196  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.dep
-rw-r--r--   1 root     root        23748  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.dep.bin
-rw-r--r--   1 root     root            0  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.devname
-rw-r--r--   1 root     root         8320  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.order
-rw-r--r--   1 root     root          117  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.softdep
-rw-r--r--   1 root     root        24707  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.symbols
-rw-r--r--   1 root     root        29469  Dec 16 01:49 lib/modules/5.3.14-gentoo-r1-x86\_64-wifitest2/modules.symbols.bin
lrwxrwxrwx   1 root     root            4  Dec 16 01:49 linuxrc -> init
drwxr-xr-x   2 root     root            0  Dec 16 01:49 mnt
drwxr-xr-x   2 root     root            0  Dec 16 01:49 proc
drwxr-xr-x   3 root     root            0  Dec 16 01:49 root
drwx------   2 root     root            0  Dec 16 01:49 root/.ssh
-rw-------   1 root     root          742  Dec 16 01:49 root/.ssh/authorized\_keys
drwxr-xr-x   2 root     root            0  Dec 16 01:49 run
-rw-r--r--   1 root     root            0  Dec 16 01:49 run/utmp
drwxr-xr-x   2 root     root            0  Dec 16 01:49 sbin
-rwxr-xr-x   1 root     root      1105720  Dec 16 01:49 sbin/blkid
-rwxr-xr-x   1 root     root      2813384  Dec 16 01:49 sbin/cryptsetup
lrwxrwxrwx   1 root     root           19  Dec 16 01:49 sbin/dmsetup -> ../usr/sbin/dmsetup
lrwxrwxrwx   1 root     root           19  Dec 16 01:49 sbin/dmstats -> ../usr/sbin/dmstats
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 sbin/init -> ../init
lrwxrwxrwx   1 root     root           15  Dec 16 01:49 sbin/lvm -> ../usr/sbin/lvm
-rwxr-xr-x   1 root     root      1510360  Dec 16 01:49 sbin/mdadm
-rwxr-xr-x   1 root     root      1267904  Dec 16 01:49 sbin/mdmon
drwxr-xr-x   2 root     root            0  Dec 16 01:49 sys
drwxrwxrwt   2 root     root            0  Dec 16 01:49 tmp
drwxr-xr-x   6 root     root            0  Dec 16 01:49 usr
drwxr-xr-x   2 root     root            0  Dec 16 01:49 usr/bin
lrwxrwxrwx   1 root     root           13  Dec 16 01:49 usr/bin/dropbearconvert -> dropbearmulti
lrwxrwxrwx   1 root     root           13  Dec 16 01:49 usr/bin/dropbearkey -> dropbearmulti
-rwxr-xr-x   1 root     root      1365144  Dec 16 01:49 usr/bin/dropbearmulti
-rwxr-xr-x   1 root     root         2881  Dec 16 01:49 usr/bin/login-remote.sh
lrwxrwxrwx   1 root     root           13  Dec 16 01:49 usr/bin/scp -> dropbearmulti
drwxr-xr-x   2 root     root            0  Dec 16 01:49 usr/lib
lrwxrwxrwx   1 root     root            3  Dec 16 01:49 usr/lib32 -> lib
lrwxrwxrwx   1 root     root            3  Dec 16 01:49 usr/lib64 -> lib
drwxr-xr-x   2 root     root            0  Dec 16 01:49 usr/sbin
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/cache\_check -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/cache\_dump -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/cache\_metadata\_size -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/cache\_repair -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/cache\_restore -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/cache\_writeback -> pdata\_tools
-rwxr-xr-x   1 root     root      1262952  Dec 16 01:49 usr/sbin/dmsetup
lrwxrwxrwx   1 root     root            7  Dec 16 01:49 usr/sbin/dmstats -> dmsetup
lrwxrwxrwx   1 root     root           20  Dec 16 01:49 usr/sbin/dropbear -> ../bin/dropbearmulti
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/era\_check -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/era\_dump -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/era\_invalidate -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/era\_restore -> pdata\_tools
-rwxr-xr-x   1 root     root      2905416  Dec 16 01:49 usr/sbin/lvm
-rwxr-xr-x   1 root     root      3061192  Dec 16 01:49 usr/sbin/pdata\_tools
-rwxr-xr-x   1 root     root          609  Dec 16 01:49 usr/sbin/resume-boot
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_check -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_delta -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_dump -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_ls -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_metadata\_size -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_repair -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_restore -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_rmap -> pdata\_tools
lrwxrwxrwx   1 root     root           11  Dec 16 01:49 usr/sbin/thin\_trim -> pdata\_tools
-rwxr-xr-x   1 root     root         3076  Dec 16 01:49 usr/sbin/unlock-luks
drwxr-xr-x   3 root     root            0  Dec 16 01:49 usr/share
drwxr-xr-x   2 root     root            0  Dec 16 01:49 usr/share/udhcpc
-rwxr-xr-x   1 root     root         1098  Dec 16 01:49 usr/share/udhcpc/default.script
drwxr-xr-x   3 root     root            0  Dec 16 01:49 var
drwxr-xr-x   2 root     root            0  Dec 16 01:49 var/log
-rw-r--r--   1 root     root            0  Dec 16 01:49 var/log/lastlog
-rw-r--r--   1 root     root            0  Dec 16 01:49 var/log/wtmp
lrwxrwxrwx   1 root     root            6  Dec 16 01:49 var/run -> ../run
========================================================================

The output above shows the e1000.ko file for an Intel NIC, the dropbear executable (usr/bin/dropbearmulti), and the cryptsetup (sbin/cryptsetup) executable have been embedded into the initramfs file.

##### Manual extraction

To extract a generated initramfs to inspect its content:

`root #````
mkdir /tmp/initramfs
```
`root #````
cd /tmp/initramfs
```
`root #````
xzcat /boot/initramfs-5.3.14-gentoo-r1-x86_64-wifitest2.img | cpio -idmv
```
`root #````
ls -l sbin/cryptsetup
```
-rwxr-xr-x   1 root     root      2813384  Dec 16 01:49 sbin/cryptsetup

### Microcode loading

For microcode (ucode) updates, kernel must support (early-)microcode loading and microcode updates must be present early at boot. See [Microcode](https://wiki.gentoo.org/wiki/Microcode) article for more details.

#### Microcode loading support in kernel

By default, genkernel will enable microcode loading support in kernel for both, AMD and Intel processors. This behavior can be controlled through `MICROCODE` option in /etc/genkernel.conf or by using the `--microcode=(no|all|amd|intel)` command-line parameter during invocation.

#### Embedding microcode updates into initramfs

To embed microcode (ucode) updates into initramfs, `MICROCODE_INITRAMFS` must be enabled in /etc/genkernel.conf or command-line parameter `--microcode-initramfs` must be passed at invocation. This will cause genkernel to prepend microcode(s) for selected processor (see `--microcode` option above) to initramfs in case [sys-firmware/intel-microcode](https://packages.gentoo.org/packages/sys-firmware/intel-microcode) with the `split-ucode` USE flag for Intel processors and/or [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) for AMD processors is installed.

### Firmware loading

Specific firmware files can be added to genkernel's automatically generated initramfs when listed, with relative paths, in the variable `FIRMWARE_FILES` in /etc/genkernel.conf. When [sys-kernel/genkernel](https://packages.gentoo.org/packages/sys-kernel/genkernel) is installed with `USE="firmware"` it will prefer firmware files from /lib/firmware.

**`/etc/genkernel.conf`**

**Including firmware in the initramfs**

```
# Add firmware(s) to initramfs
FIRMWARE="yes"
 
# Specify directory to pull from
FIRMWARE_DIR="/lib/firmware"
 
# Specify a comma-separated list of firmware files or directories to include,
# relative to FIRMWARE_DIR.  If empty or unset, the full contents of 
# FIRMWARE_DIR will be included (if FIRMWARE option above is set to YES).
FIRMWARE_FILES="<comma-separated list of firmware files here>"
```
In case [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) is installed with `USE="experimental"` and the kernel is configured with `CONFIG_GENTOO_PRINT_FIRMWARE_INFO=y`, the following command gets a comma-separated list of all currently loaded firmware files for the use in the `FIRMWARE_FILES` variable from /etc/genkernel.conf as illustrated above (the output is just an example):

`root #``dmesg -t | grep '^Loading firmware*' | sed 's/^Loading\sfirmware:\s//' | echo $(cat) | tr ' ' ','` amdgpu/green\_sardine\_sdma.bin,amdgpu/green\_sardine\_asd.bin,amdgpu/green\_sardine\_ta.bin,amdgpu/green\_sardine\_pfp.bin,amdgpu/green\_sardine\_me.bin,amdgpu/green\_sardine\_ce.bin,amdgpu/green\_sardine\_rlc.bin,amdgpu/green\_sardine\_mec.bin,amdgpu/green\_sardine\_dmcub.bin,amdgpu/green\_sardine\_vcn.bin,regulatory.db,regulatory.db.p7s,rtw89/rtw8852a\_fw.bin,rtl\_bt/rtl8852au\_fw.bin,rtl\_bt/rtl8852au\_config.bin,rtl\_nic/rtl8168h-2.fw

It is also possible to incorporate the firmware into the kernel image directly, but be aware that `CONFIG_EXTRA_FIRMWARE` in the kernel configuration file .config (normally found in /usr/src/linux) requires a space-separated list (output example):

`root #``dmesg -t | grep '^Loading firmware*' | sed 's/^Loading\sfirmware:\s//' | echo $(cat)` amdgpu/green\_sardine\_sdma.bin amdgpu/green\_sardine\_asd.bin amdgpu/green\_sardine\_ta.bin amdgpu/green\_sardine\_pfp.bin amdgpu/green\_sardine\_me.bin amdgpu/green\_sardine\_ce.bin amdgpu/green\_sardine\_rlc.bin amdgpu/green\_sardine\_mec.bin amdgpu/green\_sardine\_dmcub.bin amdgpu/green\_sardine\_vcn.bin regulatory.db regulatory.db.p7s rtw89/rtw8852a\_fw.bin rtl\_bt/rtl8852au\_fw.bin rtl\_bt/rtl8852au\_config.bin rtl\_nic/rtl8168h-2.fw

### Remote rescue shell

genkernel can embed the [net-misc/dropbear](https://packages.gentoo.org/packages/net-misc/dropbear) SSH daemon into the initramfs which will allow fixing certain things on boot remotely when initramfs is at least able to load. The most common used feature will be remote unlock capability for LUKS-encrypted root or swap devices or [ZFS](https://wiki.gentoo.org/wiki/ZFS) volumes.

#### Pre-requirement for SSH daemon support in initramfs

A authorized\_keys file must exist before genkernel will be invoked. By default, genkernel will look for /etc/dropbear/authorized\_keys. Command-line argument `--ssh-authorized-keys-file=/path/to/custom/authorized_keys` or genkernel configuration option `SSH_AUTHORIZED_KEYS_FILE` can be used to alter default value.

#### Adding SSH support to initramfs

To embed SSH daemon into genkernel's initramfs, run genkernel with `--ssh` command-line argument or set `SSH="yes"` in genkernel configuration file. Needless to mention that this feature will require working network at boot. The following example will just (re-)build initramfs with SSH daemon embedded:

`root #````
genkernel --ssh initramfs
```
\* Gentoo Linux Genkernel; Version 4.0.1
\* Using genkernel configuration from '/etc/genkernel.conf' ...
\* Running with options: --ssh initramfs
 
\* Working with Linux kernel 5.3.14-gentoo-r1-x86\_64 for x86\_64
\* Using kernel config file '/etc/kernels/kernel-config-5.3.14-gentoo-r1-x86\_64' ...
 
\* initramfs: >> Initializing ...
\*         >> Appending devices cpio data ...
\*         >> Appending base\_layout cpio data ...
\*         >> Appending auxilary cpio data ...
\*         >> Appending blkid cpio data ...
\*         >> Appending busybox cpio data ...
\*         >> Appending dropbear cpio data ...
=================================================================
This initramfs' sshd will use the following host key(s):
256 MD5:a5:13:09:90:5b:f6:a1:95:49:9f:87:d9:fa:e5:d8:02 (ECDSA)
256 SHA256:5dxNGEOwH9hvX4+sV4WtzRV/9m8/hrhgnNtTplZf5x8 (ECDSA)
2048 MD5:1d:e6:cc:ce:c8:96:a0:73:3e:4c:2a:56:ce:b9:10:26 (RSA)
2048 SHA256:V4WrMKhfVSxSeW3XIbW8dSaAmXiwN6jiMA/geNKLcqA (RSA)
=================================================================
\*         >> Appending modprobed cpio data ...
\*         >> Appending modules cpio data ...
\*         >> Appending linker cpio data ...
\*         >> Deduping cpio ...
\*         >> Pre-generating initramfs' /etc/ld.so.cache ...
\*         >> Compressing cpio data (.xz) ...
\*
\* You will find the initramfs in '/boot/initramfs-5.3.14-gentoo-r1-x86\_64.img'.
 
\* WARNING... WARNING... WARNING...
\* Additional kernel parameters that \*may\* be required to boot properly:
\* - Add "dosshd" to start SSH daemon in initramfs
 
\* Do NOT report kernel bugs as genkernel bugs unless your bug
\* is about the default genkernel configuration...
\*
\* Make sure you have the latest \~arch genkernel before reporting bugs.

#### Enabling SSH daemon on boot

Just adding SSH daemon to initramfs is not enough. Because exposing any network service could be a security risk, this feature **must** be enabled via the kernel command-line argument `dosshd`! See Configure Network for how to configure network in genkernel.

#### Remote unlock

There are two possibilities to unlock LUKS-encrypted root and/or swap volume: A manual way, through an SSH connection, run a command and will get prompted for passphrase(s) or an automatic way where user passes passphrase through SSH as command.

##### Manual unlock

Connect to the remote system through SSH and run the following commands:

`user $````
ssh root@remote-system-running-genkernel-initramfs-with-dosshd
```
\>> Welcome to Genkernel 4.0.1 (2019-12-16 22:34:14 UTC) remote rescue shell!
>> ...running Linux kernel 5.3.14-gentoo-r1-x86\_64
 
>> The lockfile '/tmp/remote-rescueshell.lock' was created.
>> In order to resume boot process, run 'resume-boot'.
>> Be aware that it will kill your connection which means
>> you will no longer be able to work in this shell.
>> To remote unlock LUKS-encrypted root device, run 'unlock-luks root'.
 
remote rescueshell \~ # unlock-luks root
>> Detected real\_root as a md device. Setting up the device node ...
Enter passphrase for /dev/md126:
>> LUKS device /dev/md126 opened
remote rescueshell \~ # resume-boot
>> Resuming boot process ...

##### Automatic unlock

It's basically the same like manual unlock just without the need to manually run resume-boot. In case user has both, encrypted root and swap volume, user must unlock swap volume first:

`user $````
cat /path/to/secret.key/on/local/disk | ssh root@remote-system-running-genkernel-initramfs-with-dosshd post root
```
\>> Detected real\_root as a md device. Setting up the device node ...
>> LUKS device /dev/md126 opened
>> Resuming boot process ...

### Network booting

#### From an installation CD

The genkernel utility can build kernel and initrd images that provide support for network booting, or netbooting. With any luck, users should be able to netboot any recent computer into the environment provided by the Installation CD.

The magic lies in genkernel's linuxrc script: it will try to netmount the Installation CD using NFS. From there, the init scripts of the Installation CD can take over, as if the CD was present locally.

#### Building the kernel and initramfs with netboot support

To enable support for netbooting, include the following options while configuring the kernel:

First, the kernel image must include the drivers for the system's Network Interface Cards (NIC). Normally, drivers for such devices will be compiled as modules. However, it is essential (for netbooting) that such drivers are compiled directly into the kernel image and not as modules.

**Configuring a 3.x.x series kernel to support various NIC drivers**

```
Device Drivers --->
   Networking Support --->
      Ethernet (10 or 100Mbit)  --->
         [*] Ethernet (10 or 100Mbit)
         <*>   The driver(s) for each network card
```
Secondly, it is suggested that *IP: kernel level autoconfiguration* is enabled as well as *IP: DHCP support options*. This avoids an unnecessary layer of complexity since the IP address and the NFS path to the Installation CD can be configured on a DHCP server. Of course, this means the kernel command line will remain constant for any machine — which is very important for etherbooting.

**Configuring a 3.x.x series kernel to support DHCP**

```
Device Drivers --->
   Networking Support --->
      Networking options
         [*] TCP/IP networking--->
         [*]   IP: kernel level autoconfiguration
         [*]     IP: DHCP support
```
Tells the kernel to send a DHCP request at bootup.

Additionally, enable [SquashFS](https://wiki.gentoo.org/wiki/SquashFS) because most modern Gentoo Installation CDs require it. Support for SquashFS is not included with the generic kernel source tree. To enable SquashFS, apply the necessary patches to the generic kernel source or install gentoo-sources.

**Configuring the kernel to support SquashFS**

```
File systems--->
   Miscellaneous filesystems --->
      [*] SquashFS 2.X - Squashed file system support
```
Once the compilation process is completed, create a compressed tarball (tar.gz) that contains the kernel's modules. This step is only necessary if the kernel version does not match the kernel image version on the Installation CD.

To create an archive containing all the modules:

`root #````
cd /
```
`root #``tar -cf /tmp/modules-X.Y.Z.tar.gz /lib/modules/X.Y.Z/`
Depending on the network boot mechanism, one of the following steps need to be followed:

To create an etherboot image:

`root #``emerge --ask net-misc/mknbi``root #````
cd /boot
```
`root #``mkelf-linux -params="root=/dev/ram0 init=/linuxrc ip=dhcp" kernel... initrd... > etherboot.img`
To create an OpenBoot/SPARC64 TFTP image:

`root #``emerge --ask sys-apps/sparc-utils``root #````
cd /boot
```
`root #````
elftoaout kernel... -o kernel.aout
```
`root #````
piggyback64 kernel.aout System.map-... initrd-...
```
`root #``mv kernel.aout openboot.img`
The openboot.img file is the boot image.

Finally, copy this kernel to the TFTP server. The details are architecture-dependent and are beyond the scope of this guide. Please refer to the documentation for the specific platform of interest.

#### NFS setup

To setup a NFS share that contains the Installation CD, use the loop device to mount the ISO image and then copy the contents of the CD into the NFS share. As a nice extra, genkernel's initrd scripts will extract all tar.gz files located in the /nfs/livecd/add/ directory. All that needs to be done here is copy the modules-X.Y.Z.tar.gz archive to the /nfs/livecd/add/ directory.

The following assumes that /nfs/livecd is an exported NFS share:

`root #````
mount /tmp/gentoo-livecd.iso /mnt/cdrom -o loop
```
`root #````
cp -p /mnt/cdrom /nfs/livecd
```
`root #``umount /mnt/cdrom`
Now copy the modules.tar.gz file into /add:

`root #````
mkdir /nfs/livecd/add
```
`root #``cp /tmp/modules-X.Y.Z.tar.gz /nfs/livecd/add`
#### DHCP setup

The netboot images will ask the DHCP server on the network for an IP as well as a `root-path` option. Both can be specified per host using a MAC address to identify machines:

**`/etc/dhcpd.conf`**

**Sample client dhcpd.conf setup**

```
# Here, 192.168.1.2 is the NFS server while 192.168.1.10 will be the IP address of the netbooted machine
host netbootableMachine {
         hardware ethernet 11:22:33:44:55:66;
         fixed-address 192.168.1.10;
         option root-path "192.168.1.2:/nfs/livecd";
}
```
#### Netbooting instructions

Netbooting itself is again very platform-specific. The important part is to specify the `ip=dhcp` and `init=/linuxrc` parameters on the kernel command line, as this will bring up the network interface and tell the initrd scripts to mount the Installation CD via NFS. Here are some platform-specific tips.

For etherboot, insert the etherboot disk into the drive and reboot. The kernel command line was specified when the image was constructed.

With Sparc64, press `Stop`+`A` at the boot prompt and then enter:

`ok``boot net ip=dhcp init=/linuxrc`
For PXE, setup pxelinux (part of syslinux), then create a pxelinux.cfg/default along the lines of:

**`pxelinux.cfg/default`**

**Default entry**

```
DEFAULT gentoo
TIMEOUT 40
PROMPT 1
  
LABEL gentoo
    KERNEL kernel-X.Y.Z
    APPEND initrd=initrd-X.Y.Z root=/dev/ram0 init=/linuxrc ip=dhcp
```
#### Booting a genkernel initramfs

##### Introduction

If an initramfs is installed with genkernel, then take a look at the various boot options that can (or should) be defined in the bootloader configuration. The most common ones are added to this guide.

##### Loading LVM or software-RAID

If the system uses LVM or software-RAID, the initramfs has to be built using the `--lvm` and `--mdadm` options. Do not forget to enable support during boot as well. This can be done using the dolvm and domdadm options.

**`/boot/grub/grub.conf`**

**Enabling LVM and/or MDADM support**

```
# Example for GRUB 1.x
title Gentoo Linux
root (hd0,0)
kernel /vmlinuz root=/dev/md3 dolvm domdadm
initrd /initramfs-genkernel-x86_64-3.4.3
```
##### Booting in single-user mode

If for some reason boot-up fails, rescuing the system by booting in the single-user mode is still possible. This will only load the really necessary services and then drop the user to a rescue (root) shell.

**`/boot/grub/grub.conf`**

**Booting in single-user mode**

```
# Example for GRUB 1.x
title Gentoo Linux
root (hd0,0)
kernel /vmlinuz root=/dev/md3 init_opts=S
initrd /initramfs-genkernel-x86_64-3.4.3
```
### Cross-compile support

To build kernel and/or initramfs for a different platform as genkernel is being executed on, kernel/initramfs must be cross-compiled.

`root #``genkernel --mdadm --no-install --cross-compile=aarch64-linux-gnu all`
The above command causes genkernel to create a kernel supporting MD raid and embed mdadm into initramfs (`--mdadm`), both kernel and initramfs will have to be manually installed (`--no-install`). The kernel and programs embedded into initramfs will run on arm64 (`--cross-compile=aarch64-linux-gnu`).

- `--cross-compile=<target triplet>`
- Target triple (i.e. `aarch64-linux-gnu`) to build for. Only needed when the system running genkernel has a different architecture like the system which should boot the created kernel/initramfs.

### Initramfs kernel command-line parameters

- `root=<...>`
- Specifies the device node of the root filesystem to mount. I.e. `root=/dev/sda3`, `root=UUID=a1e5968c-bd1b-41ee-bf08-2d0ed376fa83`.

- `cdroot`
- This attempts to load livecd.squashfs and is used for loading live media.

- `crypt_root=<...>`
- This specifies the device encrypted by LUKS, which contains the root filesystem to mount. Supports same syntax like `root` kernel command-line parameter from above.

- `crypt_swap=<...>`
- This specifies the swap device encrypted by LUKS. For more details please see `crypt_root` kernel command-line parameter from above.

- `root_trim=(yes|no)`
- Enables TRIM support for a LUKS-based root device.

- `ip=<dhcp,addr/cidr>`
- Normally used to tell the kernel that it should start a network interface which can be specified using `gk.net.iface` kernel parameter. By default, `dhcp` will be used. A specific IP address can be set using `addr/CIDR` notation, i.e. `1.2.3.4/24`.

- `gk.net.dhcp.retries=<...>`
- Sends up to 3 DHCP discovery requests by default.

- `gk.net.iface=<interface,macaddr>`
- Use the interface named `eth0` by default. Use this kernel parameter to specify another interface.
A MAC address (00:00:00:00:00:00 format) can be specified instead of an interface name.

- `gk.net.gw=<...>`
- Optional gateway. If ip is set to dhcp, this kernel parameter will be ignored.

- `gk.net.routes=<...>`
- Optional additional routes. If ip is set to dhcp, this kernel parameter will be ignored.

- `gk.net.timeout.dad=<...>`
- Wait up to 10 seconds by default for IPv6's DAD to complete. At the moment, only wait for DAD while bringing down an interface to prevent a race condition.

- `gk.net.timeout.deconfiguration=<...>`
- Wait up to 10 seconds by default while bringing down an interface to prevent a race condition.

- `gk.net.timeout.dhcp=<...>`
- Wait up to 10 seconds by default for a DHCP server reply.

- `gk.net.timeout.interface=<...>`
- Wait up to 10 seconds by default for interface to show up.

- `gk.prompt.timeout=<...>`
- By default a prompt within genkernel initramfs like shown when set root could not be found will never timeout. Use this option to set a timeout.

- `dosshd`
- Will bring up an interface and start a SSH daemon within initramfs allowing to remotely unlock encrypted devices or just for debugging purpose. See ip option for how to configure network.

- `gk.sshd.port=<...>`
- By default, sshd will listen on port 22.

- `gk.sshd.wait=<...>`
- Wait X seconds after setting up sshd. Useful for login (and thus pause boot process) before booting real system.

- `dolvm`
- Activate LVM volumes on bootup.

- `lvmraid=<...>`
- Specify RAID devices to set up before the activation of LVM volumes. Implies option `dolvm` from above.

- `domdadm` (Deprecated in Genkernel 4.2.0, udev rules now control MD assembly)
- Scan for RAID arrays on bootup.

- `dozfs[=cache,force]`
- Scan for bootable ZFS pools on bootup. Optionally use cachefile or force import if necessary or perform both actions.

- `gk.log.disabled=<...>`
- By default, any shown message and external command calls will be logged to /tmp/init.log in initramfs. Disables logging.

- `gk.log.keep=<...>`
- When set to a boolean value, genkernel will preserve /tmp/init.log, see above, and copy file to /genkernel-boot.log on root device (see `root` or `real_root` kernel command-line parameter above). Customize to a file like /root/my-genkernel-boot.log to copy the log.

- `gk.hw.load-all=<...>`
- By default, genkernel loads various module groups (nvme, sata, scsi, pata, usb...) until block device specified in `root` parameter (see above) becomes available. This boolean option can be used to force loading of all module groups regardless whether root device is already available when set to `yes`.

## FAQ

### Can genkernel be used for systemd-based systems?

Yes.

### Genkernel? Dracut?

[sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut) in comparison to [sys-kernel/genkernel](https://packages.gentoo.org/packages/sys-kernel/genkernel) is just a generic tool for creating an initramfs. It does not create a kernel like genkernel does. I.e. while both, genkernel and dracut supports booting from LUKS-encrypted root volume, only genkernel will ensure that kernel will have all required options set. It's also worthwhile to mention that genkernel will compile most packages (LVM, cryptsetup, mdadm, sshd...) used in initramfs on its own whereas dracut will copy binaries from host system which can be a problem for some setups. Thus, genkernel supports kernel/initramfs creation for another system (cross-compile) unlike dracut or genkernel-next.

### Can a separate kernel/initramfs be created for tests?

`root #``genkernel --kernel-config=/proc/config.gz --kernel-append-localversion=-test42 --menuconfig all`
The above command causes genkernel to build a new kernel and initramfs (`all`) based on config from current running kernel (`--kernel-config=/proc/config.gz`),  invoke menuconfig (`--menuconfig`) allowing user to adjust configuration and will append `-test42` to kernel's `LOCALVERSION` variable (`--kernel-append-localversion=-test42`) which will affect naming of kernel image, modules dir and initramfs by default.

### How can external modules (such as xtables-addons, nvidia-drivers...) be rebuilt for a new kernel?

By default, genkernel will call emerge @module-rebuild when building a kernel to ensure that out-of-tree modules installed through the package manager are still present in new/rebuilt kernel. This feature can be toggled via `--[no-]module-rebuild` command-line argument or `MODULEREBUILD` in /etc/genkernel.conf.

### How can additional commands be run after building the kernel?

Genkernel provides a callback for that (before version 4, callback was used to rebuild external modules). See `CMD_CALLBACK` in /etc/genkernel.conf for more details.

### How can ccache or distcc be used with genkernel?

Set up [ccache](https://wiki.gentoo.org/wiki/Ccache) or [distcc](https://wiki.gentoo.org/wiki/Distcc) the normal way like for [sys-apps/portage](https://packages.gentoo.org/packages/sys-apps/portage). Set `--kernel-cc` command-line parameter or adjust `KERNEL_CC` in /etc/genkernel.conf for the desired tool to use. Do the same for `UTILS_CC` (`--utils-cc`) and `UTILS_CXX` (`--utils-cxx`).

### ERROR: compile\_kernel(): compile\_generic() failed to compile the "bzImage" target!

Check /var/log/genkernel.log first. In most cases, a root cause will appear like:

**`/var/log/genkernel.log`**

```
[...]
  AR      drivers/usb/built-in.a
  AR      drivers/built-in.a
  GEN     .version
  CHK     include/generated/compile.h
  AR      built-in.a
  LD      vmlinux.o
  MODPOST vmlinux.o
ld: .tmp_vmlinux1: final close failed: No space left on device
make: *** [Makefile:1032: vmlinux] Error 1
 
* ERROR: compile_kernel(): compile_generic() failed to compile the "bzImage" target!
* Please consult '/var/log/genkernel.log' for more information and any
* errors that were reported above.
*
* Report any genkernel bugs to bugs.gentoo.org and
* assign your bug to genkernel@gentoo.org. Please include
* as much information as you can in your bug report; attaching
* '/var/log/genkernel.log' so that your issue can be dealt with effectively.
*
* Please do *not* report kernel compilation failures as genkernel bugs!
*
 
* mount: >> Boot partition state on '/boot' was not changed; Skipping restore boot partition state ...
>>> Ended on: 2019-12-16 02:30:19 (after 0 days 0 hours 07 minutes 49 seconds)
</pre>
```
In other words: The system has run out of disk space (`No space left on device`) during compilation.

To guard against problems like this set `CHECK_FREE_DISK_SPACE_BOOTDIR=50` and `CHECK_FREE_DISK_SPACE_KERNELOUTPUTDIR=4000` in /etc/genkernel.conf in which case genkernel would fail early with a message like

`root #````
genkernel --kernel-config=/proc/config.gz all
```
\* Gentoo Linux Genkernel; Version 4.0.1
\* Using genkernel configuration from '/etc/genkernel.conf' ...
\* Running with options: --kernel-config=/proc/config.gz all
 
\* Working with Linux kernel 4.19.89-gentoo-x86\_64 for x86\_64
\* Using kernel config file '/proc/config.gz' ...
\*
\* Note: The version above is subject to change (depends on config and status of kernel sources).
\* ERROR: 4000 MB free disk space is required in '/usr/src/linux' but only 1026 MB is available!
\[...\]

### Is the order of kernel command-line arguments important?

No.

### Required kernel option 'CONFIG\_MICROCODE\_INTEL' or 'CONFIG\_MICROCODE\_AMD' which genkernel tried to set is missing!

Kernel v6.6 permanently applied the kernel config parameters (see [this commit](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=42a7f6e3ffe06308c1ec43a7dac39a27de101574)). This [was fixed in version 4.3.10](https://gitweb.gentoo.org/repo/gentoo.git/commit/?id=791a15028278c7cc1e463aee8b7d0c42d06583db) of [sys-kernel/genkernel](https://packages.gentoo.org/packages/sys-kernel/genkernel).

### Help! Something isn't working!

To report a problem, please always provide /var/log/genkernel.log (sometimes it maybe necessary to compress that file before sharing or attaching to bugs) which will help developers a lot. Even if the command is run with `--loglevel=1` (default), the logfile will always contain complete output (no need to re-run with logging turned on) which will help developers to understand, reproduce and maybe fix a bug.

## See also

- [Dracut](https://wiki.gentoo.org/wiki/Dracut) — an [initramfs](https://wiki.gentoo.org/wiki/Initramfs) infrastructure and aims to have as little as possible hard-coded into the initramfs.
- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
- [Kernel/Configuration](https://wiki.gentoo.org/wiki/Kernel/Configuration) — describes the manual configuration and setup of the [Linux kernel](https://wiki.gentoo.org/wiki/Kernel).
- [Project:Distribution Kernel](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) — maintains sys-kernel/\*-kernel packages.
