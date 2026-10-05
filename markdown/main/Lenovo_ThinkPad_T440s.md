<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T440s | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad T440s -->
---
title: Lenovo ThinkPad T440s
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T440s
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "2f189f7ddf2e956d"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad T440s

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel Core i5-4200U i5-4210U i5-4300U i7-4600U |  | N/A | N/A | 4.3 |  | 
| GPU | Intel HD Graphics 4400 |  | 8086:0a16 | i915 | 4.3 |  | 
| SATA | SATA: Intel Lynx Point-LP SATA Controller |  | 8086:9c03 | ahci | 4.3 |  | 
| Sound | Intel HD Audio |  | N/A | snd\_hda\_intel | 4.3 |  | 
| Ethernet | Intel I218-LM PCI Express Gigabit Ethernet |  | 8086:155a | e1000e | 4.3 |  | 
| Wi-Fi | Intel 7260AN 802.11ac |  | 8086:08b2 | iwlwifi | 4.3 |  | 
| Bluetooth | Intel 7260AN USB Bluetooth |  | N/A | N/A | 4.3 |  | 
| Touchpad | SynPS/2 Synaptics TouchPad |  | N/A | N/A | 4.3 |  | 
| Webcam | Lite-On Technology Corp. USB webcam |  | 04ca:7035 | N/A | 4.3 |  | 
| Fingerprint reader | Validity VFS5017 USB fingerprint reader |  | 138a:0017 | N/A | 4.3 |  | 
| Hotkeys | N/A |  | N/A | N/A | 4.3 | BIOS >= Version 2.34-1.11 | 
| Sensors | N/A |  | N/A | coretemp thinkpad\_acpi | 4.3 |  | 
| SD card reader | Realtek RTS5227 PCI-E card reader |  | 10ec:5227 | rtsx\_pci | 4.3 |  | 
| Smart card reader | Alcor Micro USB Smart Card reader |  | 058f:9540 | N/A | N/A |  | 
| NFC | Broadcom BCM20792(?) SMBus NFC Controller |  | N/A | N/A | N/A |  | 
| Wireless WAN | Ericsson N5321 Mobile Broadband HSPA+ |  | N/A | N/A | N/A |  | 

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Intel Corporation Haswell-ULT DRAM Controller \[8086:0a04\] (rev 0b)
	Subsystem: Lenovo Device \[17aa:220c\]
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Haswell-ULT Integrated Graphics Controller \[8086:0a16\] (rev 0b)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: i915
00:03.0 Audio device \[0403\]: Intel Corporation Device \[8086:0a0c\] (rev 0b)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: snd\_hda\_intel
00:14.0 USB controller \[0c03\]: Intel Corporation Lynx Point-LP USB xHCI HC \[8086:9c31\] (rev 04)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: xhci\_hcd
00:16.0 Communication controller \[0780\]: Intel Corporation Lynx Point-LP HECI #0 \[8086:9c3a\] (rev 04)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: mei\_me
	Kernel modules: mei\_me
00:19.0 Ethernet controller \[0200\]: Intel Corporation Ethernet Connection I218-LM \[8086:155a\] (rev 04)
	Subsystem: Lenovo Device \[17aa:2214\]
	Kernel driver in use: e1000e
00:1b.0 Audio device \[0403\]: Intel Corporation Lynx Point-LP HD Audio Controller \[8086:9c20\] (rev 04)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: snd\_hda\_intel
00:1c.0 PCI bridge \[0604\]: Intel Corporation Lynx Point-LP PCI Express Root Port 6 \[8086:9c1a\] (rev e4)
	Kernel driver in use: pcieport
00:1c.1 PCI bridge \[0604\]: Intel Corporation Lynx Point-LP PCI Express Root Port 3 \[8086:9c14\] (rev e4)
	Kernel driver in use: pcieport
00:1d.0 USB controller \[0c03\]: Intel Corporation Lynx Point-LP USB EHCI #1 \[8086:9c26\] (rev 04)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: ehci-pci
00:1f.0 ISA bridge \[0601\]: Intel Corporation Lynx Point-LP LPC Controller \[8086:9c43\] (rev 04)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: lpc\_ich
00:1f.2 SATA controller \[0106\]: Intel Corporation Lynx Point-LP SATA Controller 1 \[AHCI mode\] \[8086:9c03\] (rev 04)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: ahci
00:1f.3 SMBus \[0c05\]: Intel Corporation Lynx Point-LP SMBus Controller \[8086:9c22\] (rev 04)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: i801\_smbus
02:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. Device \[10ec:5227\] (rev 01)
	Subsystem: Lenovo Device \[17aa:220c\]
	Kernel driver in use: rtsx\_pci
	Kernel modules: rtsx\_pci
03:00.0 Network controller \[0280\]: Intel Corporation Wireless 7260 \[8086:08b2\] (rev 83)
	Subsystem: Intel Corporation Dual Band Wireless-AC 7260 \[8086:c270\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi

`root #``lsusb`
Bus 001 Device 003: ID 058f:9540 Alcor Micro Corp. 
Bus 001 Device 002: ID 8087:8000 Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 004: ID 04ca:7035 Lite-On Technology Corp. 
Bus 002 Device 003: ID 8087:07dc Intel Corp. 
Bus 002 Device 002: ID 138a:0017 Validity Sensors, Inc. 
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### ACPI / Power Management

| Function | Status | Kernel version | Notes | 
|---|---|---|---|
| CPU frequency scaling |  | 3.12 | Driven by intel\_pstate | 
| GPU Powersaving (RC6) |  | 3.12 | Kernel 3.12 and later additionally support PC8+ with i915.allow\_pc8=1 kernel parameter | 
| SATA Power Management (ALPM) |  | 3.12 | Transition from low power to high power state fails with ATA errors, see [https://bugzilla.kernel.org/show\_bug.cgi?id=72191](https://bugzilla.kernel.org/show_bug.cgi?id=72191) | 
| Suspend to RAM |  | 3.12 |  | 
| Suspend to disk (hibernate) |  | 3.12 |  | 
| Display backlight control |  | 3.12 | Driven by acpi\_video. Need to hold down brightness keys for longer. Some users report that acpi\_backlight=vendor kernel parameter is necessary to work around backlight control problems. | 
| Keyboard backlight control |  | 3.12 |  | 

## Installation

### Firmware

The wireless card requires external firmware (**iwlwifi-7260-16.ucode**):

`root #``emerge --ask sys-kernel/linux-firmware`
or

`root #``emerge --ask sys-firmware/iwl7260-ucode`
### Kernel

**Kernel version 3.12**

### Emerge

For the card reader:

`root #``emerge --ask sys-apps/pcsc-tools`
For the fingerprint reader:

`root #``emerge --ask sys-auth/libfprint`
For the touchpad:

`root #``emerge --ask x11-drivers/xf86-input-synaptics`
## Configuration

### nVidia Optimus / Bumblebee support

This configuration allows for switching between integrated Intel GPU and nVidia GPU using primusrun (x11-misc/primus from bumblebee overlay).

Hardware and PCI slots

`root #``lspci`
00:02.0 VGA compatible controller: Intel Corporation Haswell-ULT Integrated Graphics Controller (rev 0b) 
04:00.0 3D controller: NVIDIA Corporation GK208M \[GeForce GT 730M\] (rev a1)

X.org server configuration

**`/etc/X11/xorg.conf`**

```
Section "ServerLayout" 
     Identifier     "Layout0" 
     Screen          0 "intel" 
     InputDevice    "Keyboard0" "CoreKeyboard" 
     InputDevice    "Mouse0" "CorePointer" 
 EndSection 
 Section "Module" 
         Load "glx" 
 EndSection 
 Section "InputDevice" 
     # generated from data in "/etc/conf.d/gpm" 
     Identifier     "Mouse0" 
     Driver         "mouse" 
     Option         "Protocol" 
     Option         "Device" "/dev/input/mice" 
     Option         "Emulate3Buttons" "no" 
     Option         "ZAxisMapping" "4 5" 
 EndSection 
 Section "InputDevice" 
     Identifier     "Keyboard0" 
     Driver         "kbd" 
 EndSection 
 Section "Monitor" 
     Identifier     "Monitor0"
     VendorName     "Unknown" 
     ModelName      "Unknown" 
 EndSection 
 Section "Device" 
     Identifier     "intel" 
     Driver         "intel" 
     Option         "CustomEDID" "DFP-0:/lib/firmware/edid/1920x1080_T440s.bin" 
     BusID          "PCI:0:2:0" 
 EndSection 
 Section "Screen" 
     Identifier     "intel" 
     Device         "intel" 
     SubSection     "Display" 
         Depth       24 
         Modes      "1920x1080" 
     EndSubSection 
     Monitor        "Monitor0" 
 EndSection 
 Section "Extensions" 
     Option         "Composite" "Enable" 
 EndSection
```
Bumblebee configuration

**`/etc/bumblebee/bumblebee.conf`**

Secondary X server for nVidia

**`/etc/bumblebee/xorg.conf.nvidia`**

```
Section "Files" 
     ModulePath   "/usr/lib64/xorg/nvidia" 
     ModulePath   "/usr/lib64/xorg/modules" 
 EndSection 
 Section "ServerLayout" 
     Identifier  "Layout0" 
     Option      "AutoAddDevices" "false" 
     Option      "AutoAddGPU" "false" 
 EndSection 
 Section "Device" 
     Identifier  "DiscreteNvidia" 
     Driver      "nvidia" 
     VendorName  "NVIDIA Corporation" 
     Option "ProbeAllGpus" "false" 
     Option "NoLogo" "true" 
     Option "UseEDID" "false" 
     Option "UseDisplayDevice" "none" 
     # my settings 
     Option         "CustomEDID" "DFP-0:/lib/firmware/edid/1920x1080_T440s.bin" 
     BusID          "PCI:4:0:0" 
     Option         "UseEDID" "True" 
     Option         "ModeValidation" "NoVirtualSizeCheck" 
     Option         "ModeDebug" "True" 
 EndSection 
 # my screen 
 Section "Screen" 
     Identifier     "nvidia" 
     Device         "DiscreteNvidia" 
     Monitor        "Monitor0" 
     Option         "RenderAccel" "True" 
     Option         "NoRenderExtension" "False" 
     Option         "AllowGLXWithComposite" "True" 
     Option         "AddARGBGLXVisuals" "True" 
     Option         "DamageEvents" "True" 
     Option         "ConnectToAcpid" "True" 
     Option         "UseDisplayDevice" "none" 
     SubSection     "Display" 
         Depth       24 
         Modes      "1920x1080" 
     EndSubSection 
 EndSection 
 Section "Monitor" 
     Identifier "Monitor0" 
     VendorName "Unknown" 
     Modelname "Unknown" 
 EndSection
```
### Trackpoint scrolling

With the new clickpad in the Thinkpad T440s, button events are now software based. This causes problems when using the classical combination of Xorg drivers: `evdev` for the trackpoint and `synaptics` for the touchpad. Since trackpoint scrolling (middle button + trackpoint movement) requires coordination between the two drivers, it does not work in the default configuration.

Working around the problem requires either

- installing the patched [xf86-input-evdev-trackpoint](http://gpo.zugaina.org/x11-drivers/xf86-input-evdev-trackpoint) driver, available in the [causelay](https://github.com/causes-/causelay) overlay in [eselect-repository](https://wiki.gentoo.org/wiki/Eselect-repository) and then configuring xorg by following [this thread](https://bbs.archlinux.org/viewtopic.php?id=174127).
- using the [x11-drivers/xf86-input-libinput](https://packages.gentoo.org/packages/x11-drivers/xf86-input-libinput) driver for both the trackpoint and the touchpad.

The second method is easier so it is described below.

#### libinput way

First add [libinput](https://wiki.gentoo.org/wiki/Libinput)`INPUT_DEVICES` variable.

**`/etc/portage/make.conf`**

**Set`INPUT_DEVICES`**

```
INPUT_DEVICES="libinput"
```
After setting the `INPUT_DEVICES` variable remember to update the system using the following command so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
After the update the packages [dev-libs/libinput](https://packages.gentoo.org/packages/dev-libs/libinput) and [x11-drivers/xf86-input-libinput](https://packages.gentoo.org/packages/x11-drivers/xf86-input-libinput) should've been pulled in.

Next you need to add the following Xorg configuration file so that the driver is selected for the devices we want.

**`/etc/X11/xorg.conf.d/90-libinput.conf`**

```
Section "InputClass"
        Identifier "libinput pointer catchall"
        MatchIsPointer "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```
After a reboot/xorg-server restart, trackpoint scrolling should work.

## Troubleshooting

### Fn keys (hotkeys) do not work properly

The BIOS should be updated to at least version **2.34-1.11** to fix the problem.

**`BIOS 2.34-1.11 Changelog`**
