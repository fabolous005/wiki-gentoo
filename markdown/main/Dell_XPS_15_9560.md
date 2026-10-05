<!-- source: https://wiki.gentoo.org/wiki/Dell_XPS_15_9560 | group: Gentoo Wiki (Main) | wiki-title: Dell XPS 15 9560 -->
---
title: Dell XPS 15 9560
url: https://wiki.gentoo.org/wiki/Dell_XPS_15_9560
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: bdd0985f59a211c9
license: CC BY-SA 4.0
---

# Dell XPS 15 9560

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

### Standard

| Device | Make/model | Status | Kernel driver(s) | Kernel version | 
|---|---|---|---|---|
| CPU | Intel(R) Core(TM) i7-7700HQ CPU @ 2.80GHz |  |  | 4.12.5 | 
| Memory | 16GB DDR4-2400MHz |  |  |  | 
| Hard disk | 512GB PCIe Solid State Drive |  | nvme |  | 
| Video card | NVIDIA Corporation GP107M GeForce GTX 1050 Mobile (4GB GDDR5) |  | nvidia, fbsimple | 4.14.8 | 
| Video card | Intel Corporation Device 591b (rev 04) |  | i915 | 4.13 | 
| Wireless | Killer 1535 802.11ac 2x2 WiFi ( [Qualcomm Atheros QCA6174](https://wiki.gentoo.org/wiki/Qualcomm_Atheros_QCA6174)) |  | ath10k\_core ath10k\_pci linux-firmware |  | 
| Touchscreen | ELAN Touchscreen |  | usbhid hid\_multitouch | 4.15.4 | 
| Touchpad | [Synaptics](https://wiki.gentoo.org/wiki/Synaptics) TouchPad |  | mouse\_ps2\_synaptics\_smbus | 4.13.0 | 
| Bluetooth | Killer 1535 Bluetooth |  | bluetooth btrtl btintel bnep btbcm rfcomm btusb linux-firmware | 4.15.4 | 
| USB 3.0 |  |  | xhci\_hcd |  | 
| Thunderbolt 3 | 2 lanes of PCI Express Gen 3. Supports: Power In / Charging, PowerShare, 40Gbps Bi-Directional, 3.1 USB Gen 2 (10Gbps), VGA, HDMI, Ethernet and USB-A via Dell Adapter (Sold Separately) |  | ? |  | 
| SD Card Reader | SD, SDHC, SDXC |  | ? |  | 
| Webcam | Widescreen HD (720p) |  | uvc | 4.14.8 | 
| Microphone | Dual array digital microphones |  | ? |  | 
| Fingerprint reader | 138a:0091 Validity Sensors, Inc. |  | None (see below) |  | 

Regarding the unsupported fingerprint reader, according to arch wiki, "The fingerprint reader is a Validity/Synaptics model with USB id 138a:0090. There currently is no Linux driver but an open source Linux driver is being developed by reverse engineering the Windows driver.". This implies some or earlier versions have the 138a:0090 version, which a driver is now functional for, however mine has the 138a:0091 version, which is unsupported. See [driver development github repository](https://github.com/nmikhailov/Validity90) for further information.

### Accessories

Some models have touch screens. Some models are 2-in-1 (break apart). I tested on a conventional (non break apart) model with touch screen, however the touch screen has not been tested. A dock exists however I have never seen it and wouldn't personally make use of it. Other reports have described docks in this series as functional, however.

### Firmware

BIOS version on receipt was `1.3.4` with `ePSA Build 4304.17 UEFI ROM`.

We need the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package to operate the wireless chipset and to provide firmware to upload to the Intel graphics controller to enable things like proper power management.

`root #``emerge linux-firmware`
## Configuration

### package.use

We want to enable a few things in {{Path|/etc/portage/package.use} ...

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* nvidia
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev wacom libinput synaptics
```
### Kernel

**Input support**

**NVMe support**

**Wireless support**

**Real Time Clock support**

**ACPI button support**

**Graphics support**

### Touchpad

[Synaptics](https://wiki.gentoo.org/wiki/Synaptics) touch pad.

`root #``emerge --ask xf86-input-synaptics`
You can tune this with a tool, see [the arch linux page on synaptics touchpad](https://wiki.archlinux.org/index.php/Touchpad_Synaptics) for more details.

### Bumblebee / Primus

Hybrid Graphics (GPU Switching) is available on the XPS 15 9560. Follow the [Gentoo Bumblebee Wiki](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee) guide to installing it.

## Troubleshooting

### Slow 2D graphics

According to [this page](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers#Getting_2D_acceleration_to_work_on_machines_with_4GB_memory_or_more) slow 2D performance attributed to a BIOS setting can be identified via:

`root #``cat /proc/mtrr`
reg00: base=0x00000000 ( 2048MB), size= 2048MB, count=1, uncachable
...

If any line contains the word "uncachable" apparently you need to reboot, enter BIOS, and change an MTRR setting from 'continuous' to 'discrete'. However, on my machine while this is certainly the case, I cannot find such an option in the BIOS.

### X11 fails to start with "No Screens Found"

This can be because you have enabled the `efifb` EFI Framebuffer Driver in the kernel. Disable it. Only `CONFIG_FB_SIMPLE=y` (Simple Framebuffer) is OK to leave enabled!

### Crash on X11 startup

This can occur you have the `nouveau` driver enabled. You can work around it by adding `nomodeset` to the kernel command line.

### Excessive CPU Throttling

When the CPU runs continuously at 100% (say, for instance when emerging larger packages), it can become quite hot. When it crosses certain temperature thresholds, it throttles down the CPU frequency, which in turn makes it run cooler for a while, but at a vastly lower clock frequency. Dell XPS 15:s (and other XPS models) have historically have not had enough airflow to cool the CPU in its default configuration (or the discrete GPU for that matter).

Several workarounds have been attempted with modding the case with cooling pads, tape, better thermal paste, etc. One easy thing to try first is to adjust the voltage of the CPU (and/or the GPU) with the 'sys-power/intel-undervolt' package.

In the settings in `/etc/intel-undervolt.conf`, try something like this:

undervolt 0 'CPU' -125
   undervolt 1 'GPU' -75
   undervolt 2 'CPU Cache' -125
   undervolt 3 'System Agent' -75
   undervolt 4 'Analog I/O' 0

Due to a lot of factors depending on your particular machine, you might still be able to get a stable system with even lower figures, or you might have to raise them a bit. It did make a huge difference in temperature, and the CPU did not throttle anymore after these settings.

## See also

- Closed source [NVIDIA drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) and open source [Nouveau](https://wiki.gentoo.org/wiki/Nouveau) drivers and [how to switch between them](https://wiki.gentoo.org/wiki/Nouveau_%26_nvidia-drivers_switching).
- [Qualcomm Atheros QCA6174](https://wiki.gentoo.org/wiki/Qualcomm_Atheros_QCA6174) and [Gentoo AMD64 Handbook Wireless Networking](https://wiki.gentoo.org/wiki/Handbook:AMD64/Networking/Wireless)
