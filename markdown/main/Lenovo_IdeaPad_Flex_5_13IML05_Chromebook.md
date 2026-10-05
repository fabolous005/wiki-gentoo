<!-- source: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_Flex_5_13IML05_Chromebook | group: Gentoo Wiki (Main) | wiki-title: Lenovo IdeaPad Flex 5 13IML05 Chromebook -->
---
title: Lenovo IdeaPad Flex 5 13IML05 Chromebook
url: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_Flex_5_13IML05_Chromebook
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-20"
fingerprint: "7e07cb57d1aa5b60"
license: CC BY-SA 4.0
---

# Lenovo IdeaPad Flex 5 13IML05 Chromebook

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

![](https://wiki.gentoo.org/images/thumb/b/b1/Lenovo_IdeaPad_Flex_5_13IML05_Chromebook.jpg/300px-Lenovo_IdeaPad_Flex_5_13IML05_Chromebook.jpg)

The **Lenovo IdeaPad Flex 5 13IML05 Chromebook** is a Chromebook released in 2020. <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> The code name of this Chromebook is **Akemi**.

This Chromebook **should not** be confused with the [Lenovo IdeaPad Flex 5 13ITL6 Chromebook](https://psref.lenovo.com/Product/IdeaPad/IP_Flex_5_Chrome_13ITL6) released in 2021. [\[2\]](https://wiki.gentoo.org#cite_note-2)

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel® Core™ i3-10110U |  | N/A | N/A | 6.6.13 |  | 
| GPU | Intel Corporation CometLake-U GT2 \[UHD Graphics\] |  | 8086:9b41 | i915 | 6.6.13 |  | 
| SSD | Samsung Electronics Co Ltd NVMe SSD Controller 980 |  | 144d:a809 | nvme | 6.6.13 |  | 
| MicroSD card reader | Intel Corporation Comet Lake PCH-LP SCS3 |  | 8086:02f5 | sdhci-pci | 6.6.13 |  | 
| USB Ports | Intel Corporation Comet Lake PCH-LP USB 3.1 xHCI Host Controller |  | N/A | xhci\_hcd | 6.6.13 |  | 
| Wi-Fi | [Intel® Wi-Fi 6 AX201](https://www.intel.com/content/www/us/en/products/sku/130293/intel-wifi-6-ax201-gig/specifications.html) |  | 8086:02f0 | [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) | 6.6.13 | The card is affected by [a bug](https://wiki.gentoo.org/wiki/Iwlwifi#Network_crashes_under_heavy_load). | 
| Bluetooth | [Intel® Wi-Fi 6 AX201](https://www.intel.com/content/www/us/en/products/sku/130293/intel-wifi-6-ax201-gig/specifications.html) |  | 8087:0026 | btusb | 6.6.13 |  | 
| Speakers | Intel Corporation Comet Lake PCH-LP cAVS |  | 8086:02c8 | sof-audio-pci-intel-cnl | 6.6.13 |  | 
| Microphone | Intel Corporation Comet Lake PCH-LP cAVS |  | N/A | sof-audio-pci-intel-cnl | 6.6.13 |  | 
| 3.5mm jack | Intel Corporation Comet Lake PCH-LP cAVS |  | N/A | sof-audio-pci-intel-cnl | 6.6.13 |  | 
| Touchpad | N/A |  | 06cb:cde1 | N/A | 6.6.13 |  | 
| Touchscreen | N/A |  | 27c6:0e32 | N/A | 6.6.13 |  | 
| Webcam | Syntek Integrated Camera |  | 174f:244f | uvcvideo | 6.6.13 |  | 
| Accelerometer | N/A |  | N/A | N/A | 6.6.38 |  | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Stylus | N/A |  | N/A | N/A | N/A |  | 

## Installation

### Gentoo LiveGUI USB Image

#### Sound

By default, the sound card is disabled and there is no output to the speakers. The image does not contain required firmware blobs, so it is necessary to install them. The following script installs the blobs and configures PipeWire to use the sound card.

**`sound.sh`**

### Firmware

WiFi, Bluetooth, GPU blobs:

`root #``emerge --ask sys-kernel/linux-firmware`
- iwlwifi-QuZ-a0-hr-b0-77.ucode
- intel/ibt-19-0-4.sfi
- intel/ibt-19-0-4.ddc
- i915/kbl\_dmc\_ver1\_04.bin


Sound blobs:

`root #``emerge --ask sys-firmware/sof-firmware`
- intel/sof/community/sof-cml.ri
- intel/sof-tplg/sof-cml-rt5682-max98357a.tplg


CPU blobs:

`root #``emerge --ask sys-firmware/intel-microcode`
- intel-ucode/06-8e-0c

### Kernel

**All required external firmware (kernel version 6.6.13)**

```
Device Drivers  --->
    Generic Driver Options  --->
        Firmware loader --->
            -*- Firmware loading facility
            (intel-ucode/06-8e-0c iwlwifi-QuZ-a0-hr-b0-77.ucode intel/ibt-19-0-4.sfi intel/ibt-19-0-4.ddc intel/sof/community/sof-cml.ri intel/sof-tplg/sof-cml-rt5682-max98357a.tplg i915/kbl_dmc_ver1_04.bin) External firmware blobs to build into the kernel binary 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EXTRA_FIRMWARE</code> to find this item.
            (/lib/firmware) Firmware blobs root directory [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EXTRA_FIRMWARE_DIR</code> to find this item.
**Graphics**

```
Device Drivers  --->
    Graphics support  --->
        Frame buffer Devices  --->
            [*] Support for frame buffer devices 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FB</code> to find this item.
        [*] Direct Rendering Manager (XFree86 4.1.0 and higher DRI support) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DRM</code> to find this item.
        [*] Intel 8xx/9xx/G3x/G4x/HD Graphics [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DRM_I915</code> to find this item.
        [*] Enable legacy fbdev support for your modesetting driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DRM_FBDEV_EMULATION</code> to find this item.
**SSD**

```
Device Drivers  --->
    NVME Support  --->
        [*] NVM Express block device 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BLK_DEV_NVME</code> to find this item.
**WiFi**

```
Device Drivers  --->
   [*] Network device support  --->
       [*] Wireless LAN  --->
           [*] Intel devices 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_WLAN_VENDOR_INTEL</code> to find this item.
           [*]   Intel Wireless WiFi Next Gen AGN - Wireless-N/Advanced-N/Ultimate-N (iwlwifi) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_IWLWIFI</code> to find this item.
           [*]     Intel Wireless WiFi MVM Firmware support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_IWLMVM</code> to find this item.
**I2C Bus**

```
Device Drivers  --->
    I2C support  --->
        -*- I2C support
              I2C Hardware Bus support  --->
                  [*] Intel 82801 (ICH/PCH) 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_I2C_I801</code> to find this item.
                  [*] Synopsys DesignWare Platform [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_I2C_DESIGNWARE_PLATFORM</code> to find this item.
                  [*] Synopsys DesignWare PCI [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_I2C_DESIGNWARE_PCI</code> to find this item.
    HID bus support  --->
        --- HID bus support
        [*]   I2C HID support  --->
                [*] HID over I2C transport layer ACPI driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_I2C_HID_ACPI</code> to find this item.
**Touchscreen (relies on the I2C bus)**

```
Device Drivers  --->
    HID bus support  --->
        --- HID bus support
        [*]   Generic HID driver 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HID_GENERIC</code> to find this item.
**Touchpad (relies on the I2C bus)**

```
Device Drivers  --->
    HID bus support  --->
        --- HID bus support
              Special HID drivers  --->
                  [*] Synaptics RMI4 device support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HID_RMI</code> to find this item.
**External USB mouses (optional)**

```
Device Drivers  --->
    HID bus support  --->
        --- HID bus support
              USB HID support  --->
                [*] USB HID transport layer 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_USB_HID</code> to find this item.
**Sound**

```
Device Drivers  --->
    -*- Pin controllers  --->
        Intel pinctrl drivers  --->
            [*] Intel Cannon Lake PCH pinctrl and GPIO driver 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_PINCTRL_CANNONLAKE</code> to find this item.
    [*] Sound card support  --->
        [*] Advanced Linux Sound Architecture  --->
            HD-Audio  --->
                [*] Build HDMI/DisplayPort HD-audio codec support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_SND_HDA_CODEC_HDMI</code> to find this item.
            [*] ALSA for SoC audio support  --->
                [*] Sound Open Firmware Support  --->
                    [*] SOF PCI enumeration support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_SND_SOC_SOF_PCI</code> to find this item.
                    [*] SOF support for Intel audio DSPs [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_SND_SOC_SOF_INTEL_TOPLEVEL</code> to find this item.
                    [*] SOF support for CometLake [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_SND_SOC_SOF_COMETLAKE</code> to find this item.
                    [*] SOF support for HDA Links(HDA/HDMI) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_SND_SOC_SOF_HDA_LINK</code> to find this item.
                    [*]   SOF support for HDAudio codecs [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_SND_SOC_SOF_HDA_AUDIO_CODEC</code> to find this item.
                -*- Intel Machine drivers  --->
                    [*] SOF with rt5682 codec in I2S Mode [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_SND_SOC_INTEL_SOF_RT5682_MACH</code> to find this item.
**Webcam**

```
Device Drivers  --->
    Multimedia support  --->
        --- Multimedia support
              Media core support  --->
                [*] Video4Linux core 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_VIDEO_DEV</code> to find this item.
              Media drivers  --->
                [*] Media USB Adapters  --->
                      --- Media USB Adapters
                      [*]   USB Video Class (UVC) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_USB_VIDEO_CLASS</code> to find this item.
**Accelerometer**

```
Device Drivers  --->
    [*] Platform support for Chrome hardware  --->
        [*]   ChromeOS Embedded Controller 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CROS_EC</code> to find this item.
        [*]     ChromeOS Embedded Controller (LPC) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CROS_EC_LPC</code> to find this item.
        [*]   ChromeOS EC MEMS Sensor Hub [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CROS_EC_SENSORHUB</code> to find this item.
    [*] Industrial I/O support  --->
        [*]   ChromeOS EC Sensors Core [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_IIO_CROS_EC_SENSORS_CORE</code> to find this item.
        [*]     ChromeOS EC Contiguous Sensor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_IIO_CROS_EC_SENSORS</code> to find this item.
## Configuration

### Sound

Install [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire) with the following USE flags: **sound-server**, **pipewire-alsa**, **bluetooth** (optional).

The sound doesn't work out of the box, so you have to manually specify the sink and source. In the case of Bluetooth or 3.5mm jack, no additional configuration is required.

**`/etc/pipewire/pipewire.conf.d/alsa.conf`**
