<!-- source: https://wiki.gentoo.org/wiki/Toshiba_Radius_12 | group: Gentoo Wiki (Main) | wiki-title: Toshiba Radius 12 -->
---
title: Toshiba Radius 12
url: https://wiki.gentoo.org/wiki/Toshiba_Radius_12
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "148df55f09f28bd4"
license: CC BY-SA 4.0
---

# Toshiba Radius 12

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A late 2015 netbook, comes in several configurations, including one with a 4k screen. This page focuses on model P20W-C-103 which appears to be identical to P20W-C-106, except for a HD instead of 4k screen. Reviews (link) praise the laptop for best-in-class performance, but poor ergonomics and battery. Out-of-the-box it comes installed with Windows 10.

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel Skylake i7-6500U |  | N/A | N/A | 4.4.11 | Model P20W-C-103 | 
| Touchscreen | Elan Microelectronics Corp. |  | 04f3:2230 | hid\_multitouch | 4.4.11 | Model P20W-C-103 | 
| Wifi | Intel® Dual Band Wireless-AC 7265 |  | 8086:095a | iwlwifi | 4.4.11 | Requires firmware iwlwifi-7265D-xx.ucode, available in linux-firmware ebuild. | 
| Bluetooth | Intel® Bluetooth adapter |  | USB ??? | btintel? | 4.4.11 | Think this goes over USB. Has RFkill switch. | 
| TPM chip | Unkown |  | N/A | N/A | 4.4.11 | In the BIOS (v. 1.05) there is a switch for TPM, but don't know how to enable it in the OS. | 
| USB Camera | Chicony Electronics Co., Ltd. |  | 04f2:b553 | uvcvideo | 4.4.11 | Video works fine. Can't seem to get integrated mic working for webcam. | 
| Infrared Camera | Unknown |  | N/A | N/A | 4.4.11 | Can't find corresponding PCI or USB device. | 
| Keyboard | Backlit Keyboard |  | N/A | N/A | 4.4.11 | Configured in BIOS. | 
| MMC Card Reader | Realtek Semiconductor Co., Ltd. Device |  | 10ec:522a | trsx\_pci,rtsx\_pci\_sdmmc | 4.4.11 | Tested with SD MMC card. | 

## Installation

(Write the necessary steps to get Gentoo onto this system. Try to document any special step that each user will need to reproduce on their system. Includes getting special drivers or firmware from a manufacturer website, etc.)

### Update BIOS from Windows

There is a windows-only application for updating the [BIOS](https://wiki.gentoo.org/wiki/BIOS) firmware, available from the drivers section on the Toshiba website (add link). Do this before proceeding further, if you are planning to wipe Windows from the machine.
For updating the BIOS from bootable media see [BIOS Update](https://wiki.gentoo.org/wiki/BIOS_Update).

### Paritioning

Out of the box, the system comes with five (?someone confirm - I wiped mine already) partitions and a GPT record.

1. UEFI Boot Partition
2. Windows 10 System Image
3. ?
4. Backup?
5. Backup?

The partition parameters (i.e. sector alignment) seem to work fine, so you may want to simply delete the ones you're not using, rather than creating a brand new partition table.

Use gdisk to delete all the partitions except the first, the UEFI boot record. Then follow the handbook.

### Firmware

The wireless adapter requires iwlwifi-7265D-xx.ucode. If following the Gentoo Handbook, do *not* skip the step describing installation of firmware, or you may find yourself without network access when starting up the machine for the first time.

### Kernel

(Show what options are necessary in the kernel in order to get all device components functional for this hardware platform.)

**Somebody please PM me to suggest how I can upload my .config, there are too many options to list here**

**Enable support for these hardware drivers**

### UEFI support

**Short version**: don't use EFI; switch it off in the BIOS menu (hold F2 on boot), under 'advanced'. Also, switch off 'secure boot'.

**Long version**: The system supports it, but poorly. With BIOS v1.05, it appears to be impossible to boot from a USB device using EFI. This creates a chicken and egg problem, as in order to install an EFI-enabled bootloader, you need the installation media to boot in EFI mode (otherwise the kernel can't access the EFI bios at run-time, which you need to be able to direct the EFI firmware where the new bootloader image is). You can work around this by overwriting the image in your EFI partition EFI/Boot/bootx64.img with the image created by your bootloader (assuming you didn't wipe the EFI parition - see section on partitioning).

For example, using grub, (link to the grub UEFI wiki here), the following creates an image /boot/efi/EFI/grub/grub.img:

`root #``grub2-install --efi-directory=/boot/efi --target=x86_64-efi --boot-directory=/boot`
Copy this over the existing /boot/efi/EFI/Boot/bootx64.img. Then reboot and set the boot mode to EFI in the BIOS (hold F2 on boot). The system will then be fooled into using the GRUB bootloader image instead of the existing Windows one, allowing you to boot from the harddisk in EFI mode. After rebooting, you can run grub2-install again as above; this time it will be able to direct the EFI firmware to the correct boot image, under /boot/efi/EFI/grub/grub.img, and you can in theory remove the 'bootx64.img'.

**It appears that changing BIOS settings, specifically toggling the EFI boot mode, can reset the EFI firmware variables, reverting the effects of the above procedure.**

### Emerge

**`/etc/portage/make.conf`**

**Sample make.conf**

```
# Please consult /etc/make.conf.example for a more detailed example.
CFLAGS="-O2 -pipe -march=broadwell"
CXXFLAGS="${CFLAGS}"
CPU_FLAGS_X86="aes avx avx2 fma3 mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3"
# WARNING: Changing your CHOST is not something that should be done lightly.
# Please consult http://www.gentoo.org/doc/en/change-chost.xml before changing.
CHOST="x86_64-pc-linux-gnu"
# These are the USE flags that were used in addition to what is provided by the
# profile used for building.
USE="vaapi"
VIDEO_CARDS="intel i965"
INPUT_DEVICES="evdev synaptics"
GRUB_PLATFORMS="efi-64 pc"
```
(Optional section. If the platform requires any user space packages or kernel patches, mention them here).

`root #``emerge --ask category/package`
## Configuration

(Explain any additional configuration or special customization for this hardware platform. Could be anything from BIOS settings to assigning proper media key functionality.)

### Power Management

You can use powertop to see your power consumption. Following the configuration in this section, you can expect the non-4K model to have a battery life of 7-8 hours, under a light load (e.g. web-browsing).

To enable the recommendations from powertop by default, create the following udev rules file:

**`/etc/udev/rules.d/60-powersave.rules`**

**udev rules for powersaving**

### Thermals

The ACPI thermal system kicks-in the fans automatically when under load, and the temperature usually peaks at around 86C. The laptop can become uncomfortably hot.

You can use thermald (add link) to prevent this from happening. The documentation is somewhat unhelpful so here is an explanation of how it works: There are three concepts: sensors, cooling devices (`cdev`s), and triggers. Sensors are use to read temperature; cooling devices to lower it; finally triggers are temperature limits, which are poorly named, as they can take effect *long before* they are reached - thermald forecasts the rate of change in the temperature and may start applying cooling methods long before you get to the trigger temperature. In addition to all this, as stated above, the kernel has it's own cooling mechanism which will kick in regardless of whether thermald is running or not.

To configure thermald for the *Radius 12* laptop, first remove the file /etc/thermald/thermal-conf.xml that comes out-of-the-box. It is useless and will be ignored on this laptop. Replace it with the following:

**`/etc/thermald/thermald.conf`**

**thermald configuration**

```
<?xml version="1.0"?>
<ThermalConfiguration>
<Platform>
	<Name>Toshiba Radius 12 Skylake Laptop</Name>
	<ProductName>*</ProductName>
	<Preference>QUIET</Preference>
	<ThermalZones>
		<ThermalZone>
			<Type>cpu</Type>
			<TripPoints>
				<TripPoint>
					<SensorType>x86_pkg_temp</SensorType>
					<Temperature>77000</Temperature>
					<type>passive</type>
					<ControlType>SEQUENTIAL</ControlType>
					<CoolingDevice>
						<index>1</index>
						<type>intel_pstate</type>
						<influence>100</influence>
						<SamplingPeriod>10</SamplingPeriod>
					</CoolingDevice>
				</TripPoint>
			</TripPoints>
		</ThermalZone>
	</ThermalZones>
</Platform>
</ThermalConfiguration>
```
Also you need to modify the order of the lines in the following file to match what is shown here (note the position of the 'p-state' entry, compared to what is in the default file):

**`/etc/thermald/thermal-cpu-cdev-order.xml`**

**Changing order of CPU cooling methods**

```
<CoolingDeviceOrder>
	<CoolingDevice>intel_pstate</CoolingDevice>
	<CoolingDevice>rapl_controller</CoolingDevice>
	<CoolingDevice>intel_powerclamp</CoolingDevice>
	<CoolingDevice>cpufreq</CoolingDevice>
	<CoolingDevice>Processor</CoolingDevice>
</CoolingDeviceOrder>
```

Once this is done, restart thermald:

`root #``systemctl restart thermald`
The above configuration adds a trigger at 77 degrees, which will tell the p-state driver to control the CPU frequency in order to keep the temperature from reaching that level. Note that, in addition to what is in the file, thermald already has in place two more triggers at 96 and 98 degrees; you can see these by inspecting the logs with journalctl -b -u thermald. Also note, if you want to create your own rules, the 'type' tags in the configuration file correspond to existing values under /sys/class/thermal/\*\*/type.

### Sensors

The laptop has a number of sensors:

- inclination, for detecting orientation of the display.
- light sensor - somebody using windows please confirm this, not certain.

The inclination sensor can be enabled using the generic IIO drivers in the kernel. You can then use iio-sensors-proxy in gnome > 3.18 to rotate the screen automatically<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

Install it by creating a custom ebuild. (provide link to custom ebuild guide).

**`/var/db/repos/local/gnome-extra/iio-sensors-proxy/iio-sensors-proxy-1.1.ebuild`**

**custom ebuild for iio-sensors-procy**

#### Known Issues with rotation

- If you find that rotation only works at 0 and 180 degrees, upgrade the xf86-video-intel package. x11-drivers/xf86-video-intel-2.99.917\_p20160812 is confirmed to fix this.
- Due to what appears to be a kernel bug (tested on 4.4.11), sensors only start working after a resume from suspend.

### XOrg

Problems with freezing etc. due to display driver have suggested work arounds by creating a new /etc/X11/xorg.conf.d/20-intel.conf file, as suggested on this github page: [https://github.com/linuxenko/ubuntu-skylake-i915-video-fix](https://github.com/linuxenko/ubuntu-skylake-i915-video-fix)
