<!-- source: https://wiki.gentoo.org/wiki/HP_Elitebook_845_G10 | group: Gentoo Wiki (Main) | wiki-title: HP Elitebook 845 G10 -->
---
title: HP Elitebook 845 G10
url: https://wiki.gentoo.org/wiki/HP_Elitebook_845_G10
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-11"
fingerprint: cf407a0fc0f2396a
license: CC BY-SA 4.0
---

# HP Elitebook 845 G10

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Video card | Advanced Micro Devices, Inc. \[AMD/ATI\] Phoenix1 (rev d3) Radeon™ 780M Graphics |  | \[1002:15bf\] | amdgpu | 6.4.11 | Require amdgpu.sg\_display=0 kernel parameter if kernel lower than 6.4.11. | 
| Audio card | Advanced Micro Devices, Inc. \[AMD/ATI\] Rembrandt Radeon High Definition Audio Controller |  | \[1002:1640\] | snd\_hda\_intel | 6.4.11 | — | 
| Audio card | Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h HD Audio Controller |  | \[1022:15e3\] | snd\_hda\_intel | 6.4.11 | — | 
| Audio Coprocessor | Advanced Micro Devices, Inc. \[AMD\] ACP/ACP3X/ACP6x Audio Coprocessor |  | \[1022:15e2\] | snd\_acp\_pci,snd\_pci\_ps | 6.4.11 | — | 
| Network controller | MEDIATEK Corp. MT7922 802.11ax PCI Express Wireless Network Adapter |  | \[14c3:0616\] | mt7921e | 6.4.11 | Required sys-kernel/linux-firmware | 
| Bluetooth | MEDIATEK Corp. MT7922 Bluetooth Adapter |  | \[0489:e0f2\] | btusb | 6.4.11 | Required sys-kernel/linux-firmware | 
| Web Camera | Quanta Computer, Inc. HP 5MP Camera |  | \[0408:545f\] | uvcvideo | 6.4.11 | Both infrared and normal cameras work | 
| Fingerprint Reader | Synaptics, Inc. |  | [06cb:00f0](https://linux-hardware.org/?id=usb:06cb-00f0) |  | 6.4.11 | — | 
| Touchpad | Elantech I2C HID Touchpad |  | [04F3:31EC](https://linux-hardware.org/?view=search&vendorid=04f3&deviceid=31ec#list) | i2c\_designware\_platform, i2c\_hid, hid\_generic, hid\_multitouch | 6.4.11 | — | 
| Speaker | Cirrus Logic CS35L41(CSC3551) audio amplifier |  | \[???\] | serial\_multi\_instantiate, i2c\_designware\_platform, snd\_hda\_scodec\_cs35l41\_i2c | 6.4.11 | Required sys-kernel/linux-firmware | 
| LEDs on the buttons | ??? |  | ??? | ??? | 6.4.11 | The kernel version must be greater than or equal to 6.4.6. | 

## Installation

### Firmware

The [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package is required for the AMDGPU graphics, MediaTek MT7922 wireless and Bluetooth adapters, and Cirrus Logic CS35L41 speaker amplifiers.

### Kernel

The following hardware-specific configuration was extracted from a known-working [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) 7.0.13 kernel on this laptop. Generic Gentoo, boot loader, root filesystem, networking, and userspace requirements are intentionally omitted. Options may move or be renamed between kernel releases; search for the shown `CONFIG_*` symbol in `menuconfig` if the menu path differs.

**Enable AMD processor, ACPI, and power management support**

**Enable PCIe, NVMe, firmware, and AMD IOMMU support**

**Enable Radeon 780M graphics and the display console**

**Enable MediaTek MT7922 Wi-Fi and Bluetooth**

**Enable the I2C touchpad, keyboard input, and sensors**

**Enable the USB controllers, webcam, USB-C, and USB4**

**Enable the AMD audio coprocessor, HDA codecs, and CS35L41 amplifiers**

**Enable S2idle and laptop platform support**

The EliteBook 845 G10 firmware does not expose classic ACPI S3 sleep. The tested configuration uses S2idle, for which `CONFIG_SUSPEND` and `CONFIG_AMD_PMC` are important. Verify the active mode with:

`root #``cat /sys/power/mem_sleep`
\[s2idle\]

## Troubleshooting

### Screen flickering white

Check kernel version, should above or equal 6.4.11

See [AMDGPU#Flickering\_and\_white\_screens](https://wiki.gentoo.org/wiki/AMDGPU#Flickering_and_white_screens) for resolution.

### ELAN I2C touch pad not working

Make sure CONFIG\_PINCTRL\_AMD, CONFIG\_I2C\_HID, CONFIG\_I2C\_DESIGNWARE\_PLATFORM and CONFIG\_HID\_MULTITOUCH is set.

AMDI0010 I2C should be in dmesg.

```
[    1.724985] input: ELAN07A8:00 04F3:31EC Mouse as /devices/platform/AMDI0010:00/i2c-0/i2c-ELAN07A8:00/0018:04F3:31EC.0001/input/input6
```

```
[    1.725046] input: ELAN07A8:00 04F3:31EC Touchpad as /devices/platform/AMDI0010:00/i2c-0/i2c-ELAN07A8:00/0018:04F3:31EC.0001/input/input8
```

[https://forums.gentoo.org/viewtopic-t-1109820-start-0.html](https://forums.gentoo.org/viewtopic-t-1109820-start-0.html)
[https://patchwork.kernel.org/project/linux-acpi/patch/1457609692-25903-1-git-send-email-Xiangliang.Yu@amd.com/](https://patchwork.kernel.org/project/linux-acpi/patch/1457609692-25903-1-git-send-email-Xiangliang.Yu@amd.com/)

### PCIe Bus Error for nvme

PCIe Bus Error: severity=Corrected, type=Physical Layer, id=00e5(Receiver ID)

Due to PCIe Active State Power Management that is transitioning the link to a lower power state and maybe causing the device to trigger these errors. Using the pcie\_aspm=off boot parameter could solve this problem but increase the power consumption as it disables the power savings.

### Speaker no sound

- Check kernel version, should above 6.3.8
- Make sure CONFIG\_SERIAL\_MULTI\_INSTANTIATE, CONFIG\_SND\_SOC\_CS35L41\_I2C, CONFIG\_SND\_SOC\_AMD\_ACP6x, CONFIG\_SND\_SOC\_ACPI, CONFIG\_SND\_HDA\_CODEC\_REALTEK is set.
- Make sure `i2cdetect -r -a 1` shows devices on address 40 and 42.
- Make sure  `hwinfo | grep CSC3551` shows  `modalias = "acpi:CSC3551:", driver = "Serial bus multi instantiate pseudo device driver"` 
- Make sure  `hwinfo | grep cs35l41-hda` shows  `cs35l41-hda: module = snd_hda_scodec_cs35l41_i2c` 

### Mute/micmute LEDs does not lit

The kernel version must be greater than or equal to 6.4.6, and upgrading the kernel to 6.4.6 will work normally.

### Suspend does not work

This laptop lacks support for classic S3 suspend, It only supports S2 suspend mode. Regarding the suspend mode, with the help of Mario Limonciello, after enabling CONFIG\_AMD\_PMC (kernel 6.4.6), I tested suspend and wake up separately, and it has worked normally.
