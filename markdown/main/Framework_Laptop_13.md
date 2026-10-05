<!-- source: https://wiki.gentoo.org/wiki/Framework_Laptop_13 | group: Gentoo Wiki (Main) | wiki-title: Framework Laptop 13 -->
---
title: Framework Laptop 13
url: https://wiki.gentoo.org/wiki/Framework_Laptop_13
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-28"
fingerprint: "7e075e77c3aa132a"
license: CC BY-SA 4.0
---

# Framework Laptop 13

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Framework Laptop 13, released in 2021, is a highly modular and repairable 13 inch laptop. The DIY edition in particular comes without an OS and the developers and community are currently focused on supporting Arch Linux as an alternative to Windows.

## Hardware

### Intel Tiger Lake (11th-gen)

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Chipset | Intel Tiger Lake |  | \[multiple\] | i801\_smbus intel\_ish\_ipc intel\_lpss\_pci intel\_pmt | 5.14.15 |  | 
| Video card | Intel Tiger Lake-LP GT2 |  | f111:0001 | i915 | 5.14.15 | i915 and intel VIDEO\_CARDS flags | 
| Sound card | Intel Tiger Lake-LP Smart Sound |  | 8086:a0c8:f111:0001 | snd\_hda\_intel, snd\_soc\_sof\_tigerlake | 5.14.15 | Also requires either Realtek or IDT HD codec, depending on date of manufacture <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | 
| Wireless network card | Intel AX210 |  | 8086:2725:8086:0024 | iwlwifi | 5.14.15 | Needs firmware from sys-kernel/linux-firmware | 
| Bluetooth | Intel AX210 |  |  | bluetooth | 5.14.15 |  | 
| Touchpad | PixArt PIXA3854 |  | 093A:0274 | hid\_multitouch | 5.14.15 | Also depends on i2c\_designware\_core, intel\_ishtp\_hid | 
| Fingerprint Reader | Goodix USB2.0 MISC |  | 27c6:609c |  |  | Requires sys-auth/fprintd-1.94.0 | 
| Webcam | Realtek Laptop Camera |  | 0bda:5634 | uvc | 5.15.8 (as tested) |  | 

### Intel Alder Lake (12th-gen)

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Chipset | Intel Alder Lake |  |  |  |  |  | 
| Video card | Intel Alder Lake-LP GT2 |  |  |  |  |  | 
| Sound card | Intel Alder Lake-LP Smart Sound |  |  |  |  |  | 
| Wireless network card | Intel AX210 |  |  |  |  |  | 
| Bluetooth | Intel AX210 |  |  |  |  |  | 
| Touchpad | PixArt PIXA3854 |  |  |  |  | Depends on pinctrl\_tigerlake and not pinctrl\_alderlake | 
| Fingerprint Reader | Goodix USB2.0 MISC |  |  |  |  |  | 
| Webcam | Realtek Laptop Camera |  |  |  |  |  | 
| Ambient light sensor |  |  |  |  |  |  | 



### Intel Raptor Lake (13th-gen)

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Chipset | Intel Alder Lake |  |  |  |  |  | 
| Video card | Intel Alder Lake-LP GT2 |  |  |  |  |  | 
| Sound card | Intel Alder Lake-LP Smart Sound |  |  |  |  |  | 
| Wireless network card | Intel AX210 |  |  |  |  |  | 
| Bluetooth | Intel AX210 |  |  |  |  |  | 
| Touchpad | PixArt PIXA3854 |  |  |  |  | Depends on pinctrl\_tigerlake and not pinctrl\_alderlake | 

### AMD 7040 Series

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Chipset | AMD Pink Sardine |  |  |  |  |  | 
| Video card | AMD Radeon 780M |  |  | amdgpu |  |  | 
| Sound card | Ryzen HD Audio Controller |  |  | snd\_hda\_intel snd\_hda\_codec\_generic |  |  | 
| Sound card via external Displayport USB-C screen | Ryzen HD Audio Controller |  |  | snd\_hda\_codec\_hdmi snd\_hda\_codec\_generic snd\_hda\_codec\_atihdmi |  |  | 
| Wireless network card | MediaTek MT7922 |  |  | mt7921e |  |  | 
| Bluetooth | MediaTek MT7922 |  |  | bluetooth btmtk btusb (You must enable CONFIG\_BT\_HCIBTUSB\_MTK) |  |  | 
| Touchpad | PixArt PIXA3854 |  |  | hid\_multitouch i2c\_designware\_platform i2c\_hid\_acpi pinctrl\_amd |  |  | 
| Fingerprint Reader | Goodix USB2.0 MISC |  | 27c6:609c |  |  | \*Requires firmware update: see [\[1\]](https://knowledgebase.frame.work/en_us/updating-fingerprint-reader-firmware-on-linux-for-13th-gen-and-amd-ryzen-7040-series-laptops-HJrvxv_za) | 
| Webcam | Generic Laptop Camera (?) |  |  | uvcvideo |  |  | 
| Ambient light sensor |  |  |  |  |  |  | 

Backlight:

The backlight brightness keys (on the 7840U at least) show up as ACPI events rather than keyboard keys, and require the CONFIG\_I2C\_DESIGNWARE\_PLATFORM kernel option to be set to work. The backlight itself can still be adjusted without that config option, via the usual echoing to the appropriate brightness file: /sys/class/backlight/amdgpu\_bl0/brightness

The backlight keys will also need [sys-power/acpilight](https://packages.gentoo.org/packages/sys-power/acpilight) installed, replacing [x11-apps/xbacklight](https://packages.gentoo.org/packages/x11-apps/xbacklight) if it was previously installed.

### Expansion cards

**See [Framework Expansion Cards](https://wiki.gentoo.org/wiki/Framework_Expansion_Cards).** The Framework Laptop 13 features four modular expansion card slots allowing for custom port configurations.

## Installation

Because of some boot menu and device selection issues, it may be necessary to update to BIOS version 3.06 before installing.  See: [https://community.frame.work/t/public-beta-test-bios-v3-06-driver-bundle-2021-10-29/10167](https://community.frame.work/t/public-beta-test-bios-v3-06-driver-bundle-2021-10-29/10167) This update is also necessary to support the Tempo audio codec in the post-Oct 2021 Framework laptops.

The Framework Laptop 13 supports [fwupd](https://wiki.gentoo.org/wiki/Fwupd) to update the BIOS.

### Firmware

Firmware from [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) is needed for the GPU, wireless, and bluetooth interfaces.  The TigerLake GPU firmware in particular needs to be loaded immediately at boot, so it must be included as a kernel blob or on an init ramdisk.  See: [Intel#Firmware](https://wiki.gentoo.org/wiki/Intel#Firmware)

### Kernel

There have been significant stability issues with Wifi and Bluetooth in kernels prior to 5.14.15, so we're starting there.

**Intel Power Management**

Processor type and features  --->
  \[\*\] Intel Low Power Subsystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_INTEL\_LPSS\</code> to find this item.
Power management and ACPI options  --->
  \[\*\] ACPI (Advanced Configuration and Power Interface) Support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\</code> to find this item.
  \~\~ Your choice of AC, Battery, Fan, Thermal, etc \~\~
     \[\*\] Intel DPTF (Dynamic Platform and Thermal Framework) Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_DPTF\</code> to find this item.
  \[\*\] Cpuidle Driver for Intel Processors [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_INTEL\_IDLE\</code> to find this item.
Firmware Drivers  --->
  \[\*\] Load custom ACPI SSDT overlay from an EFI variable [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_EFI\_CUSTOM\_SSDT\_OVERLAYS\</code> to find this item.
Device Drivers   --->
  \<\*> Thermal Drivers  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_THERMAL\</code> to find this item.
     Intel thermal drivers  --->
        \<\*> X86 package temperature thermal driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_PKG\_TEMP\_THERMAL\</code> to find this item.
        \<\*> Intel SoCs DTS thermal driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_SOC\_DTS\_THERMAL\</code> to find this item.
        ACPI INT340X thermal drivers  --->
           \<\*> ACPI INT340X thermal driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INT340X\_THERMAL\</code> to find this item.
        \<\*> Intel TCC offset cooling Driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_TCC\_COOLING\</code> to find this item.
  Multifunction device drivers  --->
     \<\*> Intel Low Power Subsystem support in PCI mode [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MFD\_INTEL\_LPSS\_PCI\</code> to find this item.
     \<\*> Intel PMC Driver for Broxton [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MFD\_INTEL\_PMC\_BXT\</code> to find this item.
     \<\*> Intel Platform Monitoring Technology (PMT) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_PMT\_TELEMETRY\</code> to find this item.
  \[\*\] X86 Platform Specific Device Drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_PLATFORM\_DEVICES\</code> to find this item.
     \<\*> Intel PMC Core driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_PMC\_CORE\</code> to find this item.

**AMD Power Management**

Processor type and features  --->
  \[\*\] AMD ACPI2Platform devices support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_AMD\_PLATFORM\_DEVICE\</code> to find this item.
  \[\*\] Supported processor vendors  --->
      \[\*\]   Support AMD processors [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_SUP\_AMD\</code> to find this item.
Power management and ACPI options  --->
  \[\*\] ACPI (Advanced Configuration and Power Interface) Support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\</code> to find this item.
      CPU Frequency scaling  --->
         \[\*\]   AMD Processor P-State driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_AMD\_PSTATE\</code> to find this item.
         (3)     AMD Processor P-State default mode
Firmware Drivers  --->
  \[\*\] Load custom ACPI SSDT overlay from an EFI variable [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_EFI\_CUSTOM\_SSDT\_OVERLAYS\</code> to find this item.
Device Drivers   --->
   \[\*\] {KEntry|Platform support for Chrome hardware  --->|CHROME\_PLATFORMS}}
        \<\*>   ChromeOS specific ACPI extensions
        \<M>   Chrome OS Laptop
        \<M>   Chrome OS pstore support
        \<M>   ChromeOS Tablet Switch Controller
        \<M>   ChromeOS Embedded Controller
        \<M>   ChromeOS Embedded Controller (I2C)
        \<M>   ChromeOS Embedded Controller (SPI)
        \<M>   ChromeOS Embedded Controller (LPC)
        \<M>   Backlight LED support for Chrome OS keyboards
        \<M>   ChromeOS EC miscdevice
        \<M>   Chromebook Pixel's lightbar support
        \<M>   ChromeOS EC MEMS Sensor Hub
        \<M>   ChromeOS EC control and information through sysfs
        \<M>   ChromeOS EC Type-C Connector Control
        \<M>   ChromeOS HPS device
        \<M>   Logging driver for USB PD charger
        \<M>   ChromeOS Type-C power delivery event notifier 
        \<M>   ChromeOS Privacy Screen support
        \<M>   ChromeOS EC Type-C Switch Control
        \<M>   ChromeOS Wilco Embedded Controller 
   \<\*> Thermal Drivers  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_THERMAL\</code> to find this item.

**I2C Bus (needed for Touchpad, Camera, and probably other stuff)**

Device Drivers  --->
  \<\*> I2C Support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\</code> to find this item.
     \<\*>   I2C device interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_CHARDEV\</code> to find this item.
     I2C Hardware Bus Support  --->
        \<\*> Intel 82801 (ICH/PCH) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_I801\</code> to find this item.
        \<\*> Synopsys Designware Platform [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_DESIGNWARE\_PLATFORM\</code> to find this item.

**I2C Bus on AMD**

Device Drivers  --->
  \<\*> Pin controllers  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PINCTRL\</code> to find this item.
    \<\*> AMD GPIO pin control [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PINCTRL\_AMD\</code> to find this item.

**Touchpad**

```
Device Drivers  --->
  Input device support  --->
  HID support  ---> 
     <*> Generic HID driver 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HID_GENERIC</code> to find this item.
     Special HID driver  --->
        <*> HID Multitouch panels [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HID_MULTITOUCH</code> to find this item.
        <*> HID Sensors framework support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HID_SENSOR_HUB</code> to find this item.
     <*> I2C HID support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_I2C_HID</code> to find this item.
       <*> HID over I2C transport layer ACPI driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_I2C_HID_ACPI</code> to find this item.
     Intel ISH HID support  --->
        <*> Intel Integrated Sensor Hub [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INTEL_ISH_HID</code> to find this item.
**Intel Wireless LAN**

\[\*\] Networking support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.
  \[\*\] Wireless  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WIRELESS\</code> to find this item.
     \<\*> cfg80211 - wireless configuration API [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CFG80211\</code> to find this item.
     \<\*> Generic IEEE 802.11 Networking Stack (mac80211) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MAC80211\</code> to find this item.

**AMD Wireless LAN**

\[\*\] Networking support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.
  \[\*\] Wireless  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WIRELESS\</code> to find this item.
     \<\*> cfg80211 - wireless configuration API [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CFG80211\</code> to find this item.
     \<\*> Generic IEEE 802.11 Networking Stack (mac80211) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MAC80211\</code> to find this item.
Device Drivers  --->
  \[\*\] Network device support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETDEVICES\</code> to find this item.
     \[\*\] Wireless LAN  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WLAN\</code> to find this item.
        \[\*\] MediaTek devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WLAN\_VENDOR\_MEDIATEK\</code> to find this item.
        \<\*>   MediaTek MT7921E (PCIe) support  [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MT7921E\</code> to find this item.

**Intel Graphics card**

Device Drivers  --->
 Graphics support  --->
  \<\*> /dev/agpgart (AGP Support)  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AGP\</code> to find this item.
  \<\*> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\</code> to find this item.
  \<\*> Intel 8xx/9xx/G3x/G4x/HD Graphics [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_I915\</code> to find this item.

**AMD Graphics card**

Device Drivers  --->
 Graphics support  --->
  \<\*> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\</code> to find this item.
     \<\*> AMDGPU [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_AMDGPU\</code> to find this item.

Note: you have to include firmware blobs in the kernel if you don't compile amdgpu as a module. The required configuration string on 7040 is:
*CONFIG\_EXTRA\_FIRMWARE="amdgpu/psp\_13\_0\_4\_toc.bin amdgpu/dcn\_3\_1\_4\_dmcub.bin amdgpu/gc\_11\_0\_1\_pfp.bin amdgpu/sdma\_6\_0\_1.bin amdgpu/vcn\_4\_0\_2.bin amdgpu/gc\_11\_0\_1\_imu.bin amdgpu/gc\_11\_0\_1\_mec.bin amdgpu/gc\_11\_0\_1\_mes.bin amdgpu/gc\_11\_0\_1\_mes\_2.bin amdgpu/psp\_13\_0\_4\_ta.bin amdgpu/gc\_11\_0\_1\_me.bin amdgpu/gc\_11\_0\_1\_mes1.bin amdgpu/gc\_11\_0\_1\_rlc.bin"*

**Sound**

Device Drivers  --->
  \<\*> Sound card support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item.
     \<\*> Advanced Linux Sound Architecture  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item.
        HD Audio  --->
           \<\*> HD Audio PCI [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_INTEL\</code> to find this item.
           \<\*> Build Realtek HD-audio codec support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_CODEC\_REALTEK\</code> to find this item.
           \<\*> Build IDT/Sigmatel HD-audio codec support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_CODEC\_SIGMATEL\</code> to find this item.
           \<\*> Build HDMI/DisplayPort HD-audio codec support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_CODEC\_HDMI\</code> to find this item.
        \<\*> ALSA for SoC audio support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\</code> to find this item.
           \[\*\] Sound Open Firmware Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\_SOF\_TOPLEVEL\</code> to find this item.
           \<\*>   SOF PCI enumeration support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\_SOF\_PCI\</code> to find this item.
           \[\*\]   SOF support for Intel audio DSPs [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\_SOF\_INTEL\_TOPLEVEL\</code> to find this item.
           \<\*>      SOF support for Tigerlake [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\_SOF\_TIGERLAKE\</code> to find this item.

**Storage (NVMe)**

```
Device Drivers  --->
  NVME Support  ---> 
      <*> NVM Express block device 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BLK_DEV_NVME</code> to find this item.
           [*] NVMe multipath support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NVME_MULTIPATH</code> to find this item.
**Ambient light sensor**

Device Drivers  --->
  \<M> Industrial I/O support   --->  [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IIO\</code> to find this item.
      Light sensors --->
           \<M> HID ALS (CONFIG\_HID\_SENSOR\_ALS) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HID\_SENSOR\_ALS\</code> to find this item.

The ambient light sensors values can be read using the command:

`root #``cat /sys/bus/iio/devices/iio\:device0/in_illuminance_raw`
For more features on battery charge limit and LED configuration an out-of-tree kernel module can be installed via

`root #``emerge --ask app-laptop/framework-laptop-kmod`
## Troubleshooting

### BIOS updates

Upgrading the Framework Laptop's BIOS will erase the EFI boot entries, leaving a Gentoo system unbootable afterwards. This can be fixed by booting from Gentoo install media, mounting and chrooting into the local install root and re-running grub-install.

Follow the *Mounting the necessary filesystems, Entering the new environment, and Mounting the boot partition* steps from the Handbook, substituting device names as necessary ([Handbook:AMD64/Parts/Installation/Base](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Mounting_the_necessary_filesystems))

Then recreate the necessary EFI entry

`root #``grub-install --target=x86_64-efi --efi-directory=/boot`
### Motherboard and/or CPU upgrades

Intel 12th Gen Alder Lake CPUs have a subset of the features of the 11th gen CPUs. Recompile the system to something more generic if attempting to use a Gentoo installation from an 11th gen Framework laptop after upgrading it to 12th gen. See the [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization) article for more details on adjusting the `-march` and `-mtune` compiler flags.

### No Lid Switch and Power Button ACPI events

If closing the lid or pressing the power button does nothing, check if ACPI events are working:

`user $``evtest`
No device specified, trying to scan all of /dev/input/event\*
Available devices:
/dev/input/event0:9Lid Switch
/dev/input/event1:9Power Button
Select the device event number \[0-14\]:

Choose the Lid Switch device and close/open the lid.
If there is no output, make sure kernel option `CONFIG_ACPI_EC` is enabled.

**CONFIG\_ACPI\_EC**

Power management and ACPI options  --->
  \[\*\] ACPI (Advanced Configuration and Power Interface) Support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\</code> to find this item.
     \[\*\] Embedded Controller  [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_EC\</code> to find this item.



### Touchpad not working

If the touchpad is not working (no movement, no clicks, no gestures), verify the "HID over I2C transport layer ACPI" driver (see Kernel section / Touchpad) is available as a module or built-in driver.

If "PIXA3854" cannot be found in the dmesg log, then the kernel is likely missing driver support.

#### Without "HID over I2C transport layer ACPI"

`user $``dmesg |grep -i hid`
\[    2.499342\] hid: raw HID events driver (C) Jiri Kosina
\[    2.499362\] usbcore: registered new interface driver usbhid
\[    2.499363\] usbhid: USB HID core driver
\[    3.376794\] hid-generic 0003:32AC:0002.0001: hiddev96,hidraw0: USB HID v1.11 Device \[Framework HDMI Expansion Card\] on usb-0000:00:14.0-4/input1

#### With "HID over I2C transport layer ACPI"

`user $``dmesg |grep -i hid`
\[    2.494448\] hid: raw HID events driver (C) Jiri Kosina
\[    2.494474\] usbcore: registered new interface driver usbhid
\[    2.494475\] usbhid: USB HID core driver
\[    3.371391\] hid-generic 0003:32AC:0002.0001: hiddev96,hidraw0: USB HID v1.11 Device \[Framework HDMI Expansion Card\] on usb-0000:00:14.0-4/input1
\[   19.053758\] hid-generic 0018:32AC:0006.0002: input,hidraw1: I2C HID v1.00 Device \[FRMW0001:00 32AC:0006\] on i2c-FRMW0001:00
\[   19.079450\] hid-generic 0018:093A:0274.0003: input,hidraw2: I2C HID v1.00 Mouse \[PIXA3854:00 093A:0274\] on i2c-PIXA3854:00
\[   19.280641\] Module hid\_sensor\_hub is blacklisted
\[   19.430450\] hid-multitouch 0018:093A:0274.0003: input,hidraw2: I2C HID v1.00 Mouse \[PIXA3854:00 093A:0274\] on i2c-PIXA3854:00

### Built-in Keyboard not working on boot to unlock encrypted device

Set `CONFIG_KEYBOARD_ATKBD` to `y` to enable built-in keyboard to unlock encrypted luks devices

```
Device Drivers  --->
  Input device support  --->
     [*] Keyboards  ---> 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT_KEYBOARD</code> to find this item.
        <*> AT keyboard [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_KEYBOARD_ATKBD</code> to find this item.
### EFI Stub Kernel Boot Failed

If `CONFIG_EFI_DISABLE_PCI_DMA`is set to `y`, manually selecting the EFI stub kernel on boot will fail with errors:

`EFI stub: ERROR exit_boot() failed!`

`EFI stub: ERROR efi_main() failed!`


These errors flash by quickly and one may only see a blue box stating boot failed.

Disable `CONFIG_EFI_DISABLE_PCI_DMA` and recompile to boot the efi stub kernel.

```
Device Drivers  --->
  Firmware Drivers --->
     EFI (Extensible Firmware Interface) Support  --->
        [ ] Clear Busmaster bit on PCI bridges during ExitBootServices() 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EFI_DISABLE_PCI_DMA</code> to find this item.
### Fingerprint reader

Fingerprints are read via the [fprintd](https://wiki.gentoo.org/wiki/Fingerprint_Reader) package installed by

`root #``emerge --ask sys-auth/fprintd`
Fingerprints are stored on the fingerprint reader itself, so if you register a fingerprint and then switch operating systems, you won't be able to register it on the second OS as fprintd gives you "Enroll result: enroll-duplicate". This is a problem if you dual-boot or have to re-install linux.

To wipe these fingerprints, install fprintd and use the following python script<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>:

**`wipe_fingerprints.py`**

**Wipe Fingerprint Script**

```
#! /usr/bin/python3
import gi
gi.require_version('FPrint', '2.0')
from gi.repository import FPrint
ctx = FPrint.Context()
for dev in ctx.get_devices():
    print(dev)
    print(dev.get_driver())
    print(dev.props.device_id);
    dev.open_sync()
    dev.clear_storage_sync()
    print("All prints deleted.")
    dev.close_sync()
```
Then run the script with python3 as root:

`root #``python3 ./wipe_fingerprints.py`
### Brightness Keys Not Working

Due to a quirk<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> with Linux drivers and how the Framework laptop is setup, one cannot have both the ambient light sensor and the brightness keys work. For the brightness keys to function and send XF86MonBrightnessUp / Down events the ambient light sensor driver must be disabled.

To do this ensure that the  `hid_sensor_hub` Kernel module is not being loaded. If the `CONFIG_HID_SENSOR_HUB` kernel build option is set to `m` then either unset it and rebuild the kernel, or add a modprobe blacklist rule:

```
# /etc/modprobe.d/framework.conf
blacklist hid_sensor_hub
```
If `CONFIG_HID_SENSOR_HUB` is set to `y` then unset it and rebuild the kernel<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.

### Freezing with 6.4 series kernels

It is necessary to boot with tpm\_tis.interrupts=0 in order to avoid a hang at boot as of 6.4.3.[\[5\]](https://wiki.gentoo.org#cite_note-5)

## See also

- [Framework Expansion Cards](https://wiki.gentoo.org/wiki/Framework_Expansion_Cards)
- [Framework Laptop 16](https://wiki.gentoo.org/wiki/Framework_Laptop_16) — a highly modular and repairable 16 inch laptop
- [Framework Laptop 12](https://wiki.gentoo.org/index.php?title=Framework_Laptop_12&action=edit&redlink=1)
- [Framework Desktop](https://wiki.gentoo.org/index.php?title=Framework_Desktop&action=edit&redlink=1)
