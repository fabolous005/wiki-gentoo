<!-- source: https://wiki.gentoo.org/wiki/Dell_Inspiron_15_5515 | group: Gentoo Wiki (Main) | wiki-title: Dell Inspiron 15 5515 -->
---
title: Dell Inspiron 15 5515
url: https://wiki.gentoo.org/wiki/Dell_Inspiron_15_5515
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: fe06add582f2902c
license: CC BY-SA 4.0
---

# Dell Inspiron 15 5515

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Dell Inspiron 15 5515** is a laptop manufactured by Dell Technologies.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen 5 5500U/5700U | Works | N/A | N/A | 5.15.26 |  | 
| Video card | AMD Lucienne \[Radeon Vega 7/8\] | Works | 1002:1637 | amdgpu | 5.15.26 |  | 
| Touchpad | Dell | Works | 27C6:0D42 | i2c-hid | 5.15.26 | Requires specific configuration, see below | 
| Touch screen | Dell | Works | 04F3:2C6B | i2c-hid | 5.15.26 | Requires specific configuration, see below | 
| Fingerprint Reader | Goodix MOC Fingerprint sensor | Works | 27C6:639C | N/A | 5.15.26 | Works with [sys-auth/fprintd](https://packages.gentoo.org/packages/sys-auth/fprintd) | 
| Webcam | Microdia | Works | 0C45:6725 | uvcvideo | 5.15.26 |  | 
| Microphone | Dell | Works | N/A | N/A | 5.15.26 |  | 
| Wi-Fi | [Qualcomm Atheros QCA6174](https://wiki.gentoo.org/wiki/Qualcomm_Atheros_QCA6174) | Works | 168C:003E | ath10k\_pci | 5.15.26 |  | 
| USB | AMD Renoir/Cezanne USB 3.1 | Works | 1022:1639 | xhci\_hcd | 5.15.26 |  | 
| TPM | fTPM 2.0 | Works | N/A | tpm\_crb, tpm\_tis | 5.15.26 |  | 

### Accessories

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Dock | Dell WD19S | Works | N/A | N/A | 5.15.26 | The power button on the dock does not work (it does nothing) | 

### Detailed information

`root #``lspci`
00:00.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir/Cezanne Root Complex
00:00.2 IOMMU: Advanced Micro Devices, Inc. \[AMD\] Renoir/Cezanne IOMMU
00:01.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe Dummy Host Bridge
00:01.2 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir/Cezanne PCIe GPP Bridge
00:02.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe Dummy Host Bridge
00:02.2 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir/Cezanne PCIe GPP Bridge
00:08.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe Dummy Host Bridge
00:08.1 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Internal PCIe GPP Bridge to Bus
00:08.2 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Internal PCIe GPP Bridge to Bus
00:14.0 SMBus: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller (rev 51)
00:14.3 ISA bridge: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge (rev 51)
00:18.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 0
00:18.1 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 1
00:18.2 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 2
00:18.3 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 3
00:18.4 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 4
00:18.5 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 5
00:18.6 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 6
00:18.7 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 7
01:00.0 Non-Volatile memory controller: KIOXIA Corporation Device 0001
02:00.0 Network controller: Qualcomm Atheros QCA6174 802.11ac Wireless Network Adapter (rev 32)
03:00.0 VGA compatible controller: Advanced Micro Devices, Inc. \[AMD/ATI\] Lucienne (rev c2)
03:00.1 Audio device: Advanced Micro Devices, Inc. \[AMD/ATI\] Renoir Radeon High Definition Audio Controller
03:00.2 Encryption controller: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) Platform Security Processor
03:00.3 USB controller: Advanced Micro Devices, Inc. \[AMD\] Renoir/Cezanne USB 3.1
03:00.4 USB controller: Advanced Micro Devices, Inc. \[AMD\] Renoir/Cezanne USB 3.1
03:00.5 Multimedia controller: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2/FireFlight/Renoir Audio Processor (rev 01)
03:00.6 Audio device: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) HD Audio Controller
04:00.0 SATA controller: Advanced Micro Devices, Inc. \[AMD\] FCH SATA Controller \[AHCI mode\] (rev 81)
04:00.1 SATA controller: Advanced Micro Devices, Inc. \[AMD\] FCH SATA Controller \[AHCI mode\] (rev 81)

`user $``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: AuthenticAMD
cpu family	: 23
model		: 104
model name	: AMD Ryzen 5 5500U with Radeon Graphics
stepping	: 1
microcode	: 0x8608103
cpu MHz		: 2100.000
cache size	: 512 KB
physical id	: 0
siblings	: 12
core id		: 0
cpu cores	: 6
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu\_exception	: yes
cpuid level	: 16
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr\_opt pdpe1gb rdtscp lm constant\_tsc rep\_good nopl nonstop\_tsc cpuid extd\_apicid aperfmperf rapl pni pclmulqdq monitor ssse3 fma cx16 sse4\_1 sse4\_2 movbe popcnt aes xsave avx f16c rdrand lahf\_lm cmp\_legacy svm extapic cr8\_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt tce topoext perfctr\_core perfctr\_nb bpext perfctr\_llc mwaitx cpb cat\_l3 cdp\_l3 hw\_pstate ssbd mba ibrs ibpb stibp vmmcall fsgsbase bmi1 avx2 smep bmi2 cqm rdt\_a rdseed adx smap clflushopt clwb sha\_ni xsaveopt xsavec xgetbv1 xsaves cqm\_llc cqm\_occup\_llc cqm\_mbm\_total cqm\_mbm\_local clzero irperf xsaveerptr rdpru wbnoinvd arat npt lbrv svm\_lock nrip\_save tsc\_scale vmcb\_clean flushbyasid decodeassists pausefilter pfthreshold avic v\_vmsave\_vmload vgif v\_spec\_ctrl umip rdpid overflow\_recov succor smca
bugs		: sysret\_ss\_attrs spectre\_v1 spectre\_v2 spec\_store\_bypass
bogomips	: 4192.33
TLB size	: 3072 4K pages
clflush size	: 64
cache\_alignment	: 64
address sizes	: 48 bits physical, 48 bits virtual
power management: ts ttp tm hwpstate cpb eff\_freq\_ro \[13\] \[14\]

## Installation

### Firmware

The [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) driver requires the following [firmware](https://wiki.gentoo.org/wiki/Linux_firmware):

- amdgpu/renoir\_sdma.bin
- amdgpu/renoir\_asd.bin
- amdgpu/renoir\_ta.bin
- amdgpu/renoir\_pfp.bin
- amdgpu/renoir\_me.bin
- amdgpu/renoir\_ce.bin
- amdgpu/renoir\_rlc.bin
- amdgpu/renoir\_mec.bin
- amdgpu/renoir\_dmcub.bin
- amdgpu/renoir\_vcn.bin


The Atheros driver requires the following firmware:

- ath10k/QCA6174/hw3.0/firmware-6.bin
- ath10k/QCA6174/hw3.0/board-2.bin

### Kernel

#### Touchpad & touch screen

For the touchpad and touch screen to work correctly, the following drivers are needed:

KERNEL **Enable support for touchpad and touch screen**

```
Processor type and features  --->
    [*] AMD ACPI2Platform devices support
Device Drivers  --->
    -*- Pin controllers  --->
        <*> AMD GPIO pin control
    HID support  --->
        Special HID drivers  --->
            <*> HID Multitouch panels
        I2C HID support  --->
            <*> HID over I2C transport layer
    I2C support  --->
        I2C Hardware Bus support  --->
            <*> AMD MP2 PCIe
            <*> Synopsys DesignWare Platform
            <*> Synopsys DesignWare PCI
    Input device support  --->
        Mice  --->
            <*> ELAN I2C Touchpad support
            [*] Enable I2C support
            [*] Enable SMbus support
```
#### Webcam

For the webcam, the following drivers are required:

KERNEL **Enable support for the webcam**

```
Device Drivers  --->
    [*] Multimedia support  --->
        [*] Filter media drivers
            Media Device Types  --->
                [*] Cameras and video grabbers
            Video4Linux options  --->
                [*] V4L2 sub-device userspace API
            Media Drivers  --->
                [*] USB Video Class (UVC)
                [*]   UVC input events device support
```
#### Sound & Microphone

For sound and the microphone to work, you need to enable the AMD Renoir audio drivers:

KERNEL **Enable audio support**

```
Device Drivers  --->
     <*> Sound card support  --->
        <*>   Advanced Linux Sound Architecture  --->
            [*]   PCI sound devices  --->
            HD-Audio  --->
                <*> HD Audio PCI
                <*> Build Realtek HD-audio codec support
                <*> Build HDMI/DisplayPort HD-audio codec support
            <*> ALSA for SoC audio support  --->
                <*> AMD Audio Coprocessor-v3.x support
                <*> AMD Audio Coprocessor - Renoir support
                <*>   AMD Renoir support for DMIC
```
## Configuration

### F keys

By default, the F keypresses default to their media/Fn function, rather than the F key itself. This can be changed in the BIOS.

## Troubleshooting

### Touch pad gestures don't work, touch screen doesn't work

Makes sure the drivers from the above Kernel section are installed.
