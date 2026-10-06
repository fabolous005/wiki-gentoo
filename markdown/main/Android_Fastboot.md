<!-- source: https://wiki.gentoo.org/wiki/Android/Fastboot | group: Gentoo Wiki (Main) | wiki-title: Android/Fastboot -->
---
title: Android/Fastboot
url: https://wiki.gentoo.org/wiki/Android/Fastboot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-04"
fingerprint: b60f9fd54167afe9
license: CC BY-SA 4.0
---

# Android/Fastboot

[Android](https://wiki.gentoo.org/wiki/Android)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Fastboot** is a communication protocol used primarily with Android devices. It is implemented in a command-line interface tool of the same name and as a mode of the bootloader of Android devices. It is included with the Android SDK package used primarily to modify the flash filesystem via a USB connection from a host computer. It requires that the device be started in Fastboot mode. If the mode is enabled, it will accept a specific set of commands sent to it via USB using a command line. Fastboot allows to boot from a custom recovery image. Fastboot does not require USB debugging to be enabled on the device. **Not all Android devices have fastboot enabled**

- Android device manufacturers have the option to implement fastboot or other protocols.

- Before performing any of the commands, it is important to install the Android SDK Platform-Tools, which provide necessary tools like adb and fastboot.

- To enter fastboot mode during a cold boot process, press the combination of the volume down and power buttons for 7000ms. For devices already powered on, use the command "adb reboot

bootloader" or reboot to fastboot from the device's terminal application.

- Unlocking the bootloader is necessary to flash a custom system, but it will erase all data on the device. Users should carefully read the instructions specific to their device model before proceeding, as unlocking the bootloader may void the warranty.

- Nexus and Pixel devices released since 2014 require a multistep process to unlock the bootloader due to factory-reset protection.

- Enabling OEM unlocking and USB debugging in the device settings is required before unlocking the bootloader.
- Different modes like recovery mode, bootloader mode, fastboot mode, and FastbootD mode allow for various operations on the device. The transition to FastbootD mode involves rebooting to the bootloader via adb and then using the "fastboot reboot fastboot" command.

## Installation

Android SDK Platform-Tools is a component for the Android SDK. It includes tools that interface with the Android platform, primarily adb and fastboot commands

`root #``emerge --ask dev-util/android-tools`
### Booting into Fastboot

Booting into Fastboot mode allows you to flash a device and perform various operations. To enter Fastboot mode during the cold boot process, press the combination of the volume down and power buttons for approximately 7 seconds. This method applies to all Android devices, regardless of the model, and can be used to restart the device in any state. Once the device restarts, continue holding the key combination until the device enters the Fastboot mode.

`user $``adb reboot bootloader`
### Unlocking the Bootloader

To flash a custom system, the bootloader must be unlocked. However, it is important to note that the bootloader is locked by default. Unlocking the bootloader will result in the deletion of all user data for privacy and security reasons. This includes both private app data and shared data accessible over USB, such as photos and movies. Before proceeding with unlocking the bootloader, it is strongly advised to back up any important files on the device.

Once the bootloader is unlocked, it can be re-locked at any time if desired.

### Unlocking Recent Devices

Factory-reset protection has been implemented on Nexus and Pixel devices released since 2014, starting with Nexus 6 and Nexus 9. These devices require a multistep process to unlock the bootloader due to this added security feature.

## Bootloader Operations

### Print Lock State

Prints the lock state information of the device's bootloader. This command provides details about whether the bootloader is locked or unlocked.

`user $``fastboot oem device info`
### Check Secure Image for Bootloader

Checks the integrity of the secure image for the bootloader. This command verifies the authenticity and integrity of the bootloader image to ensure that it has not been tampered with.

`user $``fastboot oem check-image`
### Unlock Bootloader

Unlocks the bootloader of the device. The \`\<key>\` parameter should be replaced with the unlock key provided by the device manufacturer. Unlocking the bootloader allows you to modify the device's firmware and install custom software.

`user $``fastboot oem unlock <key>`
### Check State for FB

Checks the lock state information for Fastboot. This command provides details about whether Fastboot is locked or unlocked on the device.

`user $``fastboot oem lock-state info`
### Dump Status for Bootloader

Dumps the status information for the device's bootloader. This command retrieves detailed information about the bootloader, including the current bootloader version, device-specific information, and other relevant details.

`user $``fastboot oem get-bootinfo`
## Device Information

### Get Root Status

Checks the root status of the device. This command determines whether the device has root access or not. Root access provides administrative privileges and allows users to make changes to the system files and settings.

`user $``fastboot oem check-rootinfo`
### Relock Bootloader

Relocks the bootloader of the device. This command locks the bootloader, preventing unauthorized modifications to the device's firmware. Relocking the bootloader ensures the integrity and security of the system.

`user $``fastboot oem relock`
### Erase Factory Reset Protection

Erases the Factory Reset Protection (FRP) on the device. FRP is a security feature that prevents unauthorized access to the device after a factory reset. This command allows you to remove the FRP protection.

`user $``fastboot oem frp-erase`
### Unlock Factory Reset Protection

Unlocks the Factory Reset Protection (FRP) on the device. This command allows you to bypass the FRP protection and gain access to the device after a factory reset.

`user $``fastboot oem frp-unlock`
### Run eMMC Diagnostic

Runs the eMMC (embedded MultiMediaCard) diagnostic on the device. This command initiates a diagnostic test on the device's eMMC storage to check for any issues or errors.

`user $``fastboot oem emmc_diag`
### Dump eMMC Info

Dumps detailed information about the eMMC (embedded MultiMediaCard) on the device. This command retrieves information such as the eMMC manufacturer, model, capacity, and other relevant details.

`user $``fastboot oem emmc-dump`
### Print Key Version

Prints the version of the device's keys. This command provides information about the cryptographic keys used for signing and verifying software updates on the device.

`user $``fastboot oem get_key_version`
### Dump Battery Status (millivolts)

Dumps the battery status in millivolts. This command retrieves the voltage level of the device's battery, indicating its current power level.

`user $``fastboot oem battery_present_check`
### Print HwNFF Version

Prints the version of the hardware and firmware. This command provides information about the hardware and firmware versions of the device.

`user $``fastboot oem get_hwnff_ver`
### Print IMEI and Serial Number

Prints the IMEI (International Mobile Equipment Identity) and serial number of the device. The IMEI is a unique identifier for the device, while the serial number is a unique identifier assigned by the manufacturer.

`user $``fastboot oem get-psid`
### Print Firmware Model

Prints the model of the device's firmware. This command provides information about the specific firmware model installed on the device.

`user $``fastboot oem get-build-number`
### Print Firmware Build

Prints the build version of the device's firmware. This command retrieves the firmware build information, including the build number and other relevant details.

`user $``fastboot oem get-product-model`
### Print Devices Connected in Fastboot

Lists all the devices connected in fastboot mode. This command displays the serial numbers of all the devices that are currently connected to the system in fastboot mode.

`user $``fastboot devices`
## Variable Product Information

### Print Device Info

Prints detailed information about the device. This command provides comprehensive information about the device, including the hardware specifications, software version, and other relevant details.

`user $``fastboot getvar all`
### Print Kernel

Prints the kernel version of the device. This command retrieves the version number of the device's kernel, which is the core component of the operating system.

`user $``fastboot getvar kernel`
### Print Bootloader Version

Prints the version of the device's bootloader. This command retrieves the version number of the bootloader, which is responsible for initializing the device's hardware and loading the operating system.

`user $``fastboot getvar version-bootloader`
### Print Product

Prints the product name of the device. This command retrieves the product name of the device, which represents the specific model or variant.

`user $``fastboot getvar product`
### Print Device Board

Prints the board name of the device. This command retrieves the name of the device's board, which represents the hardware platform on which the device is built.

`user $``fastboot getvar board`
### Print Secure Mode

Prints the security mode of the device. This command provides information about the security features and settings enabled on the device.

`user $``fastboot getvar secure`
### Print HwRev

Prints the hardware revision of the device. This command retrieves the hardware revision number, which indicates the specific version or variant of the device's hardware.

`user $``fastboot getvar hwrev`
### Print Radio Info

Prints information about the device's radio. This command provides details about the cellular radio module installed on the device, including the radio version and other relevant information.

`user $``fastboot getvar radio`
### Print Storage Type

Prints the type of storage used by the device. This command retrieves information about the type of storage technology employed by the device, such as eMMC or UFS.

`user $``fastboot getvar storage-type`
### Print eMMC Info

Prints information about the eMMC (embedded MultiMediaCard) on the device. This command provides details about the eMMC storage, including the manufacturer, model, capacity, and other relevant information.

`user $``fastboot getvar emmc`
### Print UFS Info

Prints information about the UFS (Universal Flash Storage) on the device. This command retrieves details about the UFS storage, including the manufacturer, model, capacity, and other relevant information.

`user $``fastboot getvar ufs`
### Print RAM Info

Prints information about the RAM (Random Access Memory) on the device. This command provides details about the device's RAM capacity and other relevant information.

`user $``fastboot getvar ram`
### Print CPU Info

Prints information about the CPU (Central Processing Unit) on the device. This command retrieves details about the device's CPU, including the architecture, clock speed, and other relevant information.

`user $``fastboot getvar cpu`
### Print Device Serial Number

Prints the serial number of the device. This command retrieves the unique serial number assigned to the device by the manufacturer.

`user $``fastboot getvar serialno`
### Print CID

Prints the CID (Carrier ID) of the device. The CID represents the specific carrier or network operator to which the device is associated.

`user $``fastboot getvar cid`
### Print Channel ID

Prints the channel ID of the device. The channel ID represents the specific distribution channel or market segment to which the device belongs.

`user $``fastboot getvar channelid`
### Dump Token

Dumps the token for the device. This command retrieves a token that can be used for specific operations or authentication purposes on the device.

`user $``fastboot getvar token`
### Dump Device Secure State

Dumps the secure state of the device. This command provides information about the security status and features enabled on the device.

`user $``fastboot getvar securestate`
### Dump Device Verity State

Dumps the verity state of the device. This command retrieves information about the verity status, which ensures the integrity of the device's system partition.

`user $``fastboot getvar verity-state`
### Dump Warranty Void Info

Dumps information about the warranty void status of the device. This command provides details about whether the device's warranty has been voided or remains valid.

`user $``fastboot getvar iswarrantyvoid`
### Dump Max Download Size

Dumps the maximum download size supported by the device. This command retrieves the maximum size of the file that can be downloaded to the device.

`user $``fastboot getvar max-download-size`
### Dump Reboot Reason

Dumps the reason for the last device reboot. This command provides information about the cause or trigger that led to the device rebooting.

`user $``fastboot getvar reason`
### Dump IMEI/IMEI2

Dumps the IMEI (International Mobile Equipment Identity) and IMEI2 of the device. The IMEI is a unique identifier for the device, while IMEI2 represents a second IMEI number if available.

`user $``fastboot getvar imei``user $``fastboot getvar imei2`
### Dump MEID Info

Dumps the MEID (Mobile Equipment Identifier) information of the device. The MEID is a unique identifier used for CDMA (Code Division Multiple Access) devices.

`user $``fastboot getvar meid`
### Dump Factory Date Device Was Born

Dumps the factory date when the device was manufactured. This command retrieves the manufacturing date of the device.

`user $``fastboot getvar date`
### Print SKU Version

Prints the SKU (Stock Keeping Unit) version of the device. The SKU represents a specific version or variant of the device.

`user $``fastboot getvar sku`
### Print Carrier SKU

Prints the carrier-specific SKU (Stock Keeping Unit) of the device. The carrier SKU represents a version or variant of the device specific to a particular carrier or network operator.

`user $``fastboot getvar carrier_sku`
### Dump Battery Serial

Dumps the serial number of the device's battery. This command retrieves the unique serial number assigned to the device's battery.

`user $``fastboot getvar battid`
### Dump Battery Voltage

Dumps the voltage of the device's battery. This command retrieves the current voltage level of the device's battery, indicating its power status.

`user $``fastboot getvar battery-voltage`
### Dump ICCID

Dumps the ICCID (Integrated Circuit Card Identifier) of the device's SIM card. The ICCID uniquely identifies the SIM card associated with the device.

`user $``fastboot getvar iccid`
### Dump MD5 Value

Dumps the MD5 value of the device's customization. This command retrieves the MD5 hash value that represents the customization or modifications applied to the device.

`user $``fastboot getvar cust_md5`
### Dump Max Sparse Size

Dumps the maximum size of a sparse image supported by the device. A sparse image is a file format used for efficient storage and transfer of binary data.

`user $``fastboot getvar max-sparse-size`
### Dump Alarm Status

Dumps the status of the power-off alarm on the device. This command provides information about the power-off alarm feature, which triggers an alarm when the device is powered off.

`user $``fastboot getvar poweroffalarm`
### Dump ro.carrier

Dumps the value of the \`ro.carrier\` property on the device. This command retrieves the carrier-specific value of the \`ro.carrier\` property, which represents the carrier or network operator name.

`user $``fastboot getvar ro.carrier`
### Dump ro.build.fingerprint Settings

Dumps the settings of the \`ro.build.fingerprint\` property on the device. This command retrieves the fingerprint settings, which represent the specific build configuration and details of the device.

`user $``fastboot getvar ro.build.fingerprint`
### Dump Baseband of Device

Dumps the baseband version of the device. The baseband version represents the software component responsible for controlling the device's cellular radio.

`user $``fastboot getvar version.baseband`
### Dump Kernel Version

Dumps the version of the device's kernel. The kernel version represents the core component of the operating system that manages system resources and provides essential services.

`user $``fastboot getvar kernel.version`
### Dump Git Info

Dumps the Git information of the device. This command retrieves details about the Git repository associated with the device's software, including commit information and version details.

`user $``fastboot getvar git`
### Dump Current Slot in Use

Dumps the current slot in use on the device. This command provides information about the active slot from which the device is currently booting.

`user $``fastboot getvar current-slot`
### Dump FRP Status

Dumps the FRP (Factory Reset Protection) status of the device. This command retrieves information about the status of FRP, which is a security feature that protects the device against unauthorized access.

`user $``fastboot getvar frp-state`
### Dump SIM Slot

Dumps the number of SIM slots available on the device. This command retrieves the count of SIM slots present on the device.

`user $``fastboot getvar slot-count`
### Dump Booted Slot

Dumps the slot that the device booted from. This command provides information about the slot that the device is currently using for booting.

`user $``fastboot getvar slot-successful`
### Dump Unbootable Slot

Dumps the slot that is unbootable on the device. This command provides information about the slot that cannot be used for booting the device.

`user $``fastboot getvar slot-unbootable`
### Dump Boot Retry Count

Dumps the number of times the device will boot from the active slot before changing to another slot. This command provides information about the boot retry count for the active slot.

`user $``fastboot getvar slot-retry-count`
### Dump Logical Block Size

Dumps the logical block size of the device. This command retrieves the size of the logical blocks used for data storage on the device.

`user $``fastboot getvar logical-block-size`
### Dump Erase Block Size

Dumps the erase block size of the device. This command retrieves the size of the erase blocks used for data storage on the device.

`user $``fastboot getvar erase-block-size`
### Dump Status for Userspace

Dumps the status of the userspace on the device. This command provides information about the current status of the userspace environment, including whether it is active or not.

`user $``fastboot getvar is-userspace`
### Dump PCB Number

Dumps the PCB (Printed Circuit Board) part number of the device. This command retrieves the part number associated with the device's PCB.

`user $``fastboot getvar pcb-part-no`
### Dump Primary/Secondary Display

Dumps the status of the primary and secondary displays on the device. This command provides information about the device's display setup, including the primary and secondary display configurations.

`user $``fastboot getvar primary-display``user $``fastboot getvar secondary-display`
### Dump Status for FDR

Dumps the status of FDR (Factory Data Reset) on the device. This command retrieves information about whether FDR is allowed or not, indicating whether a factory data reset can be performed on the device.

`user $``fastboot getvar fdr-allowed`
## Tips and Tricks

### Discovering Fastboot Commands

Due to variations in fastboot implementation across different device models (excluding Samsung devices, which do not utilize fastboot), it can be challenging to determine the available commands for your specific device. In most cases, these commands are not publicly disclosed and are considered proprietary information. However, if you have root access on your device, it is possible to extract the bootloader image to gain insights into its contents and potentially discover the hidden commands.

To proceed with this method, follow the steps below after accessing the shell of your device:

1. Determine the Bootloader Image Location:
  1. Change the directory to /dev/block/bootdevice/by-name by executing the command: cd /dev/block/bootdevice/by-name
  2. List all partitions and their directories by name using the command: ls -all
  3. The list will display various partition directories. Look for the one labeled *aboot* which represents the bootloader. The exact location of your bootloader partition may vary. For example, in the provided image, the bootloader partition is identified as *aboot* and located at /dev/block/mmcblk0pX. It is crucial to identify the correct directory for your device, as it will be referenced in the subsequent command.

1. Dump the Bootloader: `root #``dd if=/dev/block/<aboot_partition> of=$EXTERNAL_STORAGE/backup_aboot.img`

Once you have obtained the bootloader image, you can employ tools like strings or similar utilities to analyze its contents and potentially discover hidden commands or additional information related to your device's fastboot implementation.
