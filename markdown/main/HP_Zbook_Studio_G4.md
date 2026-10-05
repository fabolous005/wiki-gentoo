<!-- source: https://wiki.gentoo.org/wiki/HP_Zbook_Studio_G4 | group: Gentoo Wiki (Main) | wiki-title: HP Zbook Studio G4 -->
---
title: HP Zbook Studio G4
url: https://wiki.gentoo.org/wiki/HP_Zbook_Studio_G4
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "3f509f3ffbaa3269"
license: CC BY-SA 4.0
---

# HP Zbook Studio G4

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

`root #``lspci -k````
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers (rev 05)
	Subsystem: Hewlett-Packard Company Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers
	Kernel driver in use: skl_uncore
00:02.0 VGA compatible controller: Intel Corporation HD Graphics 630 (rev 04)
	DeviceName: Onboard IGD
	Subsystem: Hewlett-Packard Company HD Graphics 630
	Kernel driver in use: i915
00:04.0 Signal processing controller: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem (rev 05)
	Subsystem: Hewlett-Packard Company Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem
00:14.0 USB controller: Intel Corporation 100 Series/C230 Series Chipset Family USB 3.0 xHCI Controller (rev 31)
	Subsystem: Hewlett-Packard Company 100 Series/C230 Series Chipset Family USB 3.0 xHCI Controller
	Kernel driver in use: xhci_hcd
00:14.2 Signal processing controller: Intel Corporation 100 Series/C230 Series Chipset Family Thermal Subsystem (rev 31)
	Subsystem: Hewlett-Packard Company 100 Series/C230 Series Chipset Family Thermal Subsystem
	Kernel driver in use: intel_pch_thermal
	Kernel modules: intel_pch_thermal
00:15.0 Signal processing controller: Intel Corporation 100 Series/C230 Series Chipset Family Serial IO I2C Controller #0 (rev 31)
	Subsystem: Hewlett-Packard Company 100 Series/C230 Series Chipset Family Serial IO I2C Controller
00:16.0 Communication controller: Intel Corporation 100 Series/C230 Series Chipset Family MEI Controller #1 (rev 31)
	Subsystem: Hewlett-Packard Company 100 Series/C230 Series Chipset Family MEI Controller
00:17.0 SATA controller: Intel Corporation Q170/Q150/B150/H170/H110/Z170/CM236 Chipset SATA Controller [AHCI Mode] (rev 31)
	Subsystem: Hewlett-Packard Company Q170/Q150/B150/H170/H110/Z170/CM236 Chipset SATA Controller [AHCI Mode]
	Kernel driver in use: ahci
00:1b.0 PCI bridge: Intel Corporation 100 Series/C230 Series Chipset Family PCI Express Root Port #17 (rev f1)
	Kernel driver in use: pcieport
00:1c.0 PCI bridge: Intel Corporation 100 Series/C230 Series Chipset Family PCI Express Root Port #1 (rev f1)
	Kernel driver in use: pcieport
00:1c.1 PCI bridge: Intel Corporation 100 Series/C230 Series Chipset Family PCI Express Root Port #2 (rev f1)
	Kernel driver in use: pcieport
00:1c.4 PCI bridge: Intel Corporation 100 Series/C230 Series Chipset Family PCI Express Root Port #5 (rev f1)
	Kernel driver in use: pcieport
00:1d.0 PCI bridge: Intel Corporation 100 Series/C230 Series Chipset Family PCI Express Root Port #9 (rev f1)
	Kernel driver in use: pcieport
00:1f.0 ISA bridge: Intel Corporation CM238 Chipset LPC/eSPI Controller (rev 31)
	Subsystem: Hewlett-Packard Company CM238 Chipset LPC/eSPI Controller
00:1f.2 Memory controller: Intel Corporation 100 Series/C230 Series Chipset Family Power Management Controller (rev 31)
	Subsystem: Hewlett-Packard Company 100 Series/C230 Series Chipset Family Power Management Controller
00:1f.3 Audio device: Intel Corporation CM238 HD Audio Controller (rev 31)
	Subsystem: Hewlett-Packard Company CM238 HD Audio Controller
	Kernel driver in use: snd_hda_intel
00:1f.4 SMBus: Intel Corporation 100 Series/C230 Series Chipset Family SMBus (rev 31)
	Subsystem: Hewlett-Packard Company 100 Series/C230 Series Chipset Family SMBus
	Kernel driver in use: i801_smbus
00:1f.6 Ethernet controller: Intel Corporation Ethernet Connection (2) I219-LM (rev 31)
	Subsystem: Hewlett-Packard Company Ethernet Connection (2) I219-LM
	Kernel driver in use: e1000e
01:00.0 VGA compatible controller: NVIDIA Corporation GM107GLM [Quadro M1200 Mobile] (rev a2)
        Subsystem: Hewlett-Packard Company GM107GLM [Quadro M1200 Mobile]
        Kernel driver in use: nvidia
        Kernel modules: nvidia_drm, nvidia
02:00.0 Network controller: Intel Corporation Wireless 8265 / 8275 (rev 78)
	Subsystem: Intel Corporation Dual Band Wireless-AC 8265
	Kernel driver in use: iwlwifi
03:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader (rev 01)
	Subsystem: Hewlett-Packard Company RTS525A PCI Express Card Reader
	Kernel driver in use: rtsx_pci
6f:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller SM961/PM961
	Subsystem: Samsung Electronics Co Ltd NVMe SSD Controller SM961/PM961
	Kernel driver in use: nvme
```
| Device | Make/model | Status | Kernel driver(s) | 
|---|---|---|---|
| CPU | Intel Core i7-7700HQ |  | N/A | 
| Ethernet | Intel Corporation Ethernet Connection I219-LM |  | e1000e | 
| USB | Intel Corporation Sunrise Point-H USB 3.0 xHCI Controller |  | xhci\_hcd | 
| Video card | Intel Corporation HD Graphics 630 |  | i915 | 
| Video card | NVIDIA Quadro M1200 |  | nvidia | 
| WiFi | Intel Corporation Wireless 8265 / 8275 |  | iwlwifi | 
| Sound card | Intel Corporation Device a171 |  | snd\_hda\_intel | 
| Hard drive | Samsung Electronics Co Ltd NVMe SSD Controller SM961/PM961 |  | nvme | 
| Bluetooth | Intel Corporation 0a2b |  | N/A | 
| Thunderbolt 3 | Intel Corporation Sunrise Point-H PCI Express |  | pcieport | 
| SD card slot | Realtek RTS525A |  | rtsx\_pci | 
| Webcam | HD HP Camera |  | uvcvideo | 

### Sound card

`root #``lspci | grep Audio` 00:1f.3 Audio device: Intel Corporation Device a171 (rev 31)

Be sure to enable HD Audio PCI (snd-hda-intel) and enable the codecs.

### Thunderbolt 3

Thunderbolt 3 is a hot pluggable PCIe port, with USB 3.1 support.

**Thunderbolt 3 Support**

### USB

Easy, just enable xHCI.

No need to enable EHCI or OHCI. xHCI is backwards compatible already.

### WiFi

`root #``lspci | grep Network`
02:00.0 Network controller: Intel Corporation Wireless 8265 / 8275 (rev 78)

Intel Corporation Wireless 8265 / 8275 does not work out of the box.

Look for the iwlwifi firmware in the /lib/firmware directory.

`user $``ls /lib/firmware | grep iwlwifi-8265`
iwlwifi-8265-21.ucode
iwlwifi-8265-22.ucode
iwlwifi-8265-27.ucode
...

For more information on iwlwifi configuration, see [the iwlwifi wiki](https://wiki.gentoo.org/wiki/Iwlwifi).

**Enable iwlwifi in kernel 5.4.80-gentoo-r1**

No need to enable DVM, 8265 uses [iwlmvm](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi#firmware):

To enable the Hardware WiFi button:

If the wireless button does not work, check the iwlwifi ucode version.

### SD card slot

To enable the PCIe Card Reader:

**Kernel version \<4.16**

**Kernel version >4.16**

### Webcam

Setting the following kernel parameters should be enough to be able to use the built-in webcam.

**Kernel version 4.19.97**

### Video

#### NVIDIA Optimus

Configure according to [NVIDIA/Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus) & the [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) articles.


This is the necessary Xorg configuration:

**`/etc/X11/xorg.conf.d/20-nvidia.conf`**

```
Section "Module"
    Load "modesetting"
EndSection
Section "Device"
    Identifier "nvidia"
    Driver "nvidia"
    BusID "01:00:0"
    Option "AllowEmptyInitialConfiguration"
EndSection
```
Be sure to switch the OpenGL drivers before starting X:

`root #``eselect opengl set nvidia`
#### Intel

See the [intel](https://wiki.gentoo.org/wiki/Intel) page for up-to-date kernel parameter instructions.

Configure Xorg to use `intel` driver.

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i915
```
Rebuild changed packages.

`root #``emerge -DaquN @world`
Emerge the dated Intel HD Graphics driver.

`root #``emerge -qan x11-drivers/xf86-video-intel`
Configure Xorg file.

**`/etc/X11/xorg.conf.d/10-intel.conf`**

```
Section "Device"
    Identifier "intel"
    Driver "intel"
EndSection
```
#### NVIDIA

First off, add `nvidia` to `VIDEO_CARDS` in /etc/portage/package.use

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i915 nvidia
```
Update influenced packages

`root #``emerge -DaquN @world`
Follow the [NVIDIA guide](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) for a (mostly) complete configuration.

Set the following kernel parameters to be able to use `Discrete Graphics`

**Linux 4.9.76-gentoo-r1**

The following configuration enables brightness control, forces full composition pipeline to decrease screen tearing and forces a dpi of 96, which could otherwise give problems in your [wm](https://wiki.gentoo.org/wiki/Window_manager) with varying glyph sizes.

**`/etc/X11/xorg.conf.d/10-nvidia.conf`**

```
Section "Device"
    Identifier "nvidia"
    Driver "nvidia"
    Option "RegistryDwords" "EnableBrightnessControls=1"
EndSection
Section "Screen"
    Identifier "Screen0"
    Device "nvidia card"
    Monitor "Monitor0"
    Option "metamodes" "nvidia-auto-select +0+0 {ForceFullCompositionPipeline=On}"
EndSection
Section "Monitor"
	Identifier "Monitor0"
	Option "DPI" "96 x 96"
EndSection
```
### Bluetooth

Follow the [Bluetooth article](https://wiki.gentoo.org/wiki/Bluetooth) on configuring and using Bluetooth.

### HP 3D Driveguard

HP 3D Driveguard is a feature of HP laptops that turn off the hard drive when the laptop is falling. To enable HP 3D Driveguard, follow the instructions of the [HPfall](https://wiki.gentoo.org/wiki/HPfall) article.

## Troubleshooting

### External DisplayPort monitor does not work on PCIe(Thunderbolt 3) port

When using an adapter to connect a DisplayPort monitor to the PCIe port and the monitor does not get recognized, try:

1. Make sure the adapter is not plugged into the PCIe port;
2. Disconnect the DisplayPort cable from the adapter;
3. Plug the adapter into the PCIe port;
4. Connect the DisplayPort cable.

Now it should work!

### NVIDIA brightness control

To enable brightness control, add this line to the conf file

**`/etc/X11/xorg.conf.d/10-nvidia.conf`**

```
Section "Device"
    ...
    Option "RegistryDwords" "EnableBrightnessControl=1"
EndSection
```
### Screen tearing

As the browser is most frequently used: emerge [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) with *USE* variable `hwaccel`. It fixes 99% of smooth scrolling screen tearing. Really a game changer!

For other screen tearing problems, check out [x11-misc/compton](https://packages.gentoo.org/packages/x11-misc/compton).

### Keyboard layout

To set the keyboard layout to Dvorak Programmer

#### tty

For terminal:

**`/etc/conf.d/keymaps`**

#### Graphical

For X:

**`/etc/X11/xorg.conf.d/40-keyboard.conf`**

```
Section "InputClass"
    Identifier "keyboard-all"
    Driver "evdev"
    Option "XkbLayout" "us"
    Option "XkbVariant" "dvp"
    MatchIsKeyboard "on"
EndSection
```
### Unable to suspend with Nvidia driver

If you have trouble suspending while on `Discrete Graphics`, switch to [sys-auth/consolekit](https://packages.gentoo.org/packages/sys-auth/consolekit) instead of [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind). This replaces **suspend** for **pm-suspend** and should fix the suspend issue. More instructions on switching at [Suspend and hibernate](https://wiki.gentoo.org/wiki/Suspend_and_hibernate).
