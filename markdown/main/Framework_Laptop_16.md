<!-- source: https://wiki.gentoo.org/wiki/Framework_Laptop_16 | group: Gentoo Wiki (Main) | wiki-title: Framework Laptop 16 -->
---
title: Framework Laptop 16
url: https://wiki.gentoo.org/wiki/Framework_Laptop_16
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-05"
fingerprint: "7e060977d1e239c8"
license: CC BY-SA 4.0
---

# Framework Laptop 16

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Framework Laptop 16, released in 2024, is a highly modular and repairable 16 inch laptop. This article is incomplete, see [Framework Laptop 13](https://wiki.gentoo.org/wiki/Framework_Laptop_13) for a guide on comparable hardware.

## Hardware

### Ryzen mainboard

Other than the CPU, both models (Ryzen 7 and 9) have the same hardware.

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen 9 7940HS AMD Ryzen 7 7840HS | Works |  |  |  | [AMD microcode](https://wiki.gentoo.org/wiki/AMD_microcode) | 
| Chipset | AMD Pink Sardine | Works |  |  |  |  | 
| Video card | AMD Phoenix1 | Works | 1002:15bf | amdgpu |  | Use `VIDEO_CARDS: amdgpu radeonsi` | 
| Sound card | AMD Rembrandt Radeon High Definition Audio Controller | Works | 1002:1640 | snd\_hda\_intel |  |  | 
| Sound card | AMD Family 17h/19h HD Audio Controller | Works | 1022:15e3 | snd\_hda\_intel |  |  | 
| Audio coprocessor | AMD ACP/ACP3X/ACP6x Audio Coprocessor | Works | 1022:15e2 | snd\_pci\_ps |  | Also requires Realtek HD-audio codec | 
| Wireless network card | MediaTek MT7922 | Works | 14c3:0616 | mt7921e |  |  | 
| Bluetooth | MediaTek MT7922 | Works | 0e8d:e616 | btusb |  |  | 
| Fingerprint Reader | Goodix USB2.0 MISC | Works | 27c6:609c |  |  | Requires [a fingerprint reader package](https://wiki.gentoo.org/wiki/Fingerprint_Reader) | 
| Webcam | Realtek Semiconductor Corp. Laptop Camera | Works | 0bda:5634 | uvcvideo |  | The microphone is attached to the sound card (17h/19h), not the webcam | 
| Thunderbolt/USB4 | AMD Pink Sardine USB4/Thunderbolt NHI controller | Works | 1022:1668 1022:1669 | thunderbolt |  | Compatibility with complex devices such as docks and eGPUs has not been confirmed. | 
| Ambient light sensor |  | Works | 32ac:001b (Framework's HID sensor hub) | hid\_sensor\_als, hid\_sensor\_hub |  | Found at: `/sys/bus/iio/devices/iio:device0/` | 
| Encryption | AMD Family 19h (Model 74h) CCP/PSP 3.0 Device | Works | 1022:15c7 | ccp |  | Tested with `cryptsetup benchmark` | 
| AI accelerator | AMD IPU Device | Not tested | 1022:1502 |  |  | Requires `dev-libs/xdna-driver` from the [GURU repository](https://wiki.gentoo.org/wiki/Project:GURU) | 

### Input modules

The Framework Laptop 16 has a user-configurable input deck. Unless otherwise noted, the modules listed below are OEM modules from Framework.

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Keyboard | Framework Laptop 16 Keyboard Module - ANSI | Works | 32ac:0012 |  |  | Generic HID? | 
| Touchpad | PixArt PIXA3854 | Works | 093A:0274 | hid\_multitouch i2c\_designware\_platform pinctrl\_amd |  |  | 
| Numpad | Framework Laptop 16 Numpad Module | Works | 32ac:0014 |  |  | Generic HID? | 
| RGB Macropad |  | Works | 32ac:0013 |  |  | Generic HID? | 
| LED Matrix |  | Works | 32ac:0020 | cdc\_acm |  |  | 

Configuring the QMK devices and LED matrix via web tools requires udev rules (see below).

### Expansion cards

**See [Framework Expansion Cards](https://wiki.gentoo.org/wiki/Framework_Expansion_Cards).** The Framework Laptop 16 features six modular expansion card slots allowing for custom port configurations.

### Expansion bay

The Framework 16 has an expansion bay with a custom PCIe connector. Unless otherwise noted, the modules listed below are OEM modules from Framework.

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes |  | 
|---|---|---|---|---|---|---|---|
|  | Expansion Bay Shell | Works |  |  |  | Provides cooling and fills the bay. Fans are managed by the embedded controller. |  | 
| GPU | AMD Radeon RX 7700S | Works | 1002:7480 1002:ab30 | amdgpu snd\_hda\_intel |  |  |  | 
| GPU | NVIDIA RTX 5070 8GiB Max-Q / Mobile (GB206M) | Works | 10de:2d58 10de:22eb | nvidia-drivers USE=+kernel-open |  |  |  | 

## Installation

See the [Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) for instructions on the general installation process.

### Firmware

Firmware from [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) is needed for the GPU, wireless, and bluetooth interfaces.

### Kernel

The easiest way to configure the kernel is use a distribution kernel. The next easiest way is to use [genkernel](https://wiki.gentoo.org/wiki/Genkernel). See [Kernel installation](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel) in the handbook for details.

genkernel automatically enables enough to get the system booted, but additional changes are required to get all hardware working. On the upside, `genkernel all` does everything - it compiles the kernel, builds an initramfs, and installs both in the boot partition, and it can be configured to update the bootloader entries.

**Note: compiling certain drivers into the kernel (vs as a module) may cause problems.** Some drivers such as the Wi-Fi card driver (MediaTek MT7921E) will not boot up correctly if firmware is not available. This can be resolved by configuring the driver to be a module, or by including the firmware in the initramfs.

**General**

```
Processor type and features  --->
   [*] AMD ACPI2Platform devices support
   [*] Machine Check / overheating reporting
      [*] AMD MCE features
   Performance monitoring  --->
      <*> Intel/AMD rapl performance events
      <*> AMD Processor Power Reporting Mechanism
      <*> AMD Uncore performance events
Power management and ACPI options --->
   [*] ACPI (Advanced Configuration and Power Interface) Support  --->
      [*]   ACPI Platform Error Interface (APEI)
      [*]     APEI Generic Hardware Error Source
       CPU Frequency scaling  --->
         [*]   AMD Processor P-State driver
Device Drivers  --->
   <*> Hardware Monitoring support  --->
      <*>   AMD Family 10h+ temperature sensor
   [*] Watchdog Timer Support  --->
      <*>   AMD/ATI SP5100 TCO Timer/Watchdog
   [*] HID bus support  --->
      <*>   I2C HID support  --->
            AMD SFH HID Support  --->
               <*> AMD Sensor Fusion Hub
   [*] Reliability, Availability and Serviceability (RAS) features
   <*> EDAC (Error Detection And Correction) reporting  --->
      <*>   Decode MCEs in human-readable form (only on AMD for now)
      <*>   Output ACPI APEI/GHES BIOS detected errors via EDAC
   [*] X86 Platform Specific Device Drivers  --->
      <*>   AMD SoC PMC driver
   [*] IOMMU Hardware Support  --->
      [*]   AMD IOMMU support
   [*] Generic powercap sysfs driver  --->
      <*>   Intel RAPL Support via MSR Interface
```
**NVMe/SSD slots**

```
Enable the block layer  --->
   [*]   Logic for interfacing with Opal enabled SEDs
Device Drivers  --->
   NVME Support  --->
      <*> NVM Express block device
```
**Wi-Fi and Bluetooth**

```
Device Drivers  --->
   [*] Network device support  --->
      [*]   Wireless LAN  --->
         [*]   MediaTek devices
            <M>     MediaTek MT7921E (PCIe) support
[*] Networking support  --->
   <M>   Bluetooth subsystem support  --->
      [*]     Bluetooth High Speed (HS) features
      [*]   Bluetooth Low Energy (LE) features
            Bluetooth device drivers  --->
               <M> HCI USB driver
               [*]   MediaTek protocol support
```
**Graphics**

```
Memory Management options  --->
   [*] Memory hotplug  --->
      [*]   Allow for memory hot remove
   [*] Device memory (pmem, HMM, etc...) hotplug support
   [*] Unaddressable device memory (GPU memory, ...)
Device Drivers  --->
   Graphics support  --->
      <M> AMD GPU
      [*]   Enable amdgpu support for SI parts
            ACP (Audio CoProcessor) Configuration  --->
               [*] Enable AMD Audio CoProcessor IP support
      [*]   HSA kernel driver for AMD GPU devices
      [*]     Enable HMM-based shared virtual memory manager
      Frame buffer Devices  --->
         <*> Support for frame buffer device drivers  --->
            <*>   VGA 16-color graphics support
            [*]   VESA VGA graphics support
            [*]   EFI-based Framebuffer Support
```
**Keyboard and touchpad**

```
Cryptographic API  --->
   [*]   Hardware crypto devices  --->
      [*]   Support for AMD Secure Processor
      <*>     Secure Processor device driver
      [*]       Platform Security Processor (PSP) device
Device Drivers  --->
   I2C Support  --->
      <*> I2C support
      <*>   I2C device interface
      I2C Hardware Bus Support  --->
         <*> Intel PIIX4 and compatible (ATI/AMD/Serverworks/Broadcom/SMSC)
         <*> Synopsys DesignWare Platform
         [*]   AMD PSP I2C semaphore support
   [*] Pin controllers  --->
      [*]   AMD GPIO pin control
   [*] HID bus support  --->
      <*>   Generic HID driver
            Special HID drivers  --->
               <*> HID Multitouch panels
            USB HID support  --->
               <*> USB HID transport layer
      <*>   I2C HID support  --->
               <M>   HID over I2C transport layer ACPI driver
```
**USB4/Thunderbolt/Type-C**

```
Device Drivers  --->
   [*] USB support  --->
      <*>   USB Type-C Support  --->
         <*>   USB Type-C Connector System Software Interface driver
         <*>     UCSI ACPI Interface Driver
               USB Type-C Alternate Mode drivers  --->
            <*> DisplayPort Alternate Mode driver
      <*>   USB Role Switch Support
   <*> Unified support for USB4 and Thunderbolt  --->
```
**Sound card**

```
Device Drivers  --->
   <M> Sound card support  --->
      <M> Advanced Linux Sound Architecture  --->
         HD-Audio  --->
            <M> HD Audio PCI
            <M> Build Realtek HD-audio codec support
            <M> Build HDMI/DisplayPort HD-audio codec support
         <M>   ALSA for SoC audio support  --->
            <M>   AMD Audio Coprocessor-v3.x support
            <M>   AMD Audio Coprocessor - Renoir support
            <M>   AMD Audio Coprocessor-v5.x I2S support
            <M>   AMD Audio Coprocessor-v6.x Yellow Carp support
            <M>   AMD Audio Coprocessor-v6.2 RPL support
            <M>   AMD Audio Coprocessor-v6.3 Pink Sardine support
            [*]   Sound Open Firmware Support  --->
               <M>   SOF PCI enumeration support
               <M>   SOF support for AMD audio DSPs
               <M>     SOF support for RENOIR
               <M>     SOF support for VANGOGH
               <M>     SOF support for REMBRANDT
```
**Webcam**

```
Device Drivers  --->
   <M> Multimedia support  --->
      [ ]   Filter media drivers
            Media core support  --->
               <M> Video4Linux core
            Media drivers  --->
               [*] Media USB Adapters  --->
                  <M>   USB Video Class (UVC)
```
**Ambient light sensor**

```
Device Drivers  --->
   [*] HID bus support  --->
      [*]   HID bus core support
              Special HID drivers  --->
                 [*] HID Sensors framework support
   [*] Industrial I/O support  --->
            Light sensors  --->
               [*] HID ALS
```
**Embedded Controller**

```
Device Drivers  --->
   [*] Platform support for Chrome hardware  --->
      <*>   ChromeOS Embedded Controller
      <*>     ChromeOS Embedded Controller (LPC)
      <*>   ChromeOS EC miscdevice
```
**Ethernet Expansion Card**

```
Device Drivers  --->
   [*] Network device support  --->
      <M>   USB Network Adapters  --->
         <M>   Realtek RTL8152/RTL8153 Based USB Ethernet Adapters
```
**Storage Expansion Card**

```
Device Drivers  --->
   [*] USB support  --->
      <*>   USB Mass Storage support
      <M>     USB Attached SCSI
```
**LED Matrix**

```
Device Drivers  --->
   [*] USB support  --->
      <M>   USB Modem (CDC ACM) support
```
**Needed to get RyzenAdj v0.15.0 working**

```
Kernel hacking  --->
   [ ] Filter access to /dev/mem
```
For more features on battery charge limit and LED configuration an out-of-tree kernel module can be installed via

`root #``emerge --ask app-laptop/framework-laptop-kmod`
### Setup

#### Bluetooth

Enable the bluetooth service. See [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) if the service does not exist.

`root #``systemctl enable --now bluetooth`
#### Sound

Enable pipewire, pipewire-pulse, and wireplumber. These commands add your user to the pipewire group, enable services for all users (next boot), and start services for your user.

`user $````
sudo usermod -aG pipewire $(whoami)
```
`user $````
sudo systemctl enable --global pipewire{,-pulse}.{service,socket} wireplumber.service
```
`user $````
systemctl start --user pipewire{,-pulse}.{service,socket} wireplumber.service
```
#### Fingerprint reader

There are a number of [fingerprint reader packages](https://wiki.gentoo.org/wiki/Fingerprint_Reader) but `fprintd` is recommended.

`root #``emerge --ask sys-auth/fprintd`
#### Fan control

A modified version of [fw-fanctl on GitHub](https://github.com/TamtamHero/fw-fanctrl.git), which also includes a version of ectool that allows to control the fans, tracks the temperature better and will speed up fans faster when temperature rises fast. It also tracks changes in the
config file, and reloads the config when it changes and contains a script for init.d as well as systemd.

To clone fw-fanctrl:

`user $``git clone` [https://github.com/NAKlama/fw-fanctrl](https://github.com/NAKlama/fw-fanctrl)`user $``cd fw-fanctrl`
To install (for OpenRC) run inside the cloned directory:

`root #``chmod +x install-initd.sh``root #``./install-initd.sh`
To list available fan curves (configured in /etc/fw-fanctrl/config.json):

`root #``fw-fanctrl --list`
To change to the fan curve `lazy` (as an example) run:

`root #``fw-fanctrl use lazy`
#### Embedded Controller

Dustin Howett has written a [kernel module](https://github.com/DHowett/framework-laptop-kmod) that exposes to sysfs the Framework Laptop's battery charge limit, LEDs, fan controls, and privacy switches. Note that, as of this writing (17-Jul-2024), you have to apply this [patch series](https://lore.kernel.org/chrome-platform/20240403004713.130365-1-dustin@howett.net/) to your kernel sources in order to add a necessary quirk to the kernel's ChromeOS EC driver.

#### CPU Power Control (RyzenAdj)

[sys-power/RyzenAdj](https://packages.gentoo.org/packages/sys-power/RyzenAdj) can be used to adjust the CPU's power settings.

To get it working, it needs access to `/dev/mem`.

#### TCG Opal SED

If you have set a password on a TCG Opal self-encrypting drive, then the drive will be locked when Linux resumes from s0ix/s2idle suspend, and your file systems will immediately crash. You can work around this by saving your Opal password into kernel memory using the `IOC_OPAL_SAVE` ioctl, whereby the kernel will automatically resubmit the key to unlock the drive before resuming tasks. Michal Gawlik's [sed-opal-unlocker](https://github.com/dex6/sed-opal-unlocker) is a userspace tool that can issue the needed ioctl.

## Configuration

### Web configuration for input modules

The QMK-based input modules (keyboard, numpad, and macropad) are configurable via [https://keyboard.frame.work](https://keyboard.frame.work) and the LED matrix is configurable via [https://ledmatrix.frame.work](https://ledmatrix.frame.work) once some udev rules are in place; `50-qmk.rules` (sourced from [GitHub](https://github.com/qmk/qmk_firmware/blob/8a429fce3364de398ef35d425ea467414e3c80d8/util/udev/50-qmk.rules)) for QMK devices and `50-framework.rules` for the LED matrix.

**`/etc/udev/rules.d/50-qmk.rules`**

```
# Atmel DFU
### ATmega16U2
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2fef", TAG+="uaccess"
### ATmega32U2
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2ff0", TAG+="uaccess"
### ATmega16U4
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2ff3", TAG+="uaccess"
### ATmega32U4
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2ff4", TAG+="uaccess"
### AT90USB64
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2ff9", TAG+="uaccess"
### AT90USB162
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2ffa", TAG+="uaccess"
### AT90USB128
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2ffb", TAG+="uaccess"
# Input Club
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1c11", ATTRS{idProduct}=="b007", TAG+="uaccess"
# STM32duino
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1eaf", ATTRS{idProduct}=="0003", TAG+="uaccess"
# STM32 DFU
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", TAG+="uaccess"
# BootloadHID
SUBSYSTEMS=="usb", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="05df", TAG+="uaccess"
# USBAspLoader
SUBSYSTEMS=="usb", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="05dc", TAG+="uaccess"
# USBtinyISP
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1782", ATTRS{idProduct}=="0c9f", TAG+="uaccess"
# ModemManager should ignore the following devices
# Atmel SAM-BA (Massdrop)
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="6124", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
# Caterina (Pro Micro)
## pid.codes shared PID
### Keyboardio Atreus 2 Bootloader
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1209", ATTRS{idProduct}=="2302", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
## Spark Fun Electronics
### Pro Micro 3V3/8MHz
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1b4f", ATTRS{idProduct}=="9203", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
### Pro Micro 5V/16MHz
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1b4f", ATTRS{idProduct}=="9205", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
### LilyPad 3V3/8MHz (and some Pro Micro clones)
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1b4f", ATTRS{idProduct}=="9207", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
## Pololu Electronics
### A-Star 32U4
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1ffb", ATTRS{idProduct}=="0101", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
## Arduino SA
### Leonardo
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0036", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
### Micro
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0037", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
## Adafruit Industries LLC
### Feather 32U4
SUBSYSTEMS=="usb", ATTRS{idVendor}=="239a", ATTRS{idProduct}=="000c", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
### ItsyBitsy 32U4 3V3/8MHz
SUBSYSTEMS=="usb", ATTRS{idVendor}=="239a", ATTRS{idProduct}=="000d", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
### ItsyBitsy 32U4 5V/16MHz
SUBSYSTEMS=="usb", ATTRS{idVendor}=="239a", ATTRS{idProduct}=="000e", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
## dog hunter AG
### Leonardo
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2a03", ATTRS{idProduct}=="0036", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
### Micro
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2a03", ATTRS{idProduct}=="0037", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
# hid_listen
KERNEL=="hidraw*", MODE="0660", GROUP="plugdev", TAG+="uaccess", TAG+="udev-acl"
# hid bootloaders
## QMK HID
SUBSYSTEMS=="usb", ATTRS{idVendor}=="03eb", ATTRS{idProduct}=="2067", TAG+="uaccess"
## PJRC's HalfKay
SUBSYSTEMS=="usb", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="0478", TAG+="uaccess"
# APM32 DFU
SUBSYSTEMS=="usb", ATTRS{idVendor}=="314b", ATTRS{idProduct}=="0106", TAG+="uaccess"
# GD32V DFU
SUBSYSTEMS=="usb", ATTRS{idVendor}=="28e9", ATTRS{idProduct}=="0189", TAG+="uaccess"
# WB32 DFU
SUBSYSTEMS=="usb", ATTRS{idVendor}=="342d", ATTRS{idProduct}=="dfa0", TAG+="uaccess"
```
**`/etc/udev/rules.d/50-framework.rules`**

```
# LED Matrix, ModemManager should ignore
SUBSYSTEMS=="usb", ATTRS{idVendor}=="32ac", ATTRS{idProduct}=="0020", TAG+="uaccess", ENV{ID_MM_DEVICE_IGNORE}="1"
```
Once one or both of these files have been added, run the following:

`root #````
udevadm control --reload-rules
```
`root #````
udevadm trigger
```
## See also

- [Framework Expansion Cards](https://wiki.gentoo.org/wiki/Framework_Expansion_Cards)
- [Framework Laptop 13](https://wiki.gentoo.org/wiki/Framework_Laptop_13) — a highly modular and repairable 13 inch laptop
