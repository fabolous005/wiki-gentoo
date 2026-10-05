<!-- source: https://wiki.gentoo.org/wiki/Udev | group: Gentoo Wiki (Main) | wiki-title: Udev -->
---
title: udev
url: https://wiki.gentoo.org/wiki/Udev
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-19"
fingerprint: be89881ee1c9bbc0
license: CC BY-SA 4.0
---

# udev

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**udev** (user /dev) is [systemd's](https://wiki.gentoo.org/wiki/Systemd) device manager for the Linux kernel. It manages device nodes in /dev and handles all user space actions when adding or removing devices.

udev from the [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils) package is used as the default device manager for Gentoo systems using the [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) init system, independently of systemd.

Most Linux users understand that /dev/sda1 is just a fast way of referring to the first partition on the first disk that the kernel found. That's pretty easy, right?

But consider hotpluggable devices like USB, IEEE 1394, hot-swappable PCI, etc. What is the first device for each of these? And for how long? What will the other devices be named when the first one disappears? How will that affect ongoing transactions? Wouldn't it be fun if a printing job were suddenly moved from a high-end laser printer to an almost-dead matrix printer just because someone decided to pull the plug on the laser printer (which just happened to be the first printer)?

Enter the device manager. A modern device manager must:

- Run in userspace.
- Dynamically create and remove [device files](https://wiki.gentoo.org/wiki/Device_file).
- Provide consistent device naming.
- Provide a userspace application program interface (API).

Every time a change happens within the device structure, the kernel emits a *uevent* which gets picked up by the device manager. The device manager then follows the rules declared in the /etc/udev/rules.d, /run/udev/rules.d and /lib/udev/rules.d directories. Based on the information contained within the uevent, it finds the rule or rules it needs to trigger and performs the required actions. These actions may involve the creation or deletion of device files, and may also trigger the loading of particular firmware files into kernel memory.



udev requires the following kernel options:

Portage knows the [`udev`](https://packages.gentoo.org/useflags/udev) USE flag for enabling support for udev in other packages. Adding this USE flag value to the USE flag list (default in all Linux [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>)) will pull in the [virtual/udev](https://packages.gentoo.org/packages/virtual/udev) package automatically:

**`/etc/portage/make.conf`**

```
USE="udev"
```

| [+acl](https://packages.gentoo.org/useflags/+acl) | Add support for Access Control Lists | 
| [+kmod](https://packages.gentoo.org/useflags/+kmod) | Enable kernel module loading via sys-apps/kmod | 
| [+sysctl](https://packages.gentoo.org/useflags/+sysctl) | Install systemd-sysctl and sysctl.d files | 
| [+tmpfiles](https://packages.gentoo.org/useflags/+tmpfiles) | Enable systemd-tmpfiles | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable systemd-udev (userspace device manager) | 
| [boot](https://packages.gentoo.org/useflags/boot) | Enable systemd-boot (UEFI boot manager) | 
| [kernel-install](https://packages.gentoo.org/useflags/kernel-install) | Enable kernel-install | 
| [secureboot](https://packages.gentoo.org/useflags/secureboot) | Automatically sign efi executables using user specified key | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [split-usr](https://packages.gentoo.org/useflags/split-usr) | Enable behavior to support maintaining /bin, /lib\*, /sbin and /usr/sbin separately from /usr/bin and /usr/lib\* | 
| [sysusers](https://packages.gentoo.org/useflags/sysusers) | Enable systemd-sysusers | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [ukify](https://packages.gentoo.org/useflags/ukify) | Enable systemd-ukify | 

After setting USE flags update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
To start udev at boot time, add it to the sysinit runlevel. This can be done by issuing the following commands with root privileges:

`root #````
rc-update add udev sysinit
```
`root #````
rc-update add udev-trigger sysinit
```
udev provides a set of rules that match against exported values of uevents (events sent by the kernel) and properties of the discovered device. A matching rule will possibly name and create a device node and run configured programs to setup and configure the device.

The rule definitions are stored in two locations:

1. **/lib/udev/rules.d/** - Rules in this directory are installed by certain packages, they generally should not be changed by users;
2. **/etc/udev/rules.d/** - This folder is for end-user specified rules. Any new rules should be added in this directory;

In these directories, multiple rule files (with suffix .rules) are traversed in alphanumerical order. Inside the rules files, udev will find expressions that might match a uevent together with the state to match (is the uevent because a device is added or removed) and the command to execute.

The event matching is based on information such as:

- The *SUBSYSTEM* of the uevent (for which type of device is the uevent fired);
- The *ACTION* that is taken (add, change, or remove);
- One or more attributes (through *ATTR* or *ATTRS*), such as the device class, vendor or other device information;
- The kernel-provided name (through *KERNEL*), such as sd\* (for SCSI/SATA disks) or input\* (for input devices such as mice and keyboards);
- One or more environment settings (through *ENV*), used to send information between multiple rules.

Based on this information, the rule can then state if:

1. Some information needs to be shared with later events (through environment variables)
2. Links need to be created in /dev
3. Commands need to be executed

Udev does this for every rule that matches (it does not stop after the first match) to allow a flexible device management approach.

The kernel detects devices asynchronously, udev mirrors the kernel's [sysfs](https://wiki.gentoo.org/wiki/Sysfs) filesystem and so the devices are named and numbered in order of detection. So by default udev provides no persistent device names. However there are mechanisms for some device classes to provide these:

- Udev creates for storage devices additional symlinks based on the device's ID, [label, UUID](https://wiki.gentoo.org/wiki/Removable_media#UUIDs_and_labels) and path. See the /dev/disk/by-\* directory. So instead of using e.g. the device file /dev/sda use the file /dev/disk/by-label/SOME\_LABEL.

- The same for input devices in the /dev/input directory.

- Using custom rules enables users to create their own device files.

The new network interface naming convention is not the same. So the [symlinks](https://wiki.gentoo.org/wiki/Handbook:AMD64/Networking/Introduction) used by [netifrc](https://wiki.gentoo.org/wiki/Netifrc) will need to be re-linked. Use /etc/init.d/net.lo as a link target for whatever interface names need to be added. Be sure to replace *`<interface_name>`* in the commands below with the Ethernet interface names present on the system. It is possible to discover which interfaces exist by running the ip link command:

`user $``ip link`
Create a symbolic link for the existing interfaces in the /etc/init.d/ directory:

`root #``ln -s /etc/init.d/net.lo /etc/init.d/net.`*<interface_name>*
Edit /etc/conf.d/net with any necessary configuration for all interfaces.

Add the script(s) to the default runlevel to have the interface(s) start automatically:

`root #``rc-update add net.`*<interface_name>* default
Network device names such as `eth0` or `wlan0` as provided by the kernel are normally changed on system boot (see dmesg) by the /lib/udev/rules.d/80-net-setup-link.rules udev rule and the NamePolicy in /lib/systemd/network/99-default.link.

This behavior may be disabled in several ways:

- Symlink /etc/systemd/network/99-default.link to /dev/null: ln -s /dev/null /etc/systemd/network/99-default.link.
- Create a lower-numbered .link file in /etc/systemd/network which assigns a different name to the interface.
- Pass `net.ifnames=0` on the kernel command line.

Reference: [https://systemd.io/PREDICTABLE\_INTERFACE\_NAMES/](https://systemd.io/PREDICTABLE_INTERFACE_NAMES/)

udev provides a way to remap keys and buttons using *hwdb*:

**`/etc/udev/hwdb.d/99-keyboard-example.hwdb`**

The device name can be found using [app-misc/evtest](https://packages.gentoo.org/packages/app-misc/evtest):

`root #``evtest`
/dev/input/event3:	AT Translated Set 2 keyboard

After selecting the device (evtest is an interactive tool) and pressing a key, evtest displays information about the key. In this example, the scancode of the pressed key is `0xDB` and it mimics the key with keycode `58`:

Event: time 1708679751.175244, type 4 (EV\_MSC), code 4 (MSC\_SCAN), value db
Event: time 1708679751.175244, type 1 (EV\_KEY), code 58 (KEY\_CAPSLOCK), value 1

The full list of keycodes is available in /usr/include/linux/input-event-codes.h or in the
[Linux source code](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/include/uapi/linux/input-event-codes.h). As an example, `#define KEY_CAPSLOCK 58` transforms into `58`.

The keycodes of other devices are also allowed (e.g., the left click mouse button `0x110`).

To perform the changes, the hardware database /etc/udev/hwdb.bin must be updated (also suitable for OpenRC):

`root #``systemd-hwdb update`
The new configuration can be applied by executing the following command:

`root #``udevadm trigger --verbose --sysname-match=event3`
/sys/devices/pcixxxx:xx/xxxx:xx:xx.0/usb1/1-2/1-2.1/1-2.1:1.0/xxxx:xxxx:xxxx.xxxx/input/input12/event3

Or by rebooting the system.

The device configuration can be verified like so:

`root #``udevadm info /dev/input/by-path/*usb*-kbd | grep "KEYBOARD_KEY"`
E: KEYBOARD\_KEY\_db=58

Some useful commands are:

- Monitor all udev activities:

- `root #``udevadm monitor`

- Show all messages about a given device file:

- `root #``udevadm info --query=all --name=/dev/DEVICE_FILE`

- Show udev info about a given [sys path](https://wiki.gentoo.org/wiki/Sysfs) device file (might be obtained via udevadm monitor):

- `root #``udevadm info --attribute-walk --path=/devices/DEVICE_FILE`

- Udev rules file example - assign a persistent name to an Ethernet device:

- FILE**`/etc/udev/rules.d/50-ethernet.rules`** SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="01:23:45:67:89:ab", NAME="ethernet0"

- After changing a rules file in /etc/udev/rules.d/, either reboot or make udev reload them with:

- `root #``udevadm control --reload-rules`

- For already added devices (plugged USB, etc.), the reloaded rules take effect either after changing the device (replugging USB, etc.) or by making udev request kernel events again via udevadm trigger. Various event actions can be triggered. By default, the *change* event action is triggered:

- `root #``udevadm trigger`

See the [udevadm(8)](https://man.archlinux.org/man/udevadm.8.en) [man page](https://wiki.gentoo.org/wiki/Man_page) for more information.

To log all message when udevadm monitor is run, modify the following configuration file:

**`/etc/conf.d/udev`**

```
udev_monitor="YES"
```
It will create the new log file located at /run/udev/udevmonitor.log.

Enabling debug mode will output more log messages:

**`/etc/conf.d/udev`**

```
udev_debug="YES"
```
Set the logging priority:

**`/etc/udev/udev.conf`**

```
udev_log="debug"
```
The log file /run/udev/debug.log will be created but no messages will be logged to it. The most recent versions of udev will log all messages to dmesg.

### /dev/null and /dev/console

Some udev versions need the /dev/null and /dev/console files in order to work properly, but cannot create them on their own. To manually create these files for udev run the following commands with root privileges:

`root #````
mkdir test
```
`root #````
mount --bind / test
```
`root #````
cd test/dev
```
`root #````
mknod -m 660 console c 5 1
```
`root #````
mknod -m 660 null c 1 3
```
`root #````
cd ../..
```
`root #````
umount test
```
`root #````
rmdir test
```
Those having dual network cards on their motherboards may run into a situation where ip link may show no eth0 or eth1. dmesg may show their NIC detected as eth0, and later moved to eth1. Performing a ip link will also show the NIC as eth1. This is caused by using the kernel assigned names in the first place. Users should write custom rules like /etc/udev/rules.d/70-my-network.rules to use free names like lan0 or wireless0 or use predictable interface names.

Remember to also remove old files from old versions of udev:

`root #``rm /etc/udev/rules.d/70-persistent-net.rules /etc/udev/rules.d/80-net-setup-link.rules /etc/udev/rules.d/80-net-name-slot.rules /etc/systemd/network/99-default.link`
Also make sure not to pass `net.ifnames=0` on the kernel commandline. This setting would disable the predictable interface names feature of udev altogether.

On some hardware, a device may not be available as soon as it's required by a different service script. This can happen for example when using [Netifrc](https://wiki.gentoo.org/wiki/Netifrc), which requires the network interface to be available. Since [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) provides no method of waiting for the interface to be available, it will sometimes loudly fail with an `ERROR: interface wlan0 does not exist`, introducing a race condition.

To make sure all devices are available, start the `udev-settle` service during `sysinit`. This will slow down your bootup, but provide more consistency, especially if `rc_parallel` is enabled:

`root #``rc-update add udev-settle sysinit`
- [Eudev](https://wiki.gentoo.org/wiki/Eudev) — a fork of [udev], [systemd](https://wiki.gentoo.org/wiki/Systemd)'s [device file](https://wiki.gentoo.org/wiki/Device_file) manager for the Linux kernel.

- [Bug 575718 - Request for council decision regarding virtual/udev default provider](https://bugs.gentoo.org/show_bug.cgi?id=575718) - Default [virtual/udev](https://packages.gentoo.org/packages/virtual/udev) provider changed to [sys-fs/eudev](https://packages.gentoo.org/packages/sys-fs/eudev)
