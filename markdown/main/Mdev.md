<!-- source: https://wiki.gentoo.org/wiki/Mdev | group: Gentoo Wiki (Main) | wiki-title: Mdev -->
---
title: mdev
url: https://wiki.gentoo.org/wiki/Mdev
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-10"
fingerprint: "9f9bb15b8597b9a0"
license: CC BY-SA 4.0
---

# mdev

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article documents how to replace udev in Linux with mdev, thus allowing a separate /usr partition, without an [initramfs](https://wiki.gentoo.org/wiki/Initramfs). The author uses Gentoo Linux with IceWM as the window manager. The instructions here should be, with some small adjustments, applicable to other distributions.

**mdev** is a udev replacement from [Busybox](https://wiki.gentoo.org/wiki/Busybox), it populates and updates /dev ([official docs](https://git.busybox.net/busybox/plain/docs/mdev.txt)). Replacing [eudev](https://wiki.gentoo.org/wiki/Eudev) or [udev](https://wiki.gentoo.org/wiki/Udev) is non-trivial, and is probably most adapted to embedded systems.

## Will mdev work on my system?

The mdev application is definitely suitable as long as the system does not use a full-fledged desktop environment. Note that a desktop environment is not required to run AbiWord, Firefox, GIMP, Gnumeric, etc. However, KOffice applications like KMail seem to pull in most of KDE as a dependency. In general, when using KDE or GNOME, mdev is not suitable. Also using [LVM](https://wiki.gentoo.org/wiki/LVM) might be troublesome.

It will work very well when using the default Gentoo profiles:

- *default/linux/x86/13.0*
- *default/linux/amd64/13.0*

List the currently used profile by using following command:

`user $``eselect profile list`
Current /etc/portage/make.profile symlink:
  default/linux/amd64/13.0

There is one more sanity check that Gentoo users can run to check for udev dependency.

### Sanity check

Get a general overview which packages might depend on udev. The output could look similar to the one shown below:

`user $``equery d udev`
\* These packages depend on udev:
media-libs/mesa-9.0.1 (gbm ? virtual/udev)
sys-apps/hwids-20130329 (udev ? >=virtual/udev-197-r1)
sys-apps/util-linux-2.22.2 (udev ? virtual/udev)
virtual/dev-manager-0 (virtual/udev)
x11-base/xorg-server-1.13.4 (udev ? >=virtual/udev-150)
x11-drivers/xf86-video-intel-2.20.13 (udev ? virtual/udev)
x11-libs/cairo-1.10.2-r3 (drm ? >=virtual/udev-136)

Add the following line to the /etc/portage/package.mask or /etc/portage/package.mask/mdev file (which can be created if it does not exist yet):

**`/etc/portage/package.mask/mdev`**

**Mask away udev**

Disable the `udev` USE flag globally in /etc/portage/make.conf:

`root #``euse -D udev`
Rebuild all packages with the new `-udev` USE flag:

`root #``emerge -uDNvp @world`
If the only error that comes up is that Portage is not able to re-install udev as required by [virtual/dev-manager](https://packages.gentoo.org/packages/virtual/dev-manager), proceed to the next stage. Otherwise, [sys-fs/udev](https://packages.gentoo.org/packages/sys-fs/udev) might be an hard dependency of one or more packages installed on the system.

## Replacing udev with mdev

### Setting up the kernel for devtmpfs

Set up the kernel to support and automount a devtmpfs [filesystem](https://wiki.gentoo.org/wiki/Filesystem) at /dev:

**Enabling devtmpfs**

Once the changes have been made, rebuild the kernel. Do **NOT** reboot yet.

### Emerging busybox

Make sure that the `mdev` USE flag is set for [sys-apps/busybox](https://packages.gentoo.org/packages/sys-apps/busybox). The `static` USE flag is probably also a good idea. In the /etc/portage/package.use or /etc/portage/package.use/mdev file, add the following line:

**`/etc/portage/package.use/mdev`**

Now, (re)install busybox:

`root #``emerge --ask --oneshot busybox`
### Mounting devpts

The devpts filesystem exhibits non-standard behavior. It does not automount, at bootup, or with mount -a. An explicit mount devpts command is required. The standard solution for udev-based systems is to run rc-update add udev-mount sysinit as root, and have the udev-mount script do the mounting at startup. An mdev-based system will probably not have udev installed. An alternative way to do this at bootup is to include the command mount devpts in a shell script in /etc/local.d/. In this example, the file will be named /etc/local.d/000.start.

Another side-effect of not using udev-mount is that /dev/shm is only writable by root. The command chmod 1777 /dev/shm is required to restore the standard behavior. This command will also be run from the script at startup.

**`/etc/local.d/000.start`**

**Mount of devpts at bootup and change /dev/shm permissions**

```
#!/bin/bash
mount devpts
chmod 1777 /dev/shm
```
### Replace the udev service

Remove udev from the services list and replace it with mdev:

`root #````
rc-update del udev sysinit
```
`root #````
rc-update add mdev sysinit
```
### Reboot

Reboot to the new kernel. The system should now be running using mdev.

### Cleanup

Remove udev from the system:

`root #``emerge --ask --depclean --verbose sys-fs/udev sys-fs/eudev`
In the /etc/portage/package.mask file (or a file in this directory), add [sys-fs/udev](https://packages.gentoo.org/packages/sys-fs/udev) and [sys-fs/eudev](https://packages.gentoo.org/packages/sys-fs/eudev).

This now results in a completely udev-free machine.

## Setting up a USB printer running under CUPS

### Make 'find' available at early boot-time

`root #````
cd /bin
```
`root #````
ln -s /bin/busybox find
```
### Rebuild the kernel

For libusb (and hence CUPS) to see the USB ports, set `CONFIG_USB_DEVICE_CLASS=y` and `CONFIG_USB_DEVICEFS=y` in the kernel configuration.  Also enable `CONFIG_USB_PRINTER=y`, despite the injunction in the cups-1.5.2 ebuild to disable it.  When using make menuconfig, these items are found here:

**Enabling generation of USB devices at boot time**

### Configure the new kernel into the bootloader and reboot into it

### Verify the presence of the new device nodes

A hierarchy of device nodes should be present under /dev/bus/usb/. When switching on the printer, a node for it should occur at /dev/lp0. lsusb should run successfully, and this should display the printer's details.

### Configure the printer in CUPS

Configure CUPS in the [usual way](https://wiki.gentoo.org/wiki/Printing). CUPS should find the printer, and will give it a URI like parallel:/dev/lp0.

## Troubleshooting

### Linux modules missing at boot sequence

If Linux modules are required and not loaded on boot after migration, execute one or more of the following solutions:

- Add Linux modules to `*.conf` files in /etc/modules-load.d/

- Compile modules into the Linux kernel, without the need of loading them on demand.

- Write a custom module loader:

- FILE**`/sbin/hotplug`** #!/bin/sh test -n "$MODALIAS" && modprobe "$MODALIAS"; exec /sbin/mdev

Install it as the hotplug handler instead of mdev. Note that the earlier this gets executed, the better. Consider naming this script /sbin/hotplug, since that is the default value of /proc/sys/kernel/hotplug (or change the default value in the kernel configuration).

## Miscellaneous

- mdev unlike udev does not support auto-modules loading. Create files ending with `.conf` in /etc/modules-load.d/ and put all the modules there that should be loaded (nvidia, wl, etc.) one per line. Customize options via files ending with `.conf` in /etc/modprobe.d (see man 5 modprobe.d for syntax). It might be necessary to move the module configuration to this location.

- mdev -s does not create /dev/mapper nodes. Either manually create them or use dmsetup mknodes from lvm2. It is a good idea to add it after mdev -s in the init script.

- Use mouse and keyboard drivers for xorg inputs. Evdev needs udev to be built. Mousedrv (for the mouse driver) may conflict with the synaptic driver when both are loaded.

- The Kernel configuration option `CONFIG_INPUT_EVDEV` not only provides the keyboard and mouse as input device events, it will provide lid and button events to acpid as well.

## See also

- [Mdev/Automount USB](https://wiki.gentoo.org/wiki/Mdev/Automount_USB) — describes how to implement automounting of USB devices on a machine using mdev as the device manager.

## External resources

- [mdev like a boss](https://github.com/slashbeast/mdev-like-a-boss) project.
