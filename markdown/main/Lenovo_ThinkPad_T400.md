<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T400 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad T400 -->
---
title: Lenovo ThinkPad T400
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T400
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "3748056fdfaa10eb"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad T400

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A ThinkPad with [libreboot support](https://libreboot.org/docs/install/t400_external.html).

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel(R) Core(TM)2 Duo CPU P8400 @ 2.26GHz |  | N/A | N/A | 4.0.5 | N/A | 
| Video Card | Intel Corporation Mobile 4 Series Chipset Integrated Graphics Controller (rev 07) |  | 00:02.1 | i915 | 4.0.5 | N/A | 
| Ethernet controller | Intel Corporation 82567LM Gigabit Network Connection (rev 03) |  | 00:19.0 | e1000e | 4.0.5 | N/A | 
| Audio device | Intel Corporation 82801I (ICH9 Family) HD Audio Controller (rev 03) |  | 00:1b.0 | snd\_hda\_intel | 4.0.5 | N/A | 
| Network controller | Intel Corporation PRO/Wireless 5100 AGN \[Shiloh\] Network Connection |  | 03:00.0 | iwlwifi | 4.0.5 | N/A | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Dock | ThinkPad Pro Dock |  | N/A | N/A | N/A | N/A | 

## Installation

### Emerge

Install the ThinkPad SMAPI BIOS driver

`root #``emerge --ask app-laptop/tp_smapi`
## Configuration

### package.use

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i965
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: libinput
```
**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: mmx mmxext sse sse2 sse3 sse4_1 ssse3
```
### Synaptics Touchpad

**`/etc/X11/xorg.conf.d/20-touchpad.conf`**

**Synaptics enhanced configuration**

### Video

For detailed graphics card configuration follow the [Intel](https://wiki.gentoo.org/wiki/Intel) wiki article. The video card can be found out following way:

`root #````
lspci -nn |grep -i vga
```
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Mobile 4 Series Chipset Integrated Graphics Controller \[8086:2a42\] (rev 07)

VendorId: 8086
DeviceId: 2a42

The VendorID: 8086 says it is a Intel Chipset, and DeviceID 2a42 defines the VGA Controller as a GMA 4500MHD graphics, and a GM45 chipset.

Showing video card using [x11-apps/igt-gpu-tools](https://packages.gentoo.org/packages/x11-apps/igt-gpu-tools)

`root #``intel_gpu_top````
intel-gpu-top: Intel Cantiga (Gen4) @ /dev/dri/card0 - ----/---- MHz; ---% RC6
          0 irqs/s
   
         ENGINES     BUSY                                     MI_SEMA MI_WAIT
       Render/3D   30.51% |██████████▏                      |    ---%      0%
           Video    0.00% |                                 |    ---%      0%
```
## See also

## External resources

- [https://wiki.archlinux.org/index.php/Lenovo\_ThinkPad\_T400](https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_T400)
- [http://www.thinkwiki.org/wiki/Install\_Gentoo\_on\_a\_Thinkpad\_T400](http://www.thinkwiki.org/wiki/Install_Gentoo_on_a_Thinkpad_T400)
- [http://www.thinkwiki.org/wiki/Category:Gentoo](http://www.thinkwiki.org/wiki/Category:Gentoo)
