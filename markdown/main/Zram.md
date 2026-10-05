<!-- source: https://wiki.gentoo.org/wiki/Zram | group: Gentoo Wiki (Main) | wiki-title: Zram -->
---
title: zram
url: https://wiki.gentoo.org/wiki/Zram
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: "9289205aada3b9a0"
license: CC BY-SA 4.0
---

# zram

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**zram** (previously called **compcache**) is a [Linux kernel](https://wiki.gentoo.org/wiki/Kernel) feature and set of userspace tools for creating compressible RAM-based block devices. It has been included as a [module](https://wiki.gentoo.org/wiki/Kernel_Modules) of the mainline Linux kernel since version 3.14. Starting with kernel version 3.15, zram supports multiple compression algorithms, which can be changed without a system restart.

The zram kernel module creates compressed block devices in RAM. These block devices can then be used for [swap](https://wiki.gentoo.org/wiki/Swap) or general purpose RAM disks.

The RAM used for the block device is dynamically obtained and released up to its predefined uncompressed maximum size. The way it extends the amount of available RAM to a system is by using a portion of the RAM as compressed swap. It can therefore hold more pages of memory in the compressed swap than the amount of actual memory used.

Typically it compresses to a 3:1 ratio. So, 1 GiB of swap uses only 333 MiB of RAM on average. The compression ratio including memory used for overhead varies depending on the percent of maximum space used. This may vary from around 1.5:1 for a 1.5 GiB disk with only 5% space used, to over 3:1 when nearly full. It also is much faster at swapping pages than hard disks. Depending on the actual hardware and use-case, this can result in (sometimes significant) performance gains, or losses.[\[1\]](https://wiki.gentoo.org#cite_note-1)

To be more memory efficient, zram offers the possibility to configure a backing store, which holds blocks that are not compressible or which have not been accessed in a while.

Combining zram with a correctly tuned Portage configuration should keep a desktop system running in a responsive manner, even during intensive software compilation.

## Caveats

On systems with limited memory, not using any swap will reduce the amount of available memory needed for running applications. When the available memory runs out, the *Out of Memory* (OoM) issue will occur. Using zram can reduce Out of Memory issues but sometimes it can't be helped when running memory intensive applications, such as compiling in Portage. For this use case, zswap may be suitable, see [zswap](https://wiki.gentoo.org/wiki/Zswap)

When using zram with OpenRC and a SSD, avoid setting `rc_parallel=YES` in /etc/rc.conf (which starts services in parallel). Depending on the size of the zram partitions and the speed of the RAM, some swap partitions and filesystems might not be ready when the swap and localmount services are started.

In such case, if parallel must be used, consider removing these services from the boot runlevel, and adding them to default instead.

## Enabling zram support in kernel

Enable the following options in the kernel's configuration file:

```
Device Drivers  --->
    [*] Block devices 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BLK_DEV</code> to find this item. --->
        <M> Compressed RAM block device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_ZRAM</code> to find this item.
        [*] Write back incompressible or idle page to backing device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_ZRAM_WRITEBACK</code> to find this item.
        [*] Track zRam block status [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_ZRAM_MEMORY_TRACKING</code> to find this item.
It is recommended that zram be built as a loadable module. This allows changing number of zram devices without rebooting, by deactivating zram devices and re-loading module with new parameters. If zram is built-in, then the number of devices can only be changed at boot time by using the kernel boot parameter `zram.num_devices=#`.

In order to use the LZ4 compression algorithm, enable it in kernel config:

```
Cryptographic API --->
    Compression --->
        <*> LZ4 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO_LZ4</code> to find this item.
### Snippet

**`/etc/kernel/config.d/zram-linux6-1-111.config`**

## Initialization

### Using zramctl

The easiest way to create a zram device is to use **zramctl**, which is a part of the [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) package. To create a 4G zram device using zramctl with lz4 compression and use it for swap:

`root #``modprobe zram``root #``zramctl /dev/zram0 --size 4G --algorithm lz4``root #``mkswap /dev/zram0``root #``swapon /dev/zram0`
See [zramctl(8)](https://man.archlinux.org/man/zramctl.8.en) [for examples of usage.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

### Using OpenRC

Using **zramctl** works, but it is temporary and will be reset after reboot. To keep the change consistent, create two /etc/local.d files: zram.start and zram.stop. OpenRC will run these during the appropriate as part of service run process when booting or changing run levels.

For example:

- 4G RAM total
- Configure 6G zram swap and activate.
- Estimated maximum ram used 2G @ 3:1 compression ratio.

**`/etc/local.d/zram.start`**

```
#!/bin/bash
 
modprobe zram
zramctl /dev/zram0 --size 6G --algorithm lz4
mkswap /dev/zram0
swapon /dev/zram0 -p 10
```
**`/etc/local.d/zram.stop`**

```
#!/bin/bash
 
swapoff /dev/zram0
echo 1 > /sys/block/zram0/reset
modprobe -r zram
```
Now make both of these files executable:

`root #``chmod +x /etc/local.d/zram.start /etc/local.d/zram.stop`
Add local service to default runlevel if it is not already added:

`root #``rc-update add local default`
#### Using zram in /var/tmp/portage

For manual installation of zram mounted in /var/tmp/portage, the following script can be used to create 4G of zram with lzo compression:

### Using zram-init service

The same thing can also be achieved by using Martin Väth's zram-init script which allows for easy multiple zram devices management through one script.

`root #``emerge --ask sys-block/zram-init`
#### OpenRC

Edit the /etc/conf.d/zram-init file and create/configure the desired zram devices. There are lots of comments/instructions in the file. So proceed with editing and be sure to save it when the appropriate modifications have been made.

An example configuration:

**`/etc/conf.d/zram-init`**

```
load_on_start="yes"
unload_on_stop="yes"
num_devices="3"
 
type0="swap"
flag0=
size0="4096"
algo0=lz4
 
type1="/tmp"
flag1="ext4"
opts1="noatime"
mode1=777
size1="1024"
labl1="zram-tmp"
 
type2=/var/tmp/portage
flag2=ext4
opts2="noatime"
size2=8192
algo2=zstd
owgr2="portage:portage"
mode2=775
labl2=zram-var-tmp-portage
back2=/dev/mapper/zram-nvme0n1p4
icmp2=y
idle2=600
```
This example creates three zram devices:

- A 4GB swap device, using the *lz4* compression algorithm. Do not forget to enable the new swap device with low priority in /etc/fstab.
- A 1GB device mounted at /tmp. It is formatted with the ext4 filesystem with mount options *noatime* and *data=writeback* for optimal performance.
- An 8GB device mounted at /var/tmp/portage with a backing store located at the /dev/mapper/zram-nvme0n1p4 partition for incompressible blocks and for blocks that have not been accessed for 10 minutes. It changes the ownership to user and group *portage*.

Then, add the init script to the desired runlevel, usually boot, and start the service:

`root #````
rc-update add zram-init boot
```
`root #````
rc-service zram-init start
```
In this case the boot runlevel is preferable to the default runlevel because zram is providing temporary storage filesystems at /tmp or /var/tmp which are prerequisites for other services which will start during the default runlevel.

If the config was changed, restart zram-init:

`root #````
rc-service zram-init restart
```
#### systemd

The [sys-block/zram-init](https://packages.gentoo.org/packages/sys-block/zram-init) package provides systemd units with self explained names:

- zram\_btrfs.service
- zram\_swap.service
- zram\_tmp.service
- zram\_var\_tmp.service

These units feature default zram device settings which need to be adjusted first.

##### Service drop in override example

For example, to enable the /var/tmp directory (which includes Portage's temporary directory (/var/tmp/portage) used for compiling packages) in zram it is first required to override the defaults. This can be done using the systemctl edit utility:

`root #``systemctl edit zram_var_tmp.service`
**`/etc/systemd/system/zram_var_tmp.service.d/override.conf`**

**Edited service drop-in**

```
### Editing /etc/systemd/system/zram_var_tmp.service.d/override.conf
### Anything between here and the comment below will become the new contents of the file
 
[Service]
ExecStart=
ExecStart=/sbin/zram-init -d2 -alz4 -text4 -orelatime -m1777 -Lvar_tmp_dir 16384 /var/tmp
```
After this configuration is finalized, the service can be enabled and activated via:

`root #``systemctl enable --now zram_var_tmp.service`
Created symlink /etc/systemd/system/local-fs-pre.target.wants/zram\_var\_tmp.service → /etc/systemd/system/zram\_var\_tmp.service.

##### zram\_var\_tmp\_portage.service example

It appears the .service file examples are no longer included with the zram-init package, therefore this example should aid in systems that would benefit from exclusively compressing the /var/tmp/portage directory.

**`/etc/systemd/system/zram_var_tmp_portage.service`**

**64 GiB of zstd compressed space example**

```
# SPDX-License-Identifier: GPL-2.0-only
[Unit]
Description=Mount /var/tmp/portage as zram
DefaultDependencies=no
Conflicts=umount.target
Before=local-fs.target umount.target
After=local-fs-pre.target
RequiresMountsFor=/var/tmp
 
[Service]
Type=oneshot
RemainAfterExit=true
 
# Adapt the maximal size (in MB) and other options
# Make sure to avoid collision of devices (-d...) with
# zram_swap.service and zram_tmp.service and
# that num_devices in modprobe.d/zram.conf contains the maximal used number + 1
 
# 64GiB of zstd compressed space
ExecStart=/sbin/zram-init -d2 -azstd -text4 -orelatime -m1777 -Lvar_tmp_portage 65536 /var/tmp/portage
ExecStop=/sbin/zram-init -d2 0 /var/tmp/portage
 
[Install]
WantedBy=local-fs-pre.target
```
After this configuration is finalized, the service can be enabled and activated via:

`root #``systemctl enable --now zram_var_tmp_portage.service`
Created symlink /etc/systemd/system/local-fs-pre.target.wants/zram\_var\_tmp\_portage.service → /etc/systemd/system/zram\_var\_tmp\_portage.service.

### Using systemd zram-generator

systemd provides an external package for configuring zram devices. To install it, run:

`root #``emerge --ask sys-apps/zram-generator`
For the simplest zram swap configuration, simply define an empty zram device section in the config:

`root #``echo '[zram0]' > /etc/systemd/zram-generator.conf`
This will configure a zram swap device 4GB in size or half the RAM, whichever is smaller, to start on early boot. To start it immediately, run:

`root #````
systemctl daemon-reload
```
`root #````
systemctl start dev-zram0.swap
```
The configuration directory can also be .d'ed:

`root #````
mkdir /etc/systemd/zram-generator.conf.d
```
`root #````
mv /etc/systemd/zram-generator.conf /etc/systemd/zram-generator.conf.d/zram0-swap.conf
```
For all available settings and the defaults, refer to the man zram-generator.conf.

A more complete example for zram-swap would be:

**`/etc/systemd/zram-generator.conf.d/zram0-swap.conf`**

Note that on systems with up to 8GB RAM, those settings are basically the same as the very simple configuration with just the line `[zram0]`, because in the default setting half of the systems RAM is used, but not more than 4GB. Depending on the kernel configuration, the compression algorithm `zstd` could also be what the system defaults to. For all fs-types, including `swap`, `options` default to `discard`.

For more non-swap configurations, see zram-generator's [README](https://github.com/systemd/zram-generator/blob/main/README.md).

### Using udev

Another possibility is to use existing configuration files - this option works on vanilla Gentoo without need to install additional software, also useful if using systemd instead of OpenRC. The first example can be implemented using:

**`/etc/udev/rules.d/10-zram.rules`**

```
KERNEL=="zram0", SUBSYSTEM=="block", DRIVER=="", ACTION=="add", ATTR{disksize}=="0", ATTR{disksize}="512M", RUN+="/sbin/mkswap $env{DEVNAME}"
KERNEL=="zram1", SUBSYSTEM=="block", DRIVER=="", ACTION=="add", ATTR{disksize}=="0", ATTR{disksize}="512M", RUN+="/sbin/mkswap $env{DEVNAME}"
KERNEL=="zram2", SUBSYSTEM=="block", DRIVER=="", ACTION=="add", ATTR{disksize}=="0", ATTR{disksize}="512M", RUN+="/sbin/mkfs.ext4 $env{DEVNAME}"
# if you want lz4 support (since kernel 3.15) and without ext4 journaling 
# KERNEL=="zram2", SUBSYSTEM=="block", DRIVER=="", ACTION=="add", ATTR{initstate}=="0", ATTR{comp_algorithm}="lz4", ATTR{disksize}="512M", RUN+="/sbin/mkfs.ext4 -O ^has_journal -L $name $env{DEVNAME}"
```
**`/etc/fstab`**

**`/etc/modprobe.d/zram.conf`**

```
options zram num_devices=3
```
#### systemd

If using systemd with this method, ensure that the zram module is loaded by systemd. The simplest way to achieve that is to include a file in /etc/modules-load.d/ like this one:

**`/etc/modules-load.d/zram.conf`**

```
zram
```
## Checking that zram is used

The compact method to check active zram devices is to use zramctl

`user $``zramctl`
NAME       ALGORITHM DISKSIZE DATA COMPR TOTAL STREAMS MOUNTPOINT
/dev/zram0 lz4             2G   4K   64B   20K         \[SWAP\]

## Troubleshooting

### OpenRC: zram already mounted or mount point busy

**Problem:** "\* Mounting local filesystems ... " doesn't wait for mkfs.ext4 to finish, thus zram will fail to mount at boot time.

**Solution:** Set `rc_need="udev-settle"` in /etc/conf.d/localmount.

This situation occurs when using mkfs.ext4 in /etc/udev/rules.d/10-zram.rules. It is caused by mkfs.ext4 being not finished running when /etc/init.d/localmount runs (even with `rc_parallel="NO"` in /etc/rc.conf) and thus causes mount with fail with `mount: /tmp: /dev/zram1 already mounted or mount point busy.`

## See also

- [Portage TMPDIR on tmpfs](https://wiki.gentoo.org/wiki/Portage_TMPDIR_on_tmpfs) — It is unlikely that tmpfs will provide any performance gain for modern systems
- [Zswap](https://wiki.gentoo.org/wiki/Zswap) — a lightweight compressed cache for swap pages.

## External resources

- [zram](https://www.kernel.org/doc/html/latest/admin-guide/blockdev/zram.html) in official kernel documentation.
