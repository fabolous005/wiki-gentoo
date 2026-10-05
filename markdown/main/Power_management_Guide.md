<!-- source: https://wiki.gentoo.org/wiki/Power_management/Guide | group: Gentoo Wiki (Main) | wiki-title: Power management/Guide -->
---
title: Power management/Guide
url: https://wiki.gentoo.org/wiki/Power_management/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-21"
fingerprint: b2133f77e136ae22
license: CC BY-SA 4.0
---

# Power management/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This is a guide to setup power management features of a laptop. Application to non-laptops should be done carefully.

## Two key terms

Users should distinguish *laptop\_mode*, a kernel feature and laptop-mode-tools, a package.

The laptop\_mode setting is an in-kernel configuration setting that optimizes I/O, allowing disks to spin down properly (and not be woken up immediately afterwards for queued operations).

The *Laptop Mode Tools* is a software package ([app-laptop/laptop-mode-tools](https://packages.gentoo.org/packages/app-laptop/laptop-mode-tools)) which allows the user to optimize power saving functions. It allows managing the laptop\_mode setting in the Linux kernel, but has additional features that allow the tweaking of other power-related settings on the system.

## Linux kernel configuration

### Minimum kernel setup

To enable proper power management features in the Linux kernel, enable the following settings:

**Minimum kernel setup for Power Management**

Power management and ACPI options --->
  \[\*\] Device power management core functionality [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PM\</code> to find this item.
  \[\*\] ACPI (Advanced Configuration and Power Interface) Support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\</code> to find this item.
    \<\*> AC Adapter [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_AC\</code> to find this item.
    \<\*> Battery [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_BATTERY\</code> to find this item.
    \<\*> Button [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_BUTTON\</code> to find this item.
    \<\*> Video [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_VIDEO\</code> to find this item.
    \<\*> Fan [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_FAN\</code> to find this item.
    \<\*> Processor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_PROCESSOR\</code> to find this item.
    \<\*> Thermal Zone [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_THERMAL\</code> to find this item.
  CPU Frequency scaling --->
    \[\*\] CPU Frequency scaling [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\</code> to find this item.
    Default CPUFreq governor (schedutil) ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_DEFAULT\_GOV\_SCHEDUTIL\</code> to find this item.
    \<\*> 'performance' governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_GOV\_PERFORMANCE\</code> to find this item.
    \<\*> 'powersave' governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_GOV\_POWERSAVE\</code> to find this item.
    \<\*> 'userspace' governor for userspace frequency scaling [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_GOV\_USERSPACE\</code> to find this item.
    \<\*> 'ondemand' cpufreq policy governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_GOV\_ONDEMAND\</code> to find this item.
    \<\*> 'conservative' cpufreq governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_GOV\_CONSERVATIVE\</code> to find this item.
    \[\*\] 'schedutil' cpufreq policy governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_GOV\_SCHEDUTIL\</code> to find this item.
    \*\*\* CPU frequency scaling drivers \*\*\*
    \<\*> ACPI Processor P-States driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_ACPI\_CPU\_FREQ\</code> to find this item.
    \*\*\* shared options \*\*\*
    \[\*\] Current frequency derived from HW provided feedback [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPUFREQ\_ARCH\_CUR\_FREQ\</code> to find this item.

### Selecting CPU frequency driver

Available CPU frequency drivers are summarized in the separate [Processor guide](https://wiki.gentoo.org/wiki/Power_management/Processor#Specific_CPU_scaling_drivers_settings).

### Enabling additional drivers

Thermal and Powercap sysfs are additional advanced features. For Intel processors used in laptops since 2011. The Powercap sysfs can be used for AMD Zen processors since kernel 5.11.[\[1\]](https://wiki.gentoo.org#cite_note-1)

#### Thermal sysfs driver

**Generic Thermal sysfs driver**

Device Drivers --->
  \<\*> Thermal drivers ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\</code> to find this item.
    (0) Emergency poweroff delay in milli-seconds [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_EMERGENCY\_POWEROFF\_DELAY\_MS\</code> to find this item.
    \[\*\] Expose thermal sensors as hwmon device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_HWMON\</code> to find this item.
    Default Thermal governor (step\_wise) ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_DEFAULT\_GOV\_STEP\_WISE\</code> to find this item.
    \[\*\] Fair-share thermal governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_GOV\_FAIR\_SHARE\</code> to find this item.
    \[\*\] Step\_wise thermal governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_GOV\_STEP\_WISE\</code> to find this item.
    \[\*\] Bang Bang thermal governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_GOV\_BANG\_BANG\</code> to find this item.
    \[\*\] User\_space thermal governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_GOV\_USER\_SPACE\</code> to find this item.
    \[\*\] Power allocator thermal governor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_THERMAL\_GOV\_POWER\_ALLOCATOR\</code> to find this item.
    Intel Thermal drivers --->
      \<M> Intel PowerClamp idle injection driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_POWERCLAMP\</code> to find this item.
      \<M> X86 package temperature thermal driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_PKG\_TEMP\_THERMAL\</code> to find this item.
      \< > Intel SoCs DTS thermal driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_SOC\_DTS\_THERMAL\</code> to find this item.
      ACPI INT340X thermal drivers --->
        \<M> ACPI INT340X thermal drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INT340X\_THERMAL\</code> to find this item.
      \<M> Intel PCH Thermal Reporting Driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_PCH\_THERMAL\</code> to find this item.

Additionally, you can enable *Running Average Power Limit* (RAPL) technology. Both [PowerTOP](https://wiki.gentoo.org/wiki/PowerTOP) and [turbostat](https://wiki.gentoo.org/index.php?title=Turbostat&action=edit&redlink=1) utilities are aware of RAPL. To enable RAPL, use:

**Generic powercap sysfs driver**

Device Drivers --->
  \<\*> Generic powercap sysfs driver ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_POWERCAP\</code> to find this item.
    \<M> Intel RAPL Support via MSR Interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_RAPL\</code> to find this item.

To make use of Intel's [Linux thermal daemon](https://github.com/intel/thermal_daemon) you first need to emerge [sys-power/thermald](https://packages.gentoo.org/packages/sys-power/thermald) and enable it via:

`root #``rc-update add thermald default`
or

`root #``systemctl enable thermald`
### Kernel setup finalization

For a more detailed configuration description see the [Processor Kernel](https://wiki.gentoo.org/wiki/Power_management/Processor#Kernel) article.

[Build and install](https://wiki.gentoo.org/wiki/Kernel/Rebuild) the new kernel (if necessary) and reboot.

## Using TLP

### Installation

TLP is similar to laptop-mode-tools but aims to work out of the box with safe, modern defaults.

To install it:

`root #``emerge --ask sys-power/tlp`
Then simply enable it on OpenRC systems like so:

`root #````
rc-update add tlp default
```
`root #````
rc-service tlp start
```
Or for systemd machines:

`root #``systemctl enable --now tlp`
TLP is a 'set and forget' style package, but it can be customized if desired.

## Using Laptop Mode Tools

### Installation

It comes to no surprise that installation of the *Laptop mode tools* software is easily done via:

`root #``emerge --ask app-laptop/laptop-mode-tools`
However, this package takes on additional, optional settings through USE flag configuration. So let's first take a look at the supported USE flags and what they mean to the package.

| USE flag | Description | Suggested when... | 
|---|---|---|
| `acpi` | Depend on [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid) so that changes in the system are captured and power saving features are automatically enabled/disabled. | the laptop is not too old (around year 2003 and later). | 
| `apm` | Depend on [sys-apps/apmd](https://packages.gentoo.org/packages/sys-apps/apmd) so that changes in the system are captured and power saving features are automatically enabled/disabled. | the laptop is very old. | 

Observe that these two USE flags seem to collide; So what is the deal?

- The `apm` USE enables support for *Advanced Power Management* , an older (before year 2000) standard for power management features within a system.
- The `acpi` USE enables support for *Advanced Configuration and Power Interface* , the successor of APM. All modern laptops support ACPI.

Depending on the system, either `acpi` or `apm` will need to be set. In the remainder of this guide, it is assumed the laptop is recent enough to use ACPI.

So, with the USE flags set, install laptop-mode-tools:

`root #``emerge --ask app-laptop/laptop-mode-tools`
### Configuration

Having laptop-mode-tools installed on the system does not automatically enable the power management features that may be needed. To configure the package, first take a look at /etc/laptop-mode/laptop-mode.conf. This is the main configuration file for the package and is pretty well described (through comments).

But it is not the only configuration file to work with. The laptop-mode-tools package supports plugins (or modules) which have their own configuration file(s). These files are located in /etc/laptop-mode/conf.d and are named after the module they represent (such as intel-sata-powermgmt.conf).

Now, one of the important settings in each configuration file is if the laptop-mode-tools package should govern a particular setting or not. This is important when combining laptop-mode-tools with other power management services. In this example case, `CONTROL_CPU_FREQUENCY=0` must be set:

**`/etc/laptop-mode/conf.d/cpufreq.conf`**

The next few sections will help the user configure laptop-mode-tools to suit specific needs. When finished, start the laptop\_mode service and make sure it is started upon system boot.

With OpenRC:

`root #````
rc-service laptop_mode start
```
`root #````
rc-update add laptop_mode default
```
With systemd:

`root #````
systemctl enable laptop-mode.service --now
```
### How laptop-mode-tools works

When running the laptop\_mode service, the software will check in which state the system is in. The states are defined as:

- *Battery*, which is active when the system is running on battery power; the configuration files use the `BATT_` prefix for settings related to this state;
- *AC*, which is active when the system is running on AC power; the configuration files use the `AC_` prefix for settings related to this state;
- *Laptop Mode*, which is active when *laptop mode* is enabled; the configuration files use the `LM_` prefix for settings related to this state;
- *No Laptop Mode*, which is active when *laptop mode* is disabled; the configuration files use the `NOLM_` prefix for settings related to this state.

The `AC/BATT_` and `LM/NOLM_` prefixes can be combined to have a `AC_LM_` prefix.

When the laptop\_mode service is started, it will switch modes based on events that occur (and of course based on the configuration settings). For instance, the setting `ENABLE_LAPTOP_MODE_ON_BATTERY=1` will make sure that the laptop mode tools switch to *laptop mode* when battery power is used. If that is the case, then the settings starting with `LM_`, `LM_BATT_`, `BATT_LM_`, and `BATT_` will be used.

To make sure settings do not collide, it is not allowed to have overlapping settings. In the next example, the first set (for `CPU_MAXFREQ`) is valid, but the second one (for `CPU_MINFREQ`) is not.

### Configuring CPU frequency management

The support for CPU frequency management in the laptop mode tools allows switching frequencies. It supports setting the CPU frequency governor, minimum frequency and maximum frequency. The configuration file used here is /etc/laptop-mode/conf.d/cpufreq.conf

The *CPU frequency governor* is a kernel-level policy that defines how the kernel will select the CPU frequency. We already selected the governors we want to use in the kernel configuration earlier. Let's recap:

- `performance` always picks the highest frequency.
- `powersave` always picks the lowest frequency.
- `userspace` does not pick anything, but lets the user decide (or any process that the user is running that will decide for the user).
- `ondemand` will scale the CPU frequency up to the highest frequency when load is available.
- `conservative` will scale the CPU frequency up gradually when load is available.
- `schedutil` will scale the CPU frequency based on the scheduler context; this governor is generally regarded as a replacement for the older `ondemand` and `conservative` governors<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

When switching between AC or battery, or (no) laptop mode, the appropriate governor (as well as its minimum and maximum frequency) is selected.

### Configuring display brightness

With /etc/laptop-mode/conf.d/lcd-brightness.conf, the laptop mode tools can govern the brightness of the LCD screen.

The file currently uses the /proc/acpi/video/VID/LCD/brightness file ([bug 499544](https://bugs.gentoo.org/show_bug.cgi?id=499544)) to set brightness values. Recent kernels do not provide this anymore; it will need to adjust this to /sys/class/backlight/acpi\_video0/brightness instead.

The possible values that can be used are between 0 and the value in /sys/class/backlight/acpi\_video0/max\_brightness, with 0 being the lowest brightness value.

### Configuring other services

An interesting feature of laptop-mode-tools is to support reloading particular services (like the system logger) after switching its configuration file. This is handled through /etc/laptop-mode/conf.d/configuration-file-control.conf

If enabled, the laptop\_mode application will switch the configuration file(s) of the mentioned services with the same file, but suffixed with -nolm-ac , -lm-ac or -batt. It will then signal or reload the appropriate services so they can use the new configuration file.

## Using power-profiles-daemon

power-profiles-daemon modify system behavior using profiles. There are 3 different power profiles, "balanced", "power-saver", and "performance".

### Installation

To install it:

`root #``emerge --ask sys-power/power-profiles-daemon`
Now enable and start the service:

`root #````
rc-update add power-profiles-daemon default
```
`root #````
rc-service power-profiles-daemon start
```
on systemd systems:

`root #``systemctl enable --now power-profiles-daemon`
### Use

To list profiles:

`user $``powerprofilesctl`
To set the profile:

`user $``powerprofilesctl set profile_name`
## See also

- [Power management](https://wiki.gentoo.org/wiki/Power_management) — describes methods to save energy for longer battery runtimes, a quieter computer, lower power bills, and an environmentally friendly impact.
- [PowerTOP](https://wiki.gentoo.org/wiki/PowerTOP) — a Linux utility that can monitor and display a system's electrical power usage.

## External resources

- [Laptop Mode Tools Homepage](http://samwel.tk/laptop_mode/), includes [About laptop mode](http://samwel.tk/laptop_mode/laptop_mode).
- A ThinkWiki article on [How to reduce power consumption](http://www.thinkwiki.org/wiki/How_to_reduce_power_consumption) (on Linux). This article offers an exhaustive list of measures one can take. However, it should be noted that the laptop mode tools implements the majority of these (if properly configured).
