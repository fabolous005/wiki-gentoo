<!-- source: https://wiki.gentoo.org/wiki/Framework_Laptop_16 | group: Gentoo Wiki (Main) | wiki-title: Framework Laptop 16 -->
---
title: Framework Laptop 16
url: https://wiki.gentoo.org/wiki/Framework_Laptop_16
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-05"
fingerprint: "4f464e7700f211d9"
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
| CPU | AMD Ryzen 9 7940HS AMD Ryzen 7 7840HS |  |  |  |  | [AMD microcode](https://wiki.gentoo.org/wiki/AMD_microcode) | 
| Chipset | AMD Pink Sardine |  |  |  |  |  | 
| Video card | AMD Phoenix1 |  | 1002:15bf | amdgpu |  | Use `VIDEO_CARDS: amdgpu radeonsi` | 
| Sound card | AMD Rembrandt Radeon High Definition Audio Controller |  | 1002:1640 | snd\_hda\_intel |  |  | 
| Sound card | AMD Family 17h/19h HD Audio Controller |  | 1022:15e3 | snd\_hda\_intel |  |  | 
| Audio coprocessor | AMD ACP/ACP3X/ACP6x Audio Coprocessor |  | 1022:15e2 | snd\_pci\_ps |  | Also requires Realtek HD-audio codec | 
| Wireless network card | MediaTek MT7922 |  | 14c3:0616 | mt7921e |  |  | 
| Bluetooth | MediaTek MT7922 |  | 0e8d:e616 | btusb |  |  | 
| Fingerprint Reader | Goodix USB2.0 MISC |  | 27c6:609c |  |  | Requires [a fingerprint reader package](https://wiki.gentoo.org/wiki/Fingerprint_Reader) | 
| Webcam | Realtek Semiconductor Corp. Laptop Camera |  | 0bda:5634 | uvcvideo |  | The microphone is attached to the sound card (17h/19h), not the webcam | 
| Thunderbolt/USB4 | AMD Pink Sardine USB4/Thunderbolt NHI controller |  | 1022:1668 1022:1669 | thunderbolt |  | Compatibility with complex devices such as docks and eGPUs has not been confirmed. | 
| Ambient light sensor |  |  | 32ac:001b (Framework's HID sensor hub) | hid\_sensor\_als, hid\_sensor\_hub |  | Found at: `/sys/bus/iio/devices/iio:device0/` | 
| Encryption | AMD Family 19h (Model 74h) CCP/PSP 3.0 Device |  | 1022:15c7 | ccp |  | Tested with `cryptsetup benchmark` | 
| AI accelerator | AMD IPU Device |  | 1022:1502 |  |  | Requires `dev-libs/xdna-driver` from the [GURU repository](https://wiki.gentoo.org/wiki/Project:GURU) | 

### Input modules

The Framework Laptop 16 has a user-configurable input deck. Unless otherwise noted, the modules listed below are OEM modules from Framework.

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Keyboard | Framework Laptop 16 Keyboard Module - ANSI |  | 32ac:0012 |  |  | Generic HID? | 
| Touchpad | PixArt PIXA3854 |  | 093A:0274 | hid\_multitouch i2c\_designware\_platform pinctrl\_amd |  |  | 
| Numpad | Framework Laptop 16 Numpad Module |  | 32ac:0014 |  |  | Generic HID? | 
| RGB Macropad |  |  | 32ac:0013 |  |  | Generic HID? | 
| LED Matrix |  |  | 32ac:0020 | cdc\_acm |  |  | 

Configuring the QMK devices and LED matrix via web tools requires udev rules (see below).

### Expansion cards

**See [Framework Expansion Cards](https://wiki.gentoo.org/wiki/Framework_Expansion_Cards).** The Framework Laptop 16 features six modular expansion card slots allowing for custom port configurations.

### Expansion bay

The Framework 16 has an expansion bay with a custom PCIe connector. Unless otherwise noted, the modules listed below are OEM modules from Framework.

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes |  | 
|---|---|---|---|---|---|---|---|
|  | Expansion Bay Shell |  |  |  |  | Provides cooling and fills the bay. Fans are managed by the embedded controller. |  | 
| GPU | AMD Radeon RX 7700S |  | 1002:7480 1002:ab30 | amdgpu snd\_hda\_intel |  |  |  | 
| GPU | NVIDIA RTX 5070 8GiB Max-Q / Mobile (GB206M) |  | 10de:2d58 10de:22eb | nvidia-drivers USE=+kernel-open |  |  |  | 

## Installation

See the [Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) for instructions on the general installation process.

### Firmware

Firmware from [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) is needed for the GPU, wireless, and bluetooth interfaces.

### Kernel

The easiest way to configure the kernel is use a distribution kernel. The next easiest way is to use [genkernel](https://wiki.gentoo.org/wiki/Genkernel). See [Kernel installation](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel) in the handbook for details.

genkernel automatically enables enough to get the system booted, but additional changes are required to get all hardware working. On the upside, `genkernel all` does everything - it compiles the kernel, builds an initramfs, and installs both in the boot partition, and it can be configured to update the bootloader entries.

**Note: compiling certain drivers into the kernel (vs as a module) may cause problems.** Some drivers such as the Wi-Fi card driver (MediaTek MT7921E) will not boot up correctly if firmware is not available. This can be resolved by configuring the driver to be a module, or by including the firmware in the initramfs.

**General**

**NVMe/SSD slots**

**Wi-Fi and Bluetooth**

**Graphics**

**Keyboard and touchpad**

**USB4/Thunderbolt/Type-C**

**Sound card**

**Webcam**

**Ambient light sensor**

**Embedded Controller**

**Ethernet Expansion Card**

**Storage Expansion Card**

**LED Matrix**

**Needed to get RyzenAdj v0.15.0 working**

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
