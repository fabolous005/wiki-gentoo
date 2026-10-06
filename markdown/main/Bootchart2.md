<!-- source: https://wiki.gentoo.org/wiki/Bootchart2 | group: Gentoo Wiki (Main) | wiki-title: Bootchart2 -->
---
title: Bootchart2
url: https://wiki.gentoo.org/wiki/Bootchart2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: "3011997f1f26bd85"
license: CC BY-SA 4.0
---

# Bootchart2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This script is run instead of init during the boot process after the kernel is loaded and measures the time during this process. With the aid of a script in the project's Git repository, it generates nice charts which show when each daemon started and stopped.

## Installation

You can install [app-benchmarks/bootchart2](https://packages.gentoo.org/packages/app-benchmarks/bootchart2) with the following command:

`root #``emerge --ask app-benchmarks/bootchart2`
Then add the daemon to the default runlevel. This is needed to stop the bootchart process and generate the image:

`root #``rc-update add bootchart2 default`
### USE flags


### USE flags for
            [app-benchmarks/bootchart2](https://packages.gentoo.org/packages/app-benchmarks/bootchart2)
            
            Performance analysis and visualization of the system boot process

| [+cairo](https://packages.gentoo.org/useflags/+cairo) | Enable support for the cairo graphics library | 

### Kernel

To get more verbose information:

**enable proc events**

```
General setup  --->
    CPU/Task time and stats accounting -->
        -*- Export task/process statistics through netlink (EXPERIMENTAL)
Device Drivers --->
    <*> Connector - unified userspace <-> kernelspace linker  --->
        [*]   Report process events to userspace
```
## Usage

How to enable bootchart2 during boot and access the generated charts.

### Bootloader

- For any GRUB based bootloader, the kernel commandline can be changed during boot by pressing `e` in the boot menu, then edit the kernel commandline and press `F10` to boot with changed parameters. The following options should be added:

- 'initcall\_debug' - append timing data to each initcall
- 'printk.time=y' - append timing data to each message during initialization
- 'init=/sbin/bootchartd' - replace the default command for initialization with /sbin/bootchartd to start our benchmark

- To change the entry static for GRUB (bootchart on every boot):

In [GRUB](https://wiki.gentoo.org/wiki/GRUB) the parameters can be added to the variable `GRUB_CMDLINE_LINUX_DEFAULT` in /etc/default/grub.

Then run:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
### Analyzing the output

The values are stored in /var/log/bootchart.tgz as a compressed tarball.

A script that generates an easier-to-analyze PDF or PNG from this tarball can be found on the project's homepage, by cloning the git repository, and then running `make pybootchartgui/main.py` and `./pybootchartgui.py`. The script requires [dev-python/pycairo](https://packages.gentoo.org/packages/dev-python/pycairo).
