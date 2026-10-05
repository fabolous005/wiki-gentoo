<!-- source: https://wiki.gentoo.org/wiki/Udevil | group: Gentoo Wiki (Main) | wiki-title: Udevil -->
---
title: udevil
url: https://wiki.gentoo.org/wiki/Udevil
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-23"
fingerprint: "3860555a9bb71bba"
license: CC BY-SA 4.0
---

# udevil

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**udevil** is a small auto-mount utility created to be a "a hassle-free replacement for [udisks](https://wiki.gentoo.org/wiki/Udisks)."<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> It can be used *with* or *without* [systemd](https://wiki.gentoo.org/wiki/Systemd), ConsoleKit, policykit, [D-Bus](https://wiki.gentoo.org/wiki/D-Bus), udisks, [GVfs](https://wiki.gentoo.org/wiki/GVfs), and [FUSE](https://wiki.gentoo.org/wiki/Filesystem_in_Userspace).

## Installation

### Kernel

Kernel eventpolling may need to be enabled for device media to be properly detected by the kernel:

**Enable eventpolling (CONFIG\_EPOLL)**

After enabling eventpolling confirm operation by running:

`root #````
cat /sys/module/block/parameters/events_dfl_poll_msecs
```
`root #````
cat /sys/block/sr0/events_poll_msecs
```
If either command returns 0 or -1 then there will be issues detecting device media. Create a small script in [/etc/local.d](https://wiki.gentoo.org/wiki//etc/local.d) that will force event polling for each device:

**`/etc/local.d/eventpolling.start`**

**Enable event polling**

```
#!/bin/bash
source /etc/profile
echo 2000 > /sys/module/block/parameters/events_dfl_poll_msecs
echo 2000 > /sys/block/sr0/events_poll_msecs
```
Be sure to make the script executable:

`root #``chmod +x /etc/local.d/eventpolling.start`
### Emerge

Install udevil:

`root #``emerge --ask sys-apps/udevil`
## Configuration

### Global

udevil's operation can be configured using the global configuration file:

- /etc/udevil/udevil.conf

### Local

According to official documentation<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> it is possible to configure auto-mount permissions on an individual basis by creating an /etc/udevil/ configuration file in this following format:

- /etc/udevil/udevil-user-larry.conf

Where `larry` is replaced by the desired user name.

### devmon

A configuration file called devmon is also installed in the /etc.

- /etc/conf.d/devmon

## Usage

### Daemon mode

#### OpenRC

udevil can be configured to operate as a daemon by calling the devmon command. It is possible to run this command in the background by calling it as a job using the ampersand (`&`). Users who belong to the `plugdev` group can add the following line to their \~/.bashrc file, which will start devmon as a daemon each time the system boots:

**`~/.bashrc`**

**Starting devmon in daemon mode**

#### Systemd

To start devmon as a [systemd user service](https://wiki.gentoo.org/wiki/Systemd#User_services):

`root #``systemctl start devmon@larry`
Replace `larry` with the appropriate user name.

### Invocation

`user $``udevil mount <device>``user $``udevil unmount <device>`
## Troubleshooting

To avoid a permission denied error while trying to invoke udevil, ensure the user belongs to the setuid executable's group, which is most likely `plugdev`.

## See also

- [Udev](https://wiki.gentoo.org/wiki/Udev) — [systemd's](https://wiki.gentoo.org/wiki/Systemd) device manager for the Linux kernel.
- [sys-fs/udiskie](https://packages.gentoo.org/packages/sys-fs/udiskie)

## External resources

- [https://igurublog.wordpress.com/downloads/script-devmon/](https://igurublog.wordpress.com/downloads/script-devmon/) - A page describing devmon, an auto-mounting daemon that is now distributed with udevil. This link may be helpful for reference purposes.
