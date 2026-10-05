<!-- source: https://wiki.gentoo.org/wiki/Suspend_and_hibernate | group: Gentoo Wiki (Main) | wiki-title: Suspend and hibernate -->
---
title: Suspend and hibernate
url: https://wiki.gentoo.org/wiki/Suspend_and_hibernate
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-07"
fingerprint: ad819bc5247677ee
license: CC BY-SA 4.0
---

# Suspend and hibernate

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article describes how to suspend or hibernate a Gentoo system.

## Installation

### Kernel

Make sure support for suspend and hibernation has been activated (`CONFIG_SUSPEND`) and (`CONFIG_HIBERNATION`):

### Software

One of the following packages can be used to control the in-kernel default suspend/hibernate implementation, namely, *[swsusp](https://en.wikipedia.org/wiki/swsusp)*.

- [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) provides the following commands that can be launched as root or from a user account. Many [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) already require it if [systemd](https://wiki.gentoo.org/wiki/Systemd) is not used instead. Make sure it is [configured properly](https://wiki.gentoo.org/wiki/Elogind).
  - loginctl suspend
  - loginctl hibernate
  - loginctl hybrid-sleep
  - loginctl suspend-then-hibernate
- [sys-power/suspend](https://packages.gentoo.org/packages/sys-power/suspend) provides:
  - s2ram
  - s2disk
  - s2both
- [sys-power/hibernate-script](https://packages.gentoo.org/packages/sys-power/hibernate-script)

## Available suspend modes

To see available suspend modes use

`root #``cat /sys/power/state`
freeze mem disk

for swsusp, default implementation.

Those two file will list at least [ACPI](https://wiki.gentoo.org/wiki/ACPI) S2/4 power down methods on modern hardware.
New hardware would also support S5 method which is a rough S4 method.
ACPI S2 correspond to suspend to ram (*ram* method in swsusp terms and *3* in ToI terms);
S4 hibernation to disk (*disk* in swsusp terms and *4* in ToI terms; S5 hibernation to disk (*5* in ToI terms).

Swsusp users can choose between *platform*, meaning ACPI, or *shutdown* methods which can be echo-ed to /sys/power/disk sysfs file.

## Suspend to Idle

On modern hardware, traditional S3 suspend is being replaced by a set of fine-grained runtime power management capabilities for the S0 sleep state. This is referred to as S0ix by Intel and Modern Standby by Microsoft. To check available standby modes use

`root #``cat /sys/power/mem_sleep`
\[s2idle\] shallow

For S0ix to work, s2idle must be active.

## Suspend to RAM

Preferred commands to suspend are:

`root #``s2ram`
or, if using [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind):

`root #``loginctl suspend`
See settings at **/etc/elogind/sleep.conf**, in \[Sleep\] section **SuspendMode** must be **deep** if you want to disable fan noise on sleep.

For suspend (to RAM) for [sys-power/hibernate-script](https://packages.gentoo.org/packages/sys-power/hibernate-script) users:

`root #``hibernate-ram`
or

`root #``hibernate`
to hibernate (to disk).

A more "raw" method to directly communicate with the kernel is:

`root #``echo mem > /sys/power/state`
## Suspend to disk

For suspend to disk to operate a swap partition or swap file must exist.

The swap file should be active beforehand and should be echoed on the appropriate file before any attempt to suspend/hibernate.

`root #``echo /dev/sda1 > /sys/power/resume`
A more "raw" method is to:

`root #``echo disk > /sys/power/state`


### Suspend to disk and reboot afterwards

Let's say you just want to save your current session and boot into another OS, it is not necessary to do a regular hibernation including shutdown. It is sufficient to just create the hibernate image (within swap or a swap file) and reboot afterwards:

`root #``echo reboot > /sys/power/disk``root #``echo disk > /sys/power/state`
If this didn't work, please check the available options on your system (and [debugging hibernation and suspend](https://docs.kernel.org/power/basic-pm-debugging.html) on kernel.org). When reboot is available, after echo-ing it you will see something like this (the active/chosen option is within brackets):

`root #``cat /sys/power/disk`
platform shutdown \[reboot\] suspend test\_resume

Further information can be found within the documentation on [kernel.org](https://www.kernel.org/doc/Documentation/power/interface.txt) for /sys/power/disk  resp. /sys/power/state sysfs file.

### Suspend to disk with sys-auth/elogind

First, make sure a swap partition has been set, grub.cfg rebuilt and the [initramfs](https://wiki.gentoo.org/wiki/Initramfs) (if any) updated as shown above.

Reboot the system:

`root #``loginctl reboot`
Next, try running:

`root #``loginctl hibernate`
### Suspend to disk with swap file

You can use suspend to disk with a swap file. When you have a functional swap file you need to configure [kernel](https://wiki.gentoo.org/wiki/Kernel) parameters (via [GRUB](https://wiki.gentoo.org/wiki/GRUB), etc.).

First find UUID of device where your swap file resides. For example /dev/sda1.

`root #``blkid /dev/sda1`
Find offset of swap file on given partition using the swap-offset utility from [sys-power/suspend](https://packages.gentoo.org/packages/sys-power/suspend):

`root #``swap-offset /path/to/swapfile`
After that edit GRUB config and add required parameters to the boot string:

**`/etc/default/grub`**

**GRUB defaults**

Rebuild GRUB config:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Reboot the system and check used kernel parameters:

`user $``cat /proc/cmdline`
It should now be possible to hibernate the system.

## Troubleshooting

Classic kernel buffer comes handy:

`user $``dmesg`
### Can not resume after suspend

#### Buggy microcode

Try disabling the security chip setting in BIOS/UEFI and try again. Outdated [microcode](https://wiki.gentoo.org/wiki/Microcode) can result in dysfunction of resumption from suspension, thus make sure it is updated (eg. [Intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode) with i915 drivers).

For i915 drivers if the microcode update is ineffective, try disabling `CONFIG_RETPOLINE` at the cost of [Spectre v2 vulnerability](<https://en.wikipedia.org/wiki/Spectre_(security_vulnerability)>).

#### Dracut configured without resume module

If using [sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut) for the creation of the initramfs, be sure the resume module is included in the image. For example, the configuration file could contain:

**`/etc/dracut.conf`**

### WiFi stays hard blocked

Although possibly *unsafe*, tricking the BIOS into believing it being "Microsoft Windows" might solve it.

This can be done by adding `acpi_osi=! acpi_osi=Windows` or `acpi_osi=! acpi_osi='Windows 2009'`[(kernel source)](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/drivers/acpi/acpica/utosi.c) to the boot command line options.

For [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub), the options can be appended to `GRUB_CMDLINE_LINUX` in /etc/default/grub.

### Migration from pm-utils to elogind

Copy any suspend/resume and hibernate/thaw hook scripts from the directory /etc/pm/sleep.d/ to /lib64/elogind/system-sleep/, and modify them to cater for the new $1 ('pre' or 'post') and $2 ('suspend', 'hibernate', or 'hybrid-sleep'). See also: [Elogind#Suspend.2FHibernate\_Resume.2FThaw\_hook\_scripts](https://wiki.gentoo.org/wiki/Elogind#Suspend.2FHibernate_Resume.2FThaw_hook_scripts)

### High Battery Drain in S2idle

On many systems, it is necessary to override certain power management defaults to achieve S0ix properly. To test and troubleshoot such problems, Intel's [S0ixSelftestTool](https://github.com/intel/S0ixSelftestTool) is recommended.

### Long delay before suspend

Systemd versions from v256 and later will attempt to freeze the user session before suspending. If QEMU is running, Systemd might fail to freeze it and time out after 60 seconds before suspending.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> This can be identified by the \`Failed to freeze unit 'user.slice'\` message in the output of `journalctl -b`.

To work around this, Systemd's freezing can be disabled (without disabling the kernel's freezing) by [customizing](https://wiki.gentoo.org/wiki/Systemd#Customizing_unit_files) systemd-suspend.service with the following configuration:

**`/etc/systemd/system/systemd-suspend.service.d`**

```
[Service]
Environment="SYSTEMD_SLEEP_FREEZE_USER_SESSIONS=false"
```
## See also

- [Power management/Guide](https://wiki.gentoo.org/wiki/Power_management/Guide) — a guide to setup power management features of a laptop.
- [Custom Initramfs/Hibernation](https://wiki.gentoo.org/wiki/Custom_Initramfs/Hibernation) — describes how to enable hibernation with a custom initramfs.

## External resources

- [Suspend and hibernate](https://wiki.archlinux.org/index.php/Power_management/Suspend%20and%20hibernate) on wiki.archlinux.org
- [Linux kernel documentation - swsusp.txt](https://www.kernel.org/doc/Documentation/power/swsusp.txt), or the usual location of /usr/src/linux/Documentation/power/swsusp.txt
- [Gentoo Forums: Suspend and Hibernate with UEFI](https://forums.gentoo.org/viewtopic-p-8111048.html#8111048)
- [How to achieve S0ix states in Linux](https://01.org/blogs/qwang59/2018/how-achieve-s0ix-states-linux) details on how to enable and troubleshoot S0ix in Linux
