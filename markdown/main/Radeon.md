<!-- source: https://wiki.gentoo.org/wiki/Radeon | group: Gentoo Wiki (Main) | wiki-title: Radeon -->
---
title: radeon
url: https://wiki.gentoo.org/wiki/Radeon
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-10"
fingerprint: e1b985a4136992c
license: CC BY-SA 4.0
---

# radeon

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**radeon** is a family of *open source* graphics drivers for *older* AMD/ATI Radeon graphics cards. Cards based on [Graphics Core Next](https://en.wikipedia.org/wiki/Graphics_Core_Next) (GCN) 2.0 "Sea Islands" are also fully supported by the newer [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) driver, which also features experimental support for GCN1.1 (Southern Islands). Neither this nor the AMDGPU article cover installation and configuration of the closed source drivers (see the next paragraph).

Be aware AMD has dropped the support for closed source [fglrx](https://wiki.gentoo.org/wiki/Fglrx) drivers (called Catalyst on Windows). The fglrx drivers only work with certain versions of the X server. This is contrary to the open source drivers, which are now compiled against the system's currently installed kernel and X server.

Those interested in the *new* closed source AMDGPU-PRO drivers (called Crimson on Windows) should head over to the [AMDGPU-PRO](https://wiki.gentoo.org/wiki/AMDGPU-PRO) article.

## Installation

### Prerequisites

#### Hardware detection

To choose the right driver, first detect the graphics card. Use [lspci](https://wiki.gentoo.org/wiki/Hardware_detection) for this task:

`root #``lspci | grep -i VGA`
#### Feature support

| Family | Chipset name | Product name | OpenGL | OpenGL ES | `[VIDEO_CARDS](https://wiki.gentoo.org/wiki/VIDEO_CARDS)` | 
|---|---|---|---|---|---|
| R100 <sup>4</sup> | R100, RV100, RV200, RS100, RS200 | 7xxx, 320-345 |  |  | `radeon r100` | 
| R200 <sup>4</sup> | R200, RV250, RV280, RS300 | 8xxx-9250 |  |  | `radeon r200` | 
| R300 | R300, R350, RV350, RV370, RV380, RS400, RS480 | 9500-9800, X300-X600, X1050-X1150, 200M |  |  | `radeon r300` | 
| R400 | R420, R423, RV410, RS600, RS690, RS740 | X700-X850, X12xx, 2100 |  |  | `radeon r300` | 
| R500 | RV515, R520, RV530, RV560, RV570, R580 | X1300-X2300, HD2300 |  |  | `radeon r300` | 
| R600 | R600, RV610, RV630, RV620, RV635, RV670, RS780, RS880 | HD2400-HD4290 |  |  | `radeon r600` | 
| R700 | RV770, RV730, RV710, RV740 | HD4330-HD5165, HD5xxV |  |  | `radeon r600` | 
| Evergreen | CEDAR, REDWOOD, JUNIPER, CYPRESS, PALM (Wrestler/Ontario), SUMO, SUMO2 (Llano), SUMO2 (Llano) | HD5430-HD5970, all HD6xxx not listed under Northern Islands, HD7350 | <sup>1</sup> |  | `radeon r600` | 
| Northern Islands | ARUBA, BARTS, TURKS, CAICOS, CAYMAN | HD6450, HD6570, HD6670, HD6790-HD6990, HD64xxM, HD67xxM, HD69xxM, HD7450-HD7670, HD8450, R5 230 | <sup>1,2</sup> |  | `radeon r600` | 
| Southern Islands <sup>**A**</sup> | CAPE VERDE, PITCAIRN, TAHITI, OLAND, HAINAN | HD7750-HD7970, HD8550M-HD8790M, R9 270, R9 280, R9 370X, R7 240, R7 250 | <sup>3</sup> |  | `radeon radeonsi` | 
| Sea Islands <sup>**A**</sup> | BONAIRE, KABINI, MULLINS, KAVERI, HAWAII | HD7790, HD8180-HD8400, R7 260, R9 290 | <sup>3</sup> |  | `radeon radeonsi` | 

- <sup>1</sup> OpenGL 4.2+ is currently only supported on CYPRESS, CAYMAN and ARUBA (non-mobile HD6xxx or better). Other chips are limited to OpenGL 3.3 due to lacking 64-bit arithmetic.
- <sup>2</sup> OpenGL 4.5 requires [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) 4.7 or higher.
- <sup>3</sup> OpenGL 4.6 requires [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) 20.0.0 or higher<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>
- <sup>4</sup> Mesa 22.0 and higher have dropped support for R100 and R200
- <sup>**A**</sup> For *Southern Islands* and *Sea Island* it is highly recommended to use the newer [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) driver for better performance.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> Only on older kernels (pre-6.19) and older software (Mesa) will the radeon driver be more stable.

If the card is not available in the above list the [X.org wiki](http://wiki.x.org/wiki/RadeonFeature/) is a good place to look for the latest hardware.

### Kernel

Depending on the card or the features desired, set the following kernel options.

#### General

**General support**

#### AMD "Zen" CPUs

If you have a recent AMD CPU with Secure Memory Encryption (SME), it should *not* be activated by default (Linux 4.14+).  This feature is indicated by `sme` in flags of `/proc/cpuinfo`.

**General support**

#### AGP

For cards that sit in an AGP slot, enable the AGP driver:

**AGP cards**

#### Audio

Some cards have the ability to output audio over the HDMI or DisplayPort connection. Set the following options for audio support:

**Audio support**

The options from the Sound card support menu need only to be set if the card supports HDMI or DisplayPort audio and you want to use it. Other options there might be needed as well, e.g. don't set the number of soundcards too low, because even with one Radeon card alsa may detect more than one HDA ATI HDMI device, e.g.

`root #``aplay -l`
\*\*\*\* List of PLAYBACK Hardware Devices \*\*\*\*
card 0: NVidia \[HDA NVidia\], device 0: ALC888 Analog \[ALC888 Analog\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: NVidia \[HDA NVidia\], device 1: ALC888 Digital \[ALC888 Digital\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: HDMI \[HDA ATI HDMI\], device 3: HDMI 0 \[HDMI 0\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: HDMI \[HDA ATI HDMI\], device 7: HDMI 1 \[HDMI 1\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: HDMI \[HDA ATI HDMI\], device 8: HDMI 2 \[HDMI 2\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: HDMI \[HDA ATI HDMI\], device 9: HDMI 3 \[HDMI 3\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: HDMI \[HDA ATI HDMI\], device 10: HDMI 4 \[HDMI 4\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: HDMI \[HDA ATI HDMI\], device 11: HDMI 5 \[HDMI 5\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

### Firmware

Microcode (firmware) is required for R500 and newer GPUs. Install [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) (which also contains other firmware).

#### linux-firmware

##### USE flags


| [+initramfs](https://packages.gentoo.org/useflags/+initramfs) | Create and install initramfs for early microcode loading in /boot (only AMD for now) | 
| [+redistributable](https://packages.gentoo.org/useflags/+redistributable) | Install also non-free (but redistributable) firmware files | 
| [bindist](https://packages.gentoo.org/useflags/bindist) | Flag to enable or disable options for prebuilt (GRP) packages (eg. due to licensing issues) | 
| [compress-xz](https://packages.gentoo.org/useflags/compress-xz) | Compress firmware using xz (app-arch/xz-utils) before installation | 
| [compress-zstd](https://packages.gentoo.org/useflags/compress-zstd) | Compress firmware using zstd (app-arch/zstd) before installation | 
| [deduplicate](https://packages.gentoo.org/useflags/deduplicate) | Create symlinks for all firmware that is duplicate using rdfind | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Delegate microcode initramfs generation to sys-kernel/installkernel | 
| [savedconfig](https://packages.gentoo.org/useflags/savedconfig) | Allows individual selection of firmware files | 
| [unknown-license](https://packages.gentoo.org/useflags/unknown-license) | Install firmware files whose license is unknown | 

With packages like these, which introduce a number of non-free binary blobs into the system, if you are security aware, it pays to use the [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig) use flag, and do some removing of the unnecessary lines, or better yet uncommenting them, from the respective savedconfig file.  See "How to install the linux-firmware package in Gentoo" in [External resources](https://wiki.gentoo.org#External_resources) at bottom of this article.

However, savedconfig editing is entirely optional, those in a hurry may not want to take this route. The system will work the same, with or without the savedconfig editing.

##### Emerge

`root #``emerge --ask sys-kernel/linux-firmware`
#### Built-in kernel

When radeon has been compiled directly into the kernel (instead of as a module), make sure the firmware for the model (check available ones in /lib/firmware/radeon) is built-in to the kernel as well:

**Enable support for Linux firmware**

If the kernel cannot find the required firmware, in `dmesg` output there can be something like this:

`root #``dmesg | less`
…
\[   11.413985\] \[drm\] Loading R500 Microcode
\[   11.413991\] Loading firmware: radeon/R520\_cp.bin
\[   11.414074\] radeon 0000:01:00.0: Direct firmware load for radeon/R520\_cp.bin failed with error -2
\[   11.414085\] radeon\_cp: Failed to load firmware "radeon/R520\_cp.bin"
\[   11.414089\] \[drm:radeon\_get\_legacy\_connector\_info\_from\_table \[radeon\]\] \*ERROR\* Failed to load firmware!
…

Below is a list of the firmware files needed for each family/chipset of cards:

| Family | Chipset name | Product name | Firmware | 
|---|---|---|---|
| R500 | RV515 | X1300-1500 | radeon/R520\_cp.bin | 
| R600 | R600, RV610, RV630, RV620, RV635, RV670 | HD2400-2900, HD3450-3870 | radeon/R600\_rlc.bin radeon/R600\_uvd.bin | 
|  | RS780, RS880 | HD3200, HD3300, HD4200 | radeon/R600\_rlc.bin radeon/RS780\_uvd.bin radeon/RS780\_pfp.bin radeon/RS780\_me.bin | 
| R700 | RV710 | HD4300, HD4500, HD5145 | radeon/R700\_rlc.bin radeon/RV710\_smc.bin radeon/RV710\_uvd.bin radeon/RV710\_pfp.bin radeon/RV710\_me.bin | 
|  | RV730 | HD4600, HD5100, HD500V | radeon/R700\_rlc.bin radeon/RV730\_smc.bin radeon/RV710\_uvd.bin radeon/RV730\_me.bin radeon/RV730\_pfp.bin | 
|  | RV740 | HD4770 | radeon/R700\_rlc.bin radeon/RV740\_smc.bin radeon/RV710\_uvd.bin | 
|  | RV790, RV770 | HD4730, HD4830-HD4890 | radeon/R700\_rlc.bin radeon/RV770\_smc.bin radeon/RV770\_uvd.bin | 
| Evergreen | CEDAR | HD5450, HD6350, HD7350, R5 220 | radeon/CEDAR\_me.bin radeon/CEDAR\_pfp.bin radeon/CEDAR\_rlc.bin radeon/CEDAR\_smc.bin radeon/CYPRESS\_uvd.bin | 
|  | REDWOOD | HD5550, HD5570, HD5670 | radeon/REDWOOD\_me.bin radeon/REDWOOD\_pfp.bin radeon/REDWOOD\_rlc.bin radeon/REDWOOD\_smc.bin radeon/CYPRESS\_uvd.bin | 
|  | JUNIPER | HD5750, HD5770, HD6770 | radeon/JUNIPER\_me.bin radeon/JUNIPER\_pfp.bin radeon/JUNIPER\_rlc.bin radeon/JUNIPER\_smc.bin radeon/CYPRESS\_uvd.bin | 
|  | CYPRESS | HD5830, HD5850, HD5870, HD5970 | radeon/CYPRESS\_me.bin radeon/CYPRESS\_pfp.bin radeon/CYPRESS\_rlc.bin radeon/CYPRESS\_smc.bin radeon/CYPRESS\_uvd.bin | 
|  | PALM (Wrestler) | HD6250, HD6310, HD7310, HD7340 | radeon/PALM\_me.bin radeon/PALM\_pfp.bin radeon/SUMO\_rlc.bin radeon/SUMO\_uvd.bin | 
|  | SUMO | HD6290, HD6320, HD6480G, HD6520G, HD6620G | radeon/SUMO\_me.bin radeon/SUMO\_pfp.bin radeon/SUMO\_rlc.bin radeon/SUMO\_uvd.bin | 
|  | SUMO2 | HD6370D, HD6410D | radeon/SUMO2\_me.bin radeon/SUMO2\_pfp.bin radeon/SUMO\_rlc.bin radeon/SUMO\_uvd.bin | 
| Northern Islands | CAICOS | HD6450, HD64xxM, HD7450, HD8450, R5 230, R5 235, R5 235X | radeon/BTC\_rlc.bin radeon/CAICOS\_mc.bin radeon/CAICOS\_me.bin radeon/CAICOS\_pfp.bin radeon/CAICOS\_smc.bin radeon/SUMO\_uvd.bin | 
|  | TURKS | HD6570, HD6670, HD7550M/7570M/7650M | radeon/BTC\_rlc.bin radeon/TURKS\_mc.bin radeon/TURKS\_me.bin radeon/TURKS\_pfp.bin radeon/TURKS\_smc.bin radeon/SUMO\_uvd.bin | 
|  | BARTS | HD6790, HD6850, HD6870, HD67xxM | radeon/BTC\_rlc.bin radeon/BARTS\_mc.bin radeon/BARTS\_me.bin radeon/BARTS\_pfp.bin radeon/BARTS\_smc.bin radeon/SUMO\_uvd.bin | 
|  | CAYMAN | HD6950, HD6970, HD6990, HD69xxM | radeon/CAYMAN\_mc.bin radeon/CAYMAN\_me.bin radeon/CAYMAN\_pfp.bin radeon/CAYMAN\_rlc.bin radeon/CAYMAN\_smc.bin radeon/SUMO\_uvd.bin | 
|  | ARUBA | HD7400D/G, HD7500D/G, HD7600D/G, HD7660D, HD8310G, HD8410G-HD8670G | radeon/ARUBA\_me.bin radeon/ARUBA\_pfp.bin radeon/ARUBA\_rlc.bin radeon/TAHITI\_uvd.bin radeon/TAHITI\_vce.bin | 
| Southern Islands | CAPE VERDE | HD7750, HD7770, HD7870M, R7 250, R7 250X, R9 370X | radeon/verde\_ce.bin radeon/verde\_mc.bin radeon/verde\_me.bin radeon/verde\_pfp.bin radeon/verde\_rlc.bin radeon/verde\_smc.bin radeon/TAHITI\_uvd.bin radeon/TAHITI\_vce.bin | 
|  | PITCAIRN | HD7800, R9 270X | radeon/pitcairn\_ce.bin radeon/pitcairn\_mc.bin radeon/pitcairn\_me.bin radeon/pitcairn\_pfp.bin radeon/pitcairn\_rlc.bin radeon/pitcairn\_smc.bin radeon/pitcairn\_k\_smc.bin radeon/TAHITI\_uvd.bin radeon/TAHITI\_vce.bin | 
|  | TAHITI | HD7870 XT, HD7900, R9 280X | radeon/tahiti\_ce.bin radeon/tahiti\_mc.bin radeon/tahiti\_me.bin radeon/tahiti\_pfp.bin radeon/tahiti\_rlc.bin radeon/tahiti\_smc.bin radeon/TAHITI\_uvd.bin radeon/TAHITI\_vce.bin | 
|  | OLAND | HD8550M-HD8790M, R7 240 | radeon/oland\_ce.bin radeon/oland\_mc.bin radeon/oland\_me.bin radeon/oland\_pfp.bin radeon/oland\_rlc.bin radeon/oland\_smc.bin radeon/TAHITI\_uvd.bin | 
|  | HAINAN | HD8970M | radeon/hainan\_ce.bin radeon/hainan\_mc.bin radeon/hainan\_me.bin radeon/hainan\_pfp.bin radeon/hainan\_rlc.bin radeon/hainan\_smc.bin radeon/TAHITI\_uvd.bin | 
| Sea Islands | BONAIRE | HD7790, R7 260, R7 260X | radeon/bonaire\_ce.bin radeon/bonaire\_mc.bin radeon/bonaire\_me.bin radeon/bonaire\_mec.bin radeon/bonaire\_pfp.bin radeon/bonaire\_rlc.bin radeon/bonaire\_sdma.bin radeon/bonaire\_smc.bin radeon/BONAIRE\_uvd.bin radeon/BONAIRE\_vce.bin | 
|  | KABINI | HD8180-HD8400 | radeon/kabini\_ce.bin radeon/kabini\_me.bin radeon/kabini\_mec.bin radeon/kabini\_pfp.bin radeon/kabini\_rlc.bin radeon/kabini\_sdma.bin radeon/BONAIRE\_uvd.bin radeon/BONAIRE\_vce.bin | 
|  | KAVERI |  | radeon/kaveri\_ce.bin radeon/kaveri\_me.bin radeon/kaveri\_mec2.bin radeon/kaveri\_mec.bin radeon/kaveri\_pfp.bin radeon/kaveri\_rlc.bin radeon/kaveri\_sdma.bin radeon/BONAIRE\_uvd.bin radeon/BONAIRE\_vce.bin | 
|  | HAWAII | R9 290, R9 290X | radeon/hawaii\_ce.bin radeon/hawaii\_mc.bin radeon/hawaii\_me.bin radeon/hawaii\_mec.bin radeon/hawaii\_pfp.bin radeon/hawaii\_rlc.bin radeon/hawaii\_sdma.bin radeon/hawaii\_smc.bin radeon/BONAIRE\_uvd.bin radeon/BONAIRE\_vce.bin | 
|  | MULLINS |  | radeon/mullins\_ce.bin radeon/mullins\_me.bin radeon/mullins\_mec.bin radeon/mullins\_pfp.bin radeon/mullins\_rlc.bin radeon/mullins\_sdma.bin radeon/BONAIRE\_uvd.bin radeon/BONAIRE\_vce.bin | 

### USE flags


### Emerge

Portage uses the `VIDEO_CARDS` variable for enabling support for various graphics cards. Setting the `VIDEO_CARDS` variable to `-* radeon` (see the [feature matrix above](https://wiki.gentoo.org#Feature_support)) then asking Portage to rebuild the [@world set](https://wiki.gentoo.org/wiki/Package_sets#System_defining_sets) will pull in the correct driver for older radeon cards:

**`/etc/portage/package.use/00video`**

**Basic example**

```
 VIDEO_CARDS: -* radeon
```
**`/etc/portage/package.use/00video`**

**R270 example**

```
 VIDEO_CARDS: -* radeon radeonsi
```
After the values have been ask Portage to rebuild the changed USE flags in the @world set so the /etc/portage/make.conf change takes effect:

`root #``emerge --ask --changed-use --deep @world`
## Configuration

### Permissions

If the [`acl`](https://packages.gentoo.org/useflags/acl) USE flag is enabled globally and [`elogind`](https://packages.gentoo.org/useflags/elogind) is being used (default for desktop profiles) permissions to video cards will be handled automatically. It is possible to check the permissions using getfacl:

`user $``getfacl /dev/dri/card0 | grep larry``user:`**larry**:rw-
A broader solution is to add the user(s) needing access the video card to the video group:

`root #``gpasswd -a larry video`
Note that users will be able to run X without permission to the DRI subsystem, but hardware acceleration will be disabled.

### xorg.conf

The [X server](https://wiki.gentoo.org/wiki/X_server) is designed to work out-of-the-box, with no need to manually edit X.Org's configuration files. It should detect and configure devices such as displays, keyboards, and mice.

However, the main configuration file of the X server is [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf).

The X server can be forced to use desired driver with:

**`/etc/X11/xorg.conf.d/radeon.conf`**

**Explicit radeon driver section**

```
Section "Device"
   Identifier  "radeon"
   Driver      "radeon"
EndSection
```
## Advanced configuration

### Power management

Power management can be set in the [sysfs](https://wiki.gentoo.org/wiki/Sysfs) filesystem as follows:

- Check the current power method:

- `root #``cat /sys/class/drm/card0/device/power_method`

- Change the power method:

- The "dynpm" method dynamically changes the clocks based on demand. (not effective as of June 27, 2012)
- `root #``echo "dynpm" > /sys/class/drm/card0/device/power_method`
- The "profile" method lets you set a profile on how the card should behave.
- `root #``echo "profile" > /sys/class/drm/card0/device/power_method`

- Check the current profile:

- `root #``cat /sys/class/drm/card0/device/power_profile`

- Change the profile:

- Options for profile:
  1. "default" no change of clock speeds
  2. "auto" switches between "mid" and "high" power states based on the whether the system is on battery power or not. The "low" power state are selected when the monitors are in the dpms off state.
  3. "low" forces the GPU to be in the low power state all the time. Note that "low" can cause display problems on some laptops; this is why auto does not use "low" when displays are active.
  4. "mid" forces the GPU to be in the "mid" power state all the time. The "low" power state is selected when the monitors are in the dpms off state.
  5. "high" forces the GPU to be in the "high" power state all the time. The "low" power state is selected when the monitors are in the dpms off state.
- `root #``echo "low" > /sys/class/drm/card0/device/power_profile`

- Check the current GPU, Memory clocks and voltage (needs to have kernel debugfs enabled):

- `root #``cat /sys/kernel/debug/dri/0/radeon_pm_info`

##### Power Management with Linux Kernel 3.11, 3.12

Linux Kernel 3.11 introduces improved power management for some radeon cards, to activate it pass the parameter `radeon.dpm=1` to the kernel.

- Check the dynamic power management state

- `root #``cat /sys/class/drm/card0/device/power_dpm_state`

- Change the dynamic power management state

- `root #``echo "performance" > /sys/class/drm/card0/device/power_dpm_state`

Other valid options include "battery" and "balanced".

In order to verify that the new dynamic power management code is active check radeon\_pm\_info (see above), it should say something like:

- uvd vclk: 0 dclk: 0
- power level 0 sclk: 25000 mclk: 15000 vddc: 900 vddci: 950

##### Power Management with Linux Kernel >= 3.13

Linux Kernel 3.13 [enabled dynamic power managment by default](http://kernelnewbies.org/Linux_3.13#head-f95c198f6fdc7defe36f470dc8369cf0e16898df) for a lot of video cards.

- Check the current GPU, Memory clocks and voltage (needs to have kernel debugfs enabled):

- `root #``cat /sys/kernel/debug/dri/0/radeon_pm_info`

### Power consumption when using multi-head / multi-monitor

When using more than one monitor at once, the power consumption of the graphics card increases up to additional 150% relative to its idle power consumption, because the memory clock switches to full speed (see above). This behavior causes a system using a Radeon HD6870 card to consume around 30 W of additional power when running in multi-monitor mode than in single-monitor mode. This behavior can be avoided by using power method "profile" instead of "dpm" and by forcing "profile" to "low".

Using power method "dpm" and setting a low power profile (i.e. "balanced") is sure possible but useless in this case. The "dpm" method does not allow to forcibly remain on a certain power preset, because it uses the internal GPU hardware to dynamically change the clocks and voltages depending on the current GPU load. As a result, the power consumption increases as soon as you activate a second monitor (because the GPU hardware wants it so).

- Deactivate dpm first (Kernel >= 3.13) with an appropriate kernel commandline via GRUB at boottime:

- FILE**`/boot/grub/grub.cfg`**

- After rebooting the system to desktop, force the power profile to "low". (This can also be automated by creating a /etc/local.d/\* script):

- `root #``echo "low" > /sys/class/drm/card0/device/power_profile`

Limitation: For my KDE/Plasma desktop environment this procedure will only work when the power profile level is assigned *after* KDE/Plasma has started. An automated command in /etc/local.d/\*.start is not sufficient. The memory clock rises and remains high (like normal behavior).

Solution: Let KDE/Plasma do the job with its autostart feature.

- Allow the user to change the power profile by giving him the appropriate privilege when booting. Create a file like the following:

- FILE**`/etc/local.d/pm.start`**

- Add a startscript to KDE5/Plasma autostart:

- FILE**`~/.config/autostart/pm.sh`**

- Do not forget to make it executable:

- `user $` `chmod +x ~/.config/autostart/pm.sh`

You can check the result via kernel debugfs (see above) and it should be similar:

`root #``cat /sys/kernel/debug/dri/0/radeon_pm_info`
default engine clock: 900000 kHz
current engine clock: 99990 kHz
default memory clock: 1050000 kHz
current memory clock: 150000 kHz
voltage: 950 mV
PCIE lanes: 16

Benefit: After these changes, you may use as many monitors as possible without an increased power consumption of your system. Despite the fact that the GPU memory clock is constantly reduced there are still enough graphic resources left to render desktop effects, to play HD movies (also with `USE="vaapi"`) and to do all the regular stuff to get your work done.

I recommend using two entries in GRUB (with and without DPM) and creating a startscript based on the value of /sys/module/radeon/parameters/dpm (0 = DPM off, 1 = DPM on), in order to automatically adjust the power management, based on the decision at boottime (single-head with DPM vs. multi-head without DPM).

### Tuning

| Parameter | Application | Effect | Comment | 
|---|---|---|---|
| *radeon.pcie\_gen2=1* | kernel commandline | Run at PCI-E 2.0 speeds per default | Enabled by default since kernel 3.6 | 
| *radeon.audio=1* | kernel commandline | Enable HDMI audio on some cards | Enabled by default since kernel 3.13 | 
| *radeon.dpm=1* | kernel commandline | Enable dynamic power management on some cards <sup>1</sup> | Enabled by default since kernel 3.13 | 
| *R600\_STREAMOUT=1* | Environment variable | Enable the use of OpenGL 3.0/4.2 on some cards | Enabled by default since Mesa 8.0 | 
| *R600\_HYPERZ=1* | Environment variable | Enable the use Hyper-Z on some cards | See Bugs: [https://bugs.freedesktop.org/show\_bug.cgi?id=75112](https://bugs.freedesktop.org/show_bug.cgi?id=75112) | 
| *R600\_TILING=1* | Environment variable | Enable 2D Tiling on some cards (also needs the xorg.conf parameter set) | This option is probably activated by default, see *man radeon* | 
| *R600\_SURF=1* | Environment variable | Enable 2D Tiling on some cards (also needs the xorg.conf parameter set) |  | 
| *R600\_GLSL130=1* | Environment variable | Enable more features of the OpenGL 3.0 API | Enabled by default since Mesa 8.0 | 
| *ColorTiling2D True* | xorg.conf parameter | Enable 2D Color Tiling in conjunction with R600\_TILING=1 and R600\_SURF=1 | This option is probably activated by default, see *man radeon* | 

Further driver parameters can be found via the command modinfo radeon. They are listed next to *parm:*. Add the prefix `radeon.` to use them (i.e. `radeon.backlight=1`).

##### Tuning with mesa from git

Mesa from git (=mesa-9999) offers a different way to tune the 3D driver. Use the environment variable R600\_DEBUG with the following options

| Parameter | Effect | 
|---|---|
| *help* | List all of the configuration options | 

See [mesa commit](http://cgit.freedesktop.org/mesa/mesa/commit/?id=4bf0ebdd4fc8dbcab9333ff8805af35a91e6848b) for more information.

- Kernel parameters can be just added to the kernel commandline in grub.cfg or grub.conf.
- Environment variables could be put into a file like /etc/env.d/99radeon to have them initialized during boot.
- xorg.conf parameter are usual in the *Device* section for the card.
- A full list of kernel parameters can be found here: [X.Org Wiki - RadeonFeature](http://wiki.x.org/wiki/RadeonFeature#Linux_kernel_parameters)
- S3TC compression needed for some applications like most 3D games: [media-libs/libtxc\_dxtn](https://packages.gentoo.org/packages/media-libs/libtxc_dxtn)

  - 1\) supported by Linux kernel 3.11

### Monitoring

[lm sensors](https://wiki.gentoo.org/wiki/Lm_sensors) can be used to monitor the cards temperature. It uses the I2C interface, which needs to be enabled in the kernel:

**Including radeon firmware**

### Audio over HDMI

Audio through the HDMI port is available for some cards. Check the [X.Org Wiki - Radeon Feature Matrix](http://wiki.x.org/wiki/RadeonFeature#Feature_Matrix_for_Free_Radeon_Drivers) for the model family. A recent 3.x kernel may be needed.

If you are using a kernel older than 3.13, HDMI audio must be explicitly enabled using the kernel commandline paramater *radeon.audio=1*. In addition, [ALSA](https://wiki.gentoo.org/wiki/ALSA) typically does not use HDMI as the default audio, so one way to force this as the default is to add a config file:

**`~/.asoundrc`**

which may be moved to /etc/asound.conf to make HDMI audio the system-wide default.

Another solution is to enable the `pulseaudio` USE flag, update the system and use pavucontrol to set the HDMI port as fallback (or use pavucontrol to change manually which application uses which audio port).

#### Multichannel LPCM

If you want to enjoy full 5.1 HDMI sound on your Gentoo rig try out anssi's HDMI audio patch as mentioned in the phoronix forums.

`root #````
cd /usr/src/linux
```
`root #````
patch -p1 < atihdmi5.patch
```
`root #````
make && make modules_install && make install
```
Check the bootloader config to use the correct kernel and reboot.

### mesa-amber for r100 and r200

With the introduction of Mesa 22, the classic driver was dropped from the main library<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. One way to restore acceleration is to use [media-libs/mesa-amber](https://packages.gentoo.org/packages/media-libs/mesa-amber).

First, unless there is another Radeon video card in the system (which is unlikely on such an old setup), disable `llvm` and `video_cards_radeon` USE flags in package.use for [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa):

**`/etc/portage/package.use/mesa-amber`**

Then, install mesa-amber:

`root #``emerge --ask media-libs/mesa-amber`
This may or may not need further configuration.

## Troubleshooting

### Debug tools

It might be helpful to install the package [x11-apps/mesa-progs](https://packages.gentoo.org/packages/x11-apps/mesa-progs), which provides the packages glxgears and glxinfo.

### Glamor does not load

If you see errors like "glamor detected, failed to initialize EGL.", then try enabling USE="gbm egl gles2 llvm" in the mesa builds.

If you see errors like "Failed to link: error: fragment shader lacks \`main'", then make sure the glamor package has been built with USE="-gles".

On cards that are supported by radeonsi (starting at Southern Island, see the [feature matrix](https://wiki.gentoo.org#Feature_support)) make sure to not only specify "radeon" but also "radeonsi" as the [driver](https://wiki.gentoo.org#Driver).

### Poor X server performance

If graphic performance (such as playing videos) is terrible, then make sure KMS Color Tiling is enabled. You can see this in your Xorg log:

\[  3407.235\] (II) RADEON(0): KMS Color Tiling: enabled
\[  3407.235\] (II) RADEON(0): KMS Color Tiling 2D: enabled
\[  3407.235\] (II) RADEON(0): KMS Pageflipping: enabled

If you see "no" instead of "enabled", then you'll have to look earlier in the log to see why it's been disabled. If glamor failed to load, see the previous troubleshooting item.

### Trouble with integrated graphics (A8 or similar)

When having trouble getting an on chip integrated graphic core to work (strange visuals/black screen) the solution may be found in the motherboard's BIOS settings.

In some cases it appears the 'auto' settings don't work correctly, so make sure to explicitly enable the integrated graphics. In my case (MSI A88XM-E45 motherboard):

Settings > Advanced > Integrated Graphics Configuration > Initiate Graphics Devices - I changed this from 'auto' to 'Dual graphics'

Settings > Advanced > Integrated Graphics Configuration > Initiate Graphics Shared Memory - This then appeared which I gave a setting

After this all problems cleared up.

### radeon\_drv.so undefined symbol exaGetPixmapDriverPrivate

<u>Problem:</u>

X refuses to start.

<u>Error Message:</u>

symbol lookup error /usr/lib64/xorg/modules/drivers/radeon\_drv.so undefined symbol exaGetPixmapDriverPrivate ...

xinit unable to connect to X server : Bad file descriptor.

<u>Solution:</u>

Reconfigure the Kernel, making ATI Radeon (CONFIG\_DRM\_RADEON) and Direct Rendering (CONFIG\_DRM) load as a Module \[M\], as opposed to a built-in \[\*\].

### Bug trackers

- [Gentoo bugtracker: known bugs](https://bugs.gentoo.org/buglist.cgi?quicksearch=xf86-video-ati&order=bug_id%20DESC)
- [Freedesktop.org bugtracker: known bugs](https://bugs.freedesktop.org/buglist.cgi?bug_status=UNCONFIRMED&bug_status=NEW&bug_status=CONFIRMED&bug_status=ASSIGNED&bug_status=REOPENED&bug_status=NEEDINFO&bug_status=PLEASETEST&bug_status=IN_PROGRESS&product=xorg&component=Driver%2FRadeon&order=bug_id%20DESC)
- [Freedesktop.org bugtracker: known bugs](https://bugs.freedesktop.org/buglist.cgi?bug_status=UNCONFIRMED&bug_status=NEW&bug_status=CONFIRMED&bug_status=ASSIGNED&bug_status=REOPENED&bug_status=NEEDINFO&bug_status=PLEASETEST&bug_status=IN_PROGRESS&product=DRI&component=DRM%2FRadeon&order=bug_id%20DESC)



## See also

- [ATI FAQ](https://wiki.gentoo.org/wiki/ATI_FAQ) — Frequently Asked Questions (FAQ) to help users avoid some common installation and configuration issues related to DRI and X11 for AMD/ATI boards.
- [Amdgpu](https://wiki.gentoo.org/wiki/Amdgpu) — the open source graphics drivers for AMD Radeon and other GPUs.
- [Amdgpu-pro](https://wiki.gentoo.org/wiki/Amdgpu-pro) — the next generation *closed source* graphics component that operates on top of the open source [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) drivers for newer AMD/ATI Radeon graphics cards.
- [Fglrx](https://wiki.gentoo.org/wiki/Fglrx) — the proprietary graphics driver for *older* AMD/ATI graphic cards.
- [fglrx Quick Switch](https://wiki.gentoo.org/wiki/Fglrx_Quick_Switch) - Quickly switch between [fglrx](https://wiki.gentoo.org/wiki/Fglrx) and this driver using GRUB 2 without downgrading xorg-server.
- [Hprofile](https://wiki.gentoo.org/wiki/Hprofile) — an application that can be used to manage multiple profiles be it hardware or software.

## External resources

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Mesa 20.0.0 Release Notes / 2020-02-19](https://mesa3d.org/relnotes/20.0.0.html)
2. [↑](https://wiki.gentoo.org#cite_ref-2) Phoronix - [More Improvements To Old AMD GPU Support On Linux Are Planned For 2026](https://www.phoronix.com/news/Timur-More-Old-AMDGPU-2026)
3. [↑](https://wiki.gentoo.org#cite_ref-3) [Mesa 22.0.0 Release Notes / 2022-03-09](https://docs.mesa3d.org/relnotes/22.0.0.html), mesa3d.org. Retrieved on October 19, 2022
