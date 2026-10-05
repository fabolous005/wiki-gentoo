<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_P53 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad P53 -->
---
title: Lenovo ThinkPad P53
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_P53
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "503ad7f5fa2107f"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad P53

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

Installing Linux to Thinkpads is mostly easy. Even more so, since Lenovo started official support for Linux on a number of systems.[\[1\]](https://wiki.gentoo.org#cite_note-1)

The installation using a Gentoo installation media works perfectly as described [in the Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation).

Almost all of the P53 hardware is supported in kernel or by installing additional drivers from Portage. Due to the muxed GPU hardware and the decent IOMMU layout of the P53, this laptop is also a good candidate for advanced configurations like GPU passthrough in QEMU.

## Hardware

### System Model Verfication

`root #``dmidecode -s system-version`
ThinkPad P53

### Lenovo ThinkPad P53

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Notes | 
|---|---|---|---|---|---|
| CPU | Intel i7-9750H series |  | N/A | mcore2 | None | 
| Ethernet | Intel I219-V |  | 00:1f.6 | e1000e | None | 
| WiFi | Intel Wi-Fi 6 AX200 |  | 52:00.0 | iwlwifi | None | 
| Bluetooth | Intel Wireless Bluetooth |  | N/A | btusb | None | 
| Touchpad | SynPS/2 Synaptics TouchPad |  | N/A | synaptics | None | 
| SD Card Reader | RTS525A |  | 54:00.0 | rtsx\_pci | None | 
| Integrated Video card | Intel UHD 630 |  | 00:02.0 | intel i965 | None | 
| Second Video card | NVIDIA Quadro T2000M |  | 01:00.0 | N/A | Needs Nvidia drivers | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Notes | 
|---|---|---|---|---|---|
| Docking Station | Thinkpad Thunderbold 3 Dock |  | N/A | hotplug\_pci\_acpi | Using LAN over Thunderbold does not work great all the time. | 

## Configuration

### CPU

It is recommended to enable the microcode update support as explained in [Intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode).

**Kernel 5.10.52 (gentoo-sources)**

### Power

**Kernel 5.10.52 (gentoo-sources)**

### Hardware monitoring

**Kernel 5.10.52 (gentoo-sources)**

### Networking

**Kernel 5.10.52 (gentoo-sources)**

### Bluetooth

**Kernel 5.10.52 (gentoo-sources)**

It is recommended to globally enable the `bluetooth` USE Flag in order to make use of this feature it in your system and software.

### Storage

**Kernel 5.10.52 (gentoo-sources)**

### Graphics

**Kernel 5.10.52 (gentoo-sources)**

#### Intel Firmware

You may also want to include the propretary DMC firmware for the GPU.

**Kernel 5.10.52 (gentoo-sources)**

#### Nvidia Drivers

In order to make use of the more powerful Nvidia card, you'll need to install the propretary drivers.

`root #``emerge nvidia-drivers`
## See also

- [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) — The [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) package contains the *proprietary* graphics driver for [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) graphic cards.
- [NVIDIA/Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus) — a proprietary technology that seamlessly switches between two GPUs.
- [NVIDIA/Bumblebee](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee) — an open source implementation of [NVIDIA Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus).
- [Lenovo ThinkPad P50](https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_P50)
- [Lenovo ThinkPad P52](https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_P52)
