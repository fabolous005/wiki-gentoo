<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_P50 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad P50 -->
---
title: Lenovo ThinkPad P50
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_P50
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "3f5897067f221069"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad P50

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Overview

`root #``lscpu`
Architecture:          x86\_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 94
Model name:            Intel(R) Xeon(R) CPU E3-1505M v5 @ 2.80GHz
Stepping:              3
CPU MHz:               1750.000
CPU max MHz:           3700.0000
CPU min MHz:           800.0000
BogoMIPS:              5619.86
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              8192K
NUMA node0 CPU(s):     0-7
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch epb intel\_pt tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm mpx rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Intel Corporation Skylake Host Bridge/DRAM Registers \[8086:1918\] (rev 07)
	Subsystem: Lenovo Skylake Host Bridge/DRAM Registers \[17aa:222e\]
00:01.0 PCI bridge \[0604\]: Intel Corporation Skylake PCIe Controller (x16) \[8086:1901\] (rev 07)
	Kernel driver in use: pcieport
00:02.0 VGA compatible controller \[0300\]: Intel Corporation HD Graphics P530 \[8086:191d\] (rev 06)
	Subsystem: Lenovo HD Graphics P530 \[17aa:222e\]
	Kernel driver in use: i915
00:14.0 USB controller \[0c03\]: Intel Corporation Sunrise Point-H USB 3.0 xHCI Controller \[8086:a12f\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H USB 3.0 xHCI Controller \[17aa:222e\]
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
00:14.2 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-H Thermal subsystem \[8086:a131\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H Thermal subsystem \[17aa:222e\]
00:16.0 Communication controller \[0780\]: Intel Corporation Sunrise Point-H CSME HECI #1 \[8086:a13a\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H CSME HECI \[17aa:222e\]
	Kernel driver in use: mei\_me
	Kernel modules: mei\_me
00:17.0 SATA controller \[0106\]: Intel Corporation Sunrise Point-H SATA controller \[AHCI mode\] \[8086:a102\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H SATA controller \[AHCI mode\] \[17aa:222e\]
	Kernel driver in use: ahci
	Kernel modules: ahci
00:1c.0 PCI bridge \[0604\]: Intel Corporation Sunrise Point-H PCI Express Root Port #1 \[8086:a110\] (rev f1)
	Kernel driver in use: pcieport
00:1c.2 PCI bridge \[0604\]: Intel Corporation Sunrise Point-H PCI Express Root Port #3 \[8086:a112\] (rev f1)
	Kernel driver in use: pcieport
00:1c.4 PCI bridge \[0604\]: Intel Corporation Sunrise Point-H PCI Express Root Port #5 \[8086:a114\] (rev f1)
	Kernel driver in use: pcieport
00:1d.0 PCI bridge \[0604\]: Intel Corporation Sunrise Point-H PCI Express Root Port #9 \[8086:a118\] (rev f1)
	Kernel driver in use: pcieport
00:1d.4 PCI bridge \[0604\]: Intel Corporation Sunrise Point-H PCI Express Root Port #13 \[8086:a11c\] (rev f1)
	Kernel driver in use: pcieport
00:1f.0 ISA bridge \[0601\]: Intel Corporation Sunrise Point-H LPC Controller \[8086:a150\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H LPC Controller \[17aa:222e\]
00:1f.2 Memory controller \[0580\]: Intel Corporation Sunrise Point-H PMC \[8086:a121\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H PMC \[17aa:222e\]
00:1f.3 Audio device \[0403\]: Intel Corporation Sunrise Point-H HD Audio \[8086:a170\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H HD Audio \[17aa:222e\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:1f.4 SMBus \[0c05\]: Intel Corporation Sunrise Point-H SMBus \[8086:a123\] (rev 31)
	Subsystem: Lenovo Sunrise Point-H SMBus \[17aa:222e\]
	Kernel driver in use: i801\_smbus
00:1f.6 Ethernet controller \[0200\]: Intel Corporation Ethernet Connection (2) I219-LM \[8086:15b7\] (rev 31)
	Subsystem: Lenovo Ethernet Connection (2) I219-LM \[17aa:2233\]
	Kernel driver in use: e1000e
	Kernel modules: e1000e
01:00.0 VGA compatible controller \[0300\]: NVIDIA Corporation GM107GLM \[Quadro M2000M\] \[10de:13b0\] (rev ff)
	Kernel modules: nvidia\_drm, nvidia
04:00.0 Network controller \[0280\]: Intel Corporation Wireless 8260 \[8086:24f3\] (rev 3a)
	Subsystem: Intel Corporation Wireless 8260 \[8086:0130\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
3d:00.0 USB controller \[0c03\]: Intel Corporation DSL6340 USB 3.1 Controller \[Alpine Ridge\] \[8086:15b5\] (rev ff)
	Kernel modules: xhci\_pci
3e:00.0 Non-Volatile memory controller \[0108\]: Samsung Electronics Co Ltd Device \[144d:a804\]
	Subsystem: Samsung Electronics Co Ltd Device \[144d:a801\]
	Kernel driver in use: nvme
3f:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader \[10ec:525a\] (rev 01)
	Subsystem: Lenovo RTS525A PCI Express Card Reader \[17aa:222e\]
	Kernel driver in use: rtsx\_pci
	Kernel modules: rtsx\_pci

`root #``lsusb -t````
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/10p, 5000M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/16p, 480M
    |__ Port 3: Dev 39, If 12, Class=Communications, Driver=cdc_mbim, 480M
    |__ Port 3: Dev 39, If 13, Class=CDC Data, Driver=cdc_mbim, 480M
    |__ Port 8: Dev 5, If 0, Class=Video, Driver=uvcvideo, 480M
    |__ Port 8: Dev 5, If 1, Class=Video, Driver=uvcvideo, 480M
    |__ Port 9: Dev 8, If 0, Class=Vendor Specific Class, Driver=, 12M
    |__ Port 11: Dev 11, If 0, Class=Chip/SmartCard, Driver=usbfs, 12M
    |__ Port 13: Dev 14, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M
    |__ Port 14: Dev 15, If 0, Class=Wireless, Driver=btusb, 12M
    |__ Port 14: Dev 15, If 1, Class=Wireless, Driver=btusb, 12M
```
`root #``lsusb`
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 008: ID 138a:0090 Validity Sensors, Inc. 
Bus 001 Device 005: ID 04f2:b52c Chicony Electronics Co., Ltd 
Bus 001 Device 039: ID 1199:9079 Sierra Wireless, Inc. 
Bus 001 Device 015: ID 8087:0a2b Intel Corp. 
Bus 001 Device 014: ID 0765:5010 X-Rite, Inc. X-Rite Pantone Color Sensor
Bus 001 Device 011: ID 058f:9540 Alcor Micro Corp. AU9540 Smartcard Reader
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### Power management

Make sure you have "Hybrid Graphics" enabled in the BIOS, and are using the proprietary NVIDIA driver. Make sure all of these packages are installed:

First add a udev rule for putting devices to sleep when not in use:

**`/etc/udev/rules.d/90-power.rules`**

You'll also want the sound card to sleep when unused, to have direct fan control (just for fun), and to not automatically load the NVIDIA driver:

**`/etc/modprobe.d/power.conf`**

Disable the NMI watchdog so that the CPU can go into deeper sleep states:

**`/etc/sysctl.d/99-power.conf`**

Make [sys-power/bbswitch](https://packages.gentoo.org/packages/sys-power/bbswitch) load at boot time:

`root #````
echo bbswitch > /etc/modules-load.d/10-bbswitch.conf
```
`root #````
modprobe bbswitch
```
The Intel graphics card requires firmware available at initramfs time to enable its power management features, so be sure `/lib/firmware/i915` is included in your initramfs.

Read onward with the NVIDIA Optimus configuration below to complete the power savings.

### NVIDIA Optimus

Assuming you've enabled "Hybrid Graphics" in the BIOS, emerged the NVIDIA proprietary driver, emerged and configured bbswitch as in the power management section above, and emerged the Intel OpenCL SDK, continue here to get hybrid graphics working well.

Make sure generally your portage configuration is optimal:

**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptic
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel nvidia i965
```
When in hybrid graphics mode, the laptop's panel is connected to the Intel card. So, using the Intel card works as usual, but the NVIDIA card requires some tricks.

Assuming you're using [SDDM](https://wiki.gentoo.org/wiki/SDDM) as the display manager, put the following in your Xsetup, which turns off the NVIDIA card to save power, when not in use, and otherwise instructs the NVIDIA card to use the screen attached to the Intel card when in use:

**`/usr/share/sddm/scripts/Xsetup`**

```
#!/bin/sh
PATH="/sbin:/usr/sbin:/bin:/usr/bin"
if xrandr --listproviders | grep -q NVIDIA; then 
        xrandr --setprovideroutputsource modesetting NVIDIA-0
        xrandr --auto
else
        rmmod nvidia_uvm
        rmmod nvidia_drm
        rmmod nvidia_modeset
        rmmod nvidia
        echo OFF > /proc/acpi/bbswitch
fi
```
Now install these various configuration files:

**`/etc/X11/xinit/xinitrc.d/95-nvidia-settings`**

**`/etc/modprobe.d/nvidia-drm.conf`**

**`/etc/modprobe.d/nvidia-rmmod.conf`**

**`/etc/X11/xorg.conf.intel`**

**`/etc/X11/xorg.conf.nvidia`**

When using [media-video/mpv](https://packages.gentoo.org/packages/media-video/mpv):

**`/etc/mpv/mpv.conf.intel`**

**`/etc/mpv/mpv.conf.nvidia`**

Finally, add this script:

**`/usr/local/bin/nvidia`**

```
#!/bin/bash
 
[[ $UID != 0 ]] && exec sudo "$0" "$@"
 
on() {
        [ -f /usr/share/eselect/modules/opengl.eselect ] && eselect opengl set nvidia
        [ -f /usr/share/eselect/modules/opencl.eselect ] && eselect opencl set nvidia
        ln -svfn xorg.conf.nvidia /etc/X11/xorg.conf
        ln -svnf mpv.conf.nvidia /etc/mpv/mpv.conf
        echo ON > /proc/acpi/bbswitch
}
 
off() {
        [ -f /usr/share/eselect/modules/opengl.eselect ] && eselect opengl set xorg-x11
        [ -f /usr/share/eselect/modules/opencl.eselect ] && eselect opencl set intel
        ln -svfn xorg.conf.intel /etc/X11/xorg.conf
        ln -svnf mpv.conf.intel /etc/mpv/mpv.conf
}
 
case "$1" in
        on) on ;;
        off) off ;;
        *) echo "Usage: $0 on|off" >&2 ;;
esac
```
Mark this executable and enable the Intel card to start:

`root #````
chmod +x /usr/local/bin/nvidia
```
`root #````
nvidia off
```
When you want to use the NVIDIA card, simply run this command with `on` and restart sddm:

`root #````
nvidia on
```
`root #````
systemctl restart sddm
```
When you want to use the Intel card, simply run this command with `off` and restart sddm:

`root #````
nvidia off
```
`root #````
systemctl restart sddm
```
Note that you'll need to use the NVIDIA card in order to drive external displays.

### Remapping useless FN keys

The keys `FN`-`F9` to `FN`-`F12` aren't particularly useful out of the box. A nice application is to (low level) remap the keys to media buttons. This can be done via udev:

**`/etc/udev/hwdb.d/99-internal-keyboard.hwdb`**

**`/etc/udev/hwdb.d/99-external-keyboard.hwdb`**

Update the database as described [here](https://wiki.gentoo.org/wiki/Udev#Remapping_keys_and_buttons) and reboot the PC.

#### Toggle touchpad with FN+F4

To toggle the touchpad with `FN`+`F4` add an additional line:

**`/etc/udev/hwdb.d/99-keyboard.hwdb`**

### Thunderbolt connector

As the thunderbolt controller is just PCIe, you don't need to do anything at all. Have PCI express hotplugging enabled in your kernel. When you insert a thunderbolt device, the BIOS will enumerate it, and simply hotplug a PCIe device, just like express port.

Similarly, if you use the port for USB 3.1 via the USB-C connector, a new XHCI controller will be hotplugged, which will have your USB device connected to it, at USB 3.1 gen 2 speeds.

### TPM (and suspend/resume)

A common problem can be encountered if the TPM chip has been activated in UEFI/BIOS. If the kernel lacks the necessary drivers the system freezes when attempting to resume from a suspend. Simply configuring the kernel with TPM supports resolves the problem:

**TPM**

### Trackpoint and Touchpad

There are various nobs for the trackpoint and touchpad:

**`/etc/udev/rules.d/90-trackpoint.rules`**

**`/etc/X11/xorg.conf.d/20-touchpad.conf`**

**`/etc/X11/xorg.conf.d/20-trackpoint.conf`**
