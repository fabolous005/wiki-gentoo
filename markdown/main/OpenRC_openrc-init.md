<!-- source: https://wiki.gentoo.org/wiki/OpenRC/openrc-init | group: Gentoo Wiki (Main) | wiki-title: OpenRC/openrc-init -->
---
title: OpenRC/openrc-init
url: https://wiki.gentoo.org/wiki/OpenRC/openrc-init
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-12"
fingerprint: "5e1c417f01212b81"
license: CC BY-SA 4.0
---

# OpenRC/openrc-init

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A UNIX or Linux operating system always has an init process which is the first process to start after the kernel loads. It is the direct or indirect parent of all processes in the system. Daemon processes often double-fork, to make themselves direct child processes of the init process. openrc-init is OpenRC's own init system, and is available since version 0.25. It can replace Gentoo's current default, [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit)'s init.

## Kernel command line

Because the init process is started by the linux kernel, the kernel will need to be told to start the alternative openrc-init process. This is done by passing `init=/sbin/openrc-init` on the linux kernel command line at boot time. This description refers to [GRUB](https://wiki.gentoo.org/wiki/GRUB); adapt as needed in case a different boot manager is used.

Update /etc/default/grub as follows:

**`/etc/default/grub`**

```
#GRUB_CMDLINE_LINUX_DEFAULT=""
GRUB_CMDLINE_LINUX_DEFAULT="init=/sbin/openrc-init"
```
Regenerate /boot/grub/grub.cfg:

`root #````
mount /boot                # if not mounted already
```
`root #``grub-mkconfig -o /boot/grub/grub.cfg` ## Start terminals as OpenRC services

openrc-init does not use /etc/inittab, so the agetty processes for `tty1` to `tty6` will need to be started explicitly as services.

Create the links in /etc/init.d and add them to the default runlevel:

`root #````
cd /etc/init.d 
```
`root #````
for n in $(seq 1 6); do ln -s agetty agetty.tty$n; rc-update add agetty.tty$n default; done 
```
## Optional: add custom terminals

In case the system has one or more terminals connected via a serial line, then it would be necessary to define a custom agetty configuration, e.g. to specify the baudrate:

`root #``cp /etc/conf.d/agetty /etc/conf.d/agetty.ttyS0`
**`/etc/conf.d/agetty.ttyS0`**

Consult /etc/init.d/agetty for the logic.

Create a symlink to /etc/init.d/agetty, add it to the default runlevel, and reboot:

`root #````
ln -s /etc/init.d/agetty /etc/init.d/agetty.ttyS0
```
`root #````
rc-update add agetty.ttyS0 default
```
## Reboot into openrc-init

`root #``reboot`
Once rebooted the system is running under openrc-init. Verify as follows:

`root #``ps -ef | grep openrc-init`
root         1     0  0 Jul02 ?        00:00:01 /sbin/openrc-init

Process number one is now openrc-init. Commands like reboot, and shutdown are no longer working under openrc-init. Instead there is openrc-shutdown. Check its [man page](http://manpages.debian.org/testing/openrc/openrc-shutdown.8.en.html) for its usage.

## Optional: enable the sysv-utils USE flag

To use commands like reboot and shutdown instead of openrc-shutdown, [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) can be re-emerged with the [sysv-utils](https://packages.gentoo.org/useflags/sysv-utils) [use flag enabled (it is fine if the deselect operation does not find any package):](https://wiki.gentoo.org/wiki/USE_flag)

`root #````
echo "sys-apps/openrc sysv-utils" >> /etc/portage/package.use/openrc
```
`root #````
emerge --deselect sys-apps/sysvinit
```
`root #``emerge --ask --oneshot --newuse --verbose sys-apps/openrc`
Now the familiar commands halt, reboot and the like should work as per normal.

## Optional: remove openrc-init, revert back to sysv-init

In case openrc-init is not working properly, or for any other reason it is desired to revert back to [sys-apps/sysvinit](https://packages.gentoo.org/packages/sys-apps/sysvinit), then that is possible by executing the above steps in reverse order. Please do not forget to log a bug in case openrc-init didn't work.

If the [sysv-utils](https://packages.gentoo.org/useflags/sysv-utils) [USE flag is set, remove it and make sure](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/sysvinit](https://packages.gentoo.org/packages/sys-apps/sysvinit) is installed again:

`root #````
rm /etc/portage/package.use/openrc
```
`root #````
echo "sys-apps/openrc -sysv-utils" >> /etc/portage/package.use/openrc
```
`root #``emerge -a1N sys-apps/openrc sys-apps/sysvinit`
Remove `init=/sbin/openrc-init` from the kernel command line, and reboot:

`root #``openrc-shutdown -r now`
