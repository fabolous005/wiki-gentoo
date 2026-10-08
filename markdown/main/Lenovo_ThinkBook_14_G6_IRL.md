<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkBook_14_G6_IRL | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkBook 14 G6 IRL -->
---
title: Lenovo ThinkBook 14 G6 IRL
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkBook_14_G6_IRL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-08"
fingerprint: ae19dd2d51a63323
license: CC BY-SA 4.0
---

# Lenovo ThinkBook 14 G6 IRL

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Business laptop built on an integrated system-on-chip (SoC) platform powered by Intel Core mobile processor with integrated graphics, all housed within a aluminum enclosure.

For neural network workloads, it lacks a dedicated hardware NPU, but have an integrated Intel GNA (Gaussian & Neural Accelerator) co-processor primarily focused on low-power voice recognition. Have two RAM slots (up to 64GB) and two SSD slots. May be IRL (Intel) or ABP (AMD).

## Hardware

| Device | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| CPU | Intel i3/i5/i7 | Works | N/A | 6.18.52 |  | 
| GPU Video Card | Intel Corporation Iris Xe Graphics | Works | i915 xe | 6.18.52 | Firmware required. | 
| Audio speakers, microphone and jack (3.5mm) port | Intel Corporation Raptor Lake-P/U/H cAVS | Works | snd\_soc\_avs snd\_sof\_pci\_intel\_tgl snd\_hda\_intel | 6.18.52 | Requires configuration and firmware. | 
| Wi-Fi and Bluetooth | Intel Corporation Raptor Lake PCH CNVi WiFi | Works | iwlwifi wl | 6.18.52 | Firmware required. Bluetooth not tested. | 
| Ethernet (RJ-45) port | Intel Corporation Ethernet Connection (23) I219-V | Works | e1000e | 6.18.52 |  | 
| Trusted Platform Module (TPM) 2.0 | Intel Corporation Raptor Lake LPC/eSPI Controller | Works | N/A | 6.18.52 |  | 
| Gaussian & Neural Accelerator | Intel Corporation GNA Scoring Accelerator module | Borked | intel\_gna | 6.18.52 | No driver in gentoo-sources. | 
| Fingerprint scanner | Elan Microelectronics Corp. ELAN:Fingerprint | Borked | N/A | 6.18.52 | No driver available. | 
| SD/MMC Card Reader | O2 Micro, Inc. OZ711 SD/MMC Card Reader Controller | Works | sdhci\_pci | 6.18.52 |  | 

## Keys

- Fn + Esc - FnLock
- Fn + Space - keyboard backlight
- Fn + B - Ctrl+Break
- Fn + P - Pause
- Fn + N - Unknown key
- Fn + R - Unknown key
- Fn + M - TouchpadToggle
- Fn + S - Alt+SysRq
- Fn + K - SckrLk
- Fn + Right Ctrl - right mouse click
- Fn + i - left mouse click

Note: All keys are working, handled by hardware or propagated to OS.



## Bash aliases

Terminal commands for low-level hardware control using the /sys filesystem (requires CONFIG\_SYSFS=y in the kernel and appropriate user access permissions to the directory).

**`/root/.bash_aliases`**

**Display Backlight**

```
brsyspath=/sys/class/backlight/intel_backlight/brightness
alias br1="echo 1 > $brsyspath"
alias br2="echo 200 > $brsyspath"
alias brmax="echo 15360 > $brsyspath"
```
**`/root/.bash_aliases`**

**Get battery status**

```
ba() {
    local fulld=$(cat /sys/class/power_supply/BAT0/energy_full_design)
    echo -e "Full design \t" $(($fulld / 10000))
    local full=$(cat /sys/class/power_supply/BAT0/energy_full)
    echo -e "Energy_full\t" $(($full / 10000))
    local now=$(python3 -c 'print(f"{int(open("/sys/class/power_supply/BAT0/energy_now").read())/'$full'*100:.2f}%")')
    echo -e "Energy now\t" $now
    cat /sys/class/power_supply/BAT0/status
}
```
**`/root/.bashrc`**

**One time options**

```
# Enable Ideapad advanced Battery saving, that stop charging at 80% (recommended at User manual).
echo 1 > /sys/devices/pci0000:00/0000:00:1f.0/PNP0C09:00/VPC2004:00/conservation_mode
# Disable keyboard backlight
echo 0 > /sys/devices/pci0000:00/0000:00:1f.0/PNP0C09:00/VPC2004:00/leds/platform::kbd_backlight/brightness
```
**`/root/.bash_aliases`**

**Performance mode switching**

```
perf(){ # set max performance
    echo performance > /sys/firmware/acpi/platform_profile # affect CPU and funs
    echo "5000" > /sys/class/backlight/intel_backlight/brightness
    echo 100 > /sys/devices/system/cpu/intel_pstate/max_perf_pct
    echo 100 > /sys/devices/system/cpu/intel_pstate/min_perf_pct
    echo 0 > /sys/devices/system/cpu/intel_pstate/no_turbo
    echo performance | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
    sysctl -w kernel.nmi_watchdog=0 >/dev/null
    sysctl -w kernel.sched_autogroup_enabled=0 >/dev/null
}
power(){ # set powersave performance
    echo powersave | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor # old
    echo powersave > /sys/module/pcie_aspm/parameters/policy
    echo low-power > /sys/firmware/acpi/platform_profile
    echo 1 > /sys/devices/system/cpu/intel_pstate/no_turbo
    echo 40 > /sys/devices/system/cpu/intel_pstate/max_perf_pct
    echo 0  > /sys/devices/system/cpu/intel_pstate/min_perf_pct
    sysctl -w kernel.nmi_watchdog=1 >/dev/null #default
    sysctl -w kernel.sched_autogroup_enabled=1 >/dev/null #default
}
pbal(){ # set balanced performance
    echo powersave | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor # old
    echo balance_performance | tee /sys/devices/system/cpu/cpu*/cpufreq/energy_performance_preference
    echo powersave > /sys/module/pcie_aspm/parameters/policy
    echo balanced > /sys/firmware/acpi/platform_profile
    echo 0 > /sys/devices/system/cpu/intel_pstate/no_turbo
    echo 100 > /sys/devices/system/cpu/intel_pstate/max_perf_pct
    echo 0  > /sys/devices/system/cpu/intel_pstate/min_perf_pct
    sysctl -w kernel.nmi_watchdog=1 >/dev/null #default
    sysctl -w kernel.sched_autogroup_enabled=1 >/dev/null #default
}
pget(){ echo other commands: perf, power, pbal
	  echo scaling_governor:
	  cat /sys/devices/system/cpu/cpu[0-7]/cpufreq/scaling_governor
	  cat /sys/devices/system/cpu/cpu8/cpufreq/scaling_governor
	  cat /sys/devices/system/cpu/cpu9/cpufreq/scaling_governor
	  cat /sys/devices/system/cpu/cpu1*/cpufreq/scaling_governor
	  echo energy_performance_preference:
	  cat /sys/devices/system/cpu/cpu*/cpufreq/energy_performance_preference # default performance balance_performance balance_power power
	  echo acpi/platform_profile_choices: $(cat /sys/firmware/acpi/platform_profile_choices)
	  echo acpi/platform_profile
	  cat /sys/firmware/acpi/platform_profile # low-power balanced performance
	  echo pcie_aspm/parameters/policy
	  cat /sys/module/pcie_aspm/parameters/policy # default performance [powersave] powersupersave
	  echo pstate/no_turbo
	  cat /sys/devices/system/cpu/intel_pstate/no_turbo
	  echo pstate/min_perf_pct and max_perf_pct
	  cat /sys/devices/system/cpu/intel_pstate/min_perf_pct
	  cat /sys/devices/system/cpu/intel_pstate/max_perf_pct
	}
```
## GPU and make.conf

You may choose to use "i915" or "xe" driver for GPU. i915 is main driver.

New xe driver was tested and fully working with Wayland. Should be set as \<M> module and require option: "Force probe xe for selected Intel hardware IDs"

**`/etc/portage/make.conf`**

```
VIDEO_CARDS="intel"
INPUT_DEVICES="libinput synaptics"
```
## Linux kernel configuration

### Firmware

`root #``emerge --ask sys-kernel/linux-firmware``root #``emerge --ask sys-firmware/sof-firmware`
Note: without microcode for CPU.

### Supported options

## Minor disadvantages

Pressure on laptop cover goes directly to display and potentially may break it.

Firmware required for Wifi, GPU and audio.

Impossible to boot from SD card. SD card slot dont allow to put card in fully.

Impossible to lock boot order in UEFI/BIOS (security issue of all UEFI). No Legacy BIOS boot support.

Touchpad is too large, easy to touch it accidentally.

GPU driver works only when \<M> loaded as module.
