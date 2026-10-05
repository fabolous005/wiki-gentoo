<!-- source: https://wiki.gentoo.org/wiki/PowerTOP | group: Gentoo Wiki (Main) | wiki-title: PowerTOP -->
---
title: PowerTOP
url: https://wiki.gentoo.org/wiki/PowerTOP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-06"
fingerprint: "36131a16d1eb39a2"
license: CC BY-SA 4.0
---

# PowerTOP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**PowerTOP** is a Linux utility that can monitor and display a system's electrical power usage. It is useful as a hardware monitoring and diagnostic tool. It is among the most powerful battery stretching utilities for notebook computers.

## Installation

### Kernel

Several kernel options must be enabled in the kernel for PowerTOP to work properly. These include: `CONFIG_DEBUG_FS`, `CONFIG_TRACING`, `CONFIG_BLK_DEV_IO_TRACE`, `CONFIG_TIMER_STATS` (was removed in kernel 4.11), `CONFIG_CPU_FREQ_STAT`, and `CONFIG_CPU_FREQ_STAT_DETAILS` (removed in 4.11, and rolled into `CONFIG_CPU_FREQ_STAT`).

For newer Intel Core series of processors (based on the Sandy Bridge microarchitecture or newer) enable the [powercap sysfs driver](https://wiki.gentoo.org/wiki/Power_management/Guide#powercap_sysfs_driver) via `CONFIG_POWERCAP` and `CONFIG_INTEL_RAPL`.

Optionally, for wireless power saving enable: `CONFIG_TRACEPOINTS`.

**Enable support for PowerTOP in the kernel**

**Generic powercap sysfs driver**

Processor type and features --->
  \<M> /dev/cpu/\*/msr - Model-specific register support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_MSR\</code> to find this item.
Device Drivers --->
  \<\*> Generic powercap sysfs driver ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_POWERCAP\</code> to find this item.
    \<M> Intel RAPL Support via MSR Interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_RAPL\</code> to find this item.

If the above options have not properly been enabled [Portage](https://wiki.gentoo.org/wiki/Portage) will display warning messages at the end of the emerge. If help is required for upgrading the kernel while enabling the above options, be sure to see the [kernel upgrade article](https://wiki.gentoo.org/wiki/Kernel/Upgrade)!

### USE flags


### Emerge

After setting USE flags, emerge PowerTOP:

`root #``emerge --ask sys-power/powertop`
## Configuration

PowerTOP does not have any configuration other than passing options via the command-line.

### Calibration

Calibration can be performed in order for PowerTOP to gain an understanding of the system:

`root #``powertop --calibrate`
## Usage

### Invocation

`user $``/usr/sbin/powertop --help````
Usage: powertop [OPTIONS]
     --auto-tune         sets all tunable options to their GOOD setting
 -c, --calibrate         runs powertop in calibration mode
 -C, --csv[=filename]    generate a csv report
     --debug             run in "debug" mode
     --extech[=devnode]  uses an Extech Power Analyzer for measurements
 -r, --html[=filename]   generate a html report
 -i, --iteration[=iterations] number of times to run each test
 -q, --quiet             suppress stderr output
 -t, --time[=seconds]    generate a report for 'x' seconds
 -w, --workload[=workload] file to execute for workload
 -V, --version           print version information
 -h, --help              print this help menu
For more help please refer to the 'man 8 powertop'
```
## Troubleshooting

### Could not find a Makefile in the kernel source directory

After an emerge a message similar to the following message may be displayed:

This warning indicates the PowerTOP ebuild has attempted to verify a successful operating environment for the PowerTOP software package. In order to be sure PowerTOP will work as intended, at the end of the emerge process, a check is ran against the current kernel source configuration. In the case of the above message two warnings were provided:

1. No kernel sources have been detected. This can happen as a result of running an emerge --depclean or failed to have a specific kernel set using the eselect kernel command.
2. Since no kernel sources have been detected Portage was not able to scan the kernel's .config file to determine if the correct features have been enabled in the kernel. According to the error message above, five features are not set. The features may or may not be set in the current running kernel. Portage is simply making the user aware there is no way to verify these features have been set without a .config file. If PowerTOP can perform some functions but not others, be sure all the kernel features listed have been enabled. For more information on how to do so consult the [kernel configuration article](https://wiki.gentoo.org/wiki/Kernel/Configuration).

## See also

- [Power management](https://wiki.gentoo.org/wiki/Power_management) — describes methods to save energy for longer battery runtimes, a quieter computer, lower power bills, and an environmentally friendly impact.

## External resources

- [https://www.linux.com/learn/powertop-finds-power-hogs-your-linux-pc](https://www.linux.com/learn/powertop-finds-power-hogs-your-linux-pc) - A Linux.com article on using PowerTOP to measure large power consumers.
- [https://01.org/sites/default/files/page/powertop\_users\_guide\_201406.pdf](https://01.org/sites/default/files/page/powertop_users_guide_201406.pdf) - A PowerTOP user guide writen by two Intel employees (PDF).
