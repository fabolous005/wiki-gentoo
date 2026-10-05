<!-- source: https://wiki.gentoo.org/wiki/Udev/Upgrade_Guide | group: Gentoo Wiki (Main) | wiki-title: Udev/Upgrade Guide -->
---
title: Udev/Upgrade Guide
url: https://wiki.gentoo.org/wiki/Udev/Upgrade_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: b783c13fcd7ab7c1
license: CC BY-SA 4.0
---

# Udev/Upgrade Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Udev** is a central system component and may require extra care on upgrades.

## Example interface IDs

The default naming precedence rules are these:

1. Names incorporating Firmware/BIOS provided index numbers for on-board devices (`ID_NET_NAME_ONBOARD`, example: eno1)
2. Names incorporating Firmware/BIOS provided PCI Express hotplug slot index numbers (`ID_NET_NAME_SLOT`, example: ens1)
3. Names incorporating physical/geographical location of the connector of the hardware (`ID_NET_NAME_PATH`, example: enp2s0)
4. Names incorporating the interfaces's MAC address (`ID_NET_NAME_MAC`, example: enx78e7d1ea46da)
5. Classic, unpredictable kernel-native ethX naming (example: eth0)

(Rule 4 is disabled by default.) So, if output is like this:

`user $``udevadm test-builtin net_id /sys/class/net/eth0 2> /dev/null`
ID\_NET\_NAME\_MAC=enxd4a8xxxxxxxx
ID\_NET\_NAME\_ONBOARD=eno1
ID\_NET\_NAME\_PATH=enp1s0f1

The interface will be named *eno1*, because `ID_NET_NAME_ONBOARD` takes precedence over `ID_NET_NAME_PATH`.

## Things to think of when upgrading

- /etc/conf.d/net
- Renaming init scripts
- Replacing init scripts in runlevels (via `rc-update`)
- iptables configuration
- vnstat configuration
- samba configuration

A quick way to get a list of files that will need to be updated can be found using the following:

`user $``grep -r 'eth0' /etc/* | grep -v '#'`
## Fixing network interfaces

This section provides examples on how to fix network-interfaces if they do not work anymore (or before they stop working before the new udev package has been switched to). It is only applicable to simple setups, like one ethernet and one wifi interface. For more complicated setup (like more ethernet adapters) most users should already know what to do.

### Renaming init scripts

If **still running the old udev** and are ready to enable the new one, then execute:

`user $``udevadm test-builtin net_id /sys/class/net/eth0 2> /dev/null`
ID\_NET\_NAME\_MAC=enxd4a8xxxxxxxx
ID\_NET\_NAME\_PATH=enp1s0f1

Take the ID\_NET\_NAME\_PATH-item (here *enp1s0f1*) and execute

`user $``ln -s net.lo /etc/init.d/net.enp1s0f1`
Do the same for any wifi-interfaces.

When **already running the new udev and the network is down**, execute:

`user $``ls -la /sys/class/net`
total 0
drwxr-xr-x  2 root root 0 Apr  2 19:42 ./
drwxr-xr-x 49 root root 0 Apr  2 19:42 ../
lrwxrwxrwx  1 root root 0 Apr  2 19:42 enp1s0f1 -> ../../devices/pci0000:00/0000:00:1c.1/0000:02:00.0/net/enp1s0f1/
lrwxrwxrwx  1 root root 0 Apr  2 19:42 lo -> ../../devices/virtual/net/lo/
lrwxrwxrwx  1 root root 0 Apr  2 19:42 sit0 -> ../../devices/virtual/net/sit0/

Look which link points to a pci-device (here *enp1s0f1*) and execute:

`user $``ln -s net.lo /etc/init.d/net.enp1s0f1`
When wifi-interfaces exist do not forget include these interfaces as well.

### Replacing init scripts in runlevels (via rc-update)

`user $````
rc-update delete net.eth0 default
```
`user $````
rc-update add net.enp1s0f1 default
```
If necessary do the same for any wifi-interfaces.

## Instruction to upgrade to specific versions

### udev 171 to 204

The following special attention is required:

- `CONFIG_DEVTMPFS=y` must be set in the kernel. To do this Linux kernel 2.6.32+ is needed.

- If there is a /dev line in [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab), it must have the devtmpfs filesystem type or it will conflict with the above options.

- Remove udev-postmount from any runlevels, it is not necessary. Exception: Note that [sys-fs/eudev](https://packages.gentoo.org/packages/sys-fs/eudev) still has the udev-postmount init script in use with USE="rule-generator"

- When using /etc/udev/rules.d/70-persistent-net.rules, read [Predictable Network Interface Names](http://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames),

otherwise delete it together with /etc/udev/rules.d/70-persistent-cd.rules as both are from the now deleted rule\_generator that does not exist anymore.

- The new [Predictable Network Interface Names](http://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames) will be used by default, unless the kernel commandline contains the `net.ifnames=0` parameter. (If the method of masking out the /etc/udev/rules.d/80-net-name-slot.rules file was used in the past be aware it will not work in v209 and newer releases.)

- When using Linux 3.8 or higher it should be possible to disable USE="firmware-loader" and let the kernel do the loading instead. However, USE="firmware-loader" can be used on newer kernels too.

- [sys-apps/biosdevname](https://packages.gentoo.org/packages/sys-apps/biosdevname) is no longer necessary if the [Predictable Network Interface Names](http://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames) are being used.

- Any file that has not been personally created or edited in /etc/udev/rules.d should be backed up and removed since it is likely a relic from old an udev installation.

For more details, read the full news article /usr/portage/metadata/news/2013-03-29-udev-upgrade/2013-03-29-udev-upgrade.en.txt and the post install output of emerging udev.

### udev 204 to 208

The following special attention is required:

- Since this version, kernel with accept4() function support is mandatory. It is supported for **amd64**, **ia64**, **mips**, **sparc** and **x86** in 2.6.32.61. All others, like ARM, will need a newer kernel like 3.0 for the function.
- It is no longer the job of the /dev manager, like udev, to create /dev nodes for static kernel modules such as /dev/fuse or /dev/snd/seq. Instead they are now created by the kmod-static-nodes init script which is part of [sys-apps/kmod](https://packages.gentoo.org/packages/sys-apps/kmod) since version 15. Make sure the init script has been added to the "sysinit" runlevel.

### udev 208 to 216

The following special attention is required:

- Since this version, kernel settings `CONFIG_FHANDLE=y` and `CONFIG_NET=y` are mandatory. Kernel setting `CONFIG_DMIID=y` is recommended for **amd64**/**ia64**/**x86**, for example, keyboard rules.
- File /lib/udev/rules.d/80-net-name-slot.rules was replaced with /lib/udev/rules.d/80-net-setup-link.rules. If currently using an empty (or single-comment) /etc/udev/rules.d/80-net-name-slot.rules to disable predictable network interface names, the 80-net-setup-link.rules should now be used. For example:

`root #``cd /etc/udev/rules.d/ && ln 80-net-name-slot.rules 80-net-setup-link.rules`
This keeps the override both pre- and post-upgrade; then run:

`root #``rm /etc/udev/rules.d/80-net-name-slot.rules`
once the upgrade has been made. The hardlink can be made, in order to protect against not noticing the upgrade in a busy or non-professional situation.

- However, 80-net-setup-link.rules is only a trigger for the actual configuration file 99-default.link at /lib/systemd/network/ which can be overrided at /etc/systemd/network/
- The most reliable way of disabling the new network interface scheme is still the kernel bootline parameter: `net.ifnames=0`

### udev 216 to 217

The following special attention is required:

- Since this version, kernel setting `CONFIG_FW_LOADER_USER_HELPER` should be set to `=n`, because the userspace firmware loader has been removed. Upgrade the kernel at least to version 3.7 for the kernel firmware loader.
- If `CONFIG_FW_LOADER_USER_HELPER` cannot be disabled, try to disable `CONFIG_DELL_RBU`, it will select `CONFIG_FW_LOADER_USER_HELPER` automatically in Linux kernel 3.18.x.
