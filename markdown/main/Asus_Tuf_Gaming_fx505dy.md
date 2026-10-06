<!-- source: https://wiki.gentoo.org/wiki/Asus_Tuf_Gaming_fx505dy | group: Gentoo Wiki (Main) | wiki-title: Asus Tuf Gaming fx505dy -->
---
title: Asus Tuf Gaming fx505dy
url: https://wiki.gentoo.org/wiki/Asus_Tuf_Gaming_fx505dy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: ff462cd5c33a25ec
license: CC BY-SA 4.0
---

# Asus Tuf Gaming fx505dy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

`root #``lspci`
00:00.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Root Complex
00:00.2 IOMMU: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 IOMMU
00:01.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge
00:01.1 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\]
00:01.2 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\]
00:01.3 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\]
00:08.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge
00:08.1 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus A
00:08.2 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus B
00:14.0 SMBus: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller (rev 61)
00:14.3 ISA bridge: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge (rev 51)
00:18.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 0
00:18.1 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 1
00:18.2 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 2
00:18.3 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 3
00:18.4 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 4
00:18.5 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 5
00:18.6 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 6
00:18.7 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 7
01:00.0 Display controller: Advanced Micro Devices, Inc. \[AMD/ATI\] Baffin \[Radeon RX 460/560D / Pro 450/455/460/555/555X/560/560X\] (rev 
e5)
02:00.0 Non-Volatile memory controller: Micron/Crucial Technology Device 2263 (rev 03)
03:00.0 Network controller: Realtek Semiconductor Co., Ltd. RTL8821CE 802.11ac PCIe Wireless Network Adapter
04:00.0 VGA compatible controller: Advanced Micro Devices, Inc. \[AMD/ATI\] Picasso (rev c2)
04:00.1 Audio device: Advanced Micro Devices, Inc. \[AMD/ATI\] Raven/Raven2/Fenghuang HDMI/DP Audio Controller
04:00.2 Encryption controller: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) Platform Security Processor
04:00.3 USB controller: Advanced Micro Devices, Inc. \[AMD\] Raven USB 3.1
04:00.4 USB controller: Advanced Micro Devices, Inc. \[AMD\] Raven USB 3.1
04:00.6 Audio device: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) HD Audio Controller
05:00.0 SATA controller: Advanced Micro Devices, Inc. \[AMD\] FCH SATA Controller \[AHCI mode\] (rev 61)

### BIOS

### CPU

The CPU is an AMD Ryzen 5 3550H with four cores.

`user $``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: AuthenticAMD
cpu family	: 23
model		: 24
model name	: AMD Ryzen 5 3550H with Radeon Vega Mobile Gfx
stepping	: 1
microcode	: 0x8108102
cpu MHz		: 1707.098
cache size	: 512 KB
physical id	: 0
siblings	: 8
core id		: 0
cpu cores	: 4
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu\_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr\_opt pdpe1gb rdtscp lm constant\_tsc rep\_good nopl nonstop\_tsc cpuid extd\_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4\_1 sse4\_2 movbe popcnt aes xsave avx f16c rdrand lahf\_lm cmp\_legacy svm extapic cr8\_legacy abm sse4a misalignsse 3dnowprefetch osvw skinit wdt tce topoext perfctr\_core perfctr\_nb bpext perfctr\_llc mwaitx cpb hw\_pstate sme ssbd sev ibpb vmmcall fsgsbase bmi1 avx2 smep bmi2 rdseed adx smap clflushopt sha\_ni xsaveopt xsavec xgetbv1 xsaves clzero irperf xsaveerptr arat npt lbrv svm\_lock nrip\_save tsc\_scale vmcb\_clean flushbyasid decodeassists pausefilter pfthreshold avic v\_vmsave\_vmload vgif overflow\_recov succor smca
bugs		: sysret\_ss\_attrs null\_seg spectre\_v1 spectre\_v2 spec\_store\_bypass
bogomips	: 4191.95
TLB size	: 2560 4K pages
clflush size	: 64
cache\_alignment	: 64
address sizes	: 43 bits physical, 48 bits virtual
power management: ts ttp tm hwpstate eff\_freq\_ro \[13\] \[14\]

### Graphics

The CPU provides Radeon Vega Mobile graphics, and there is a Radeon RX 560X GPU ([Wikipedia:List\_of\_AMD\_graphics\_processing\_units#Radeon\_RX\_500\_Series](https://en.wikipedia.org/wiki/List_of_AMD_graphics_processing_units#Radeon_RX_500_Series)) with 4Gb of dedicated memory. Follow the instructions in the [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) article.

`root #``lspci -k -s 01:00.0````
01:00.0 Display controller: Advanced Micro Devices, Inc. [AMD/ATI] Baffin [Radeon RX 460/560D / Pro 450/455/460/555/555X/560/560X] (rev e5)
        Subsystem: ASUSTeK Computer Inc. Baffin [Radeon RX 460/560D / Pro 450/455/460/555/555X/560/560X]
        Kernel driver in use: amdgpu
        Kernel modules: amdgpu
```
Note that once the amdgpu driver is working, Xorg will use the Vega graphics by default. To use the faster RX 560 GPU, export the DRI\_PRIME variable before executing your program.

`user $``DRI_PRIME=1 glxinfo | grep -i devi`
Device: Radeon RX 560 Series (POLARIS11, DRM 3.33.0, 5.3.1-gentoo, LLVM 8.0.1) (0x67ef)

### NVME SSD

In order to boot any linux device, it may be necessary to use the following kernel parameter with the laptop's included NVME SSD.

nvme\_core.default\_ps\_max\_latency\_us=5500

The above parameter may also help if the system shows signs of intermittant stalling during frequent writes to disc (e.g. ext4 journalling), after the original SSD has been upgraded.

### Ethernet

### ACPI

### Wireless

`root #``lspci -k -s 03:00.0````
03:00.0 Network controller: Realtek Semiconductor Co., Ltd. RTL8821CE 802.11ac PCIe Wireless Network Adapter
        Subsystem: AzureWave RTL8821CE 802.11ac PCIe Wireless Network Adapter
        Kernel driver in use: rtl8821ce
        Kernel modules: 8821ce
```
The rtl8821ce wifi is not supported by Linux Kernel. The simplest way to enable it is to add [trolltoo overlay](https://github.com/dallenwilson/trolltoo) and install [net-wireless/rtl8821ce-driver](https://packages.gentoo.org/packages/net-wireless/rtl8821ce-driver):

`root #``eselect repository enable trolltoo``root #``emerge -av net-wireless/rtl8821ce-driver`
### Sound

The laptop features AMD/Realtek HD audio.

`root #``lspci -k -s 04:00.6````
04:00.6 Audio device: Advanced Micro Devices, Inc. [AMD] Family 17h (Models 10h-1fh) HD Audio Controller
        Subsystem: ASUSTeK Computer Inc. Family 17h (Models 10h-1fh) HD Audio Controller
        Kernel driver in use: snd_hda_intel
```
Configure [ALSA](https://wiki.gentoo.org/wiki/ALSA) with the following driver settings:

```
Device Drivers --->
    <*> Sound card support
        <*> Advanced Linux Sound Architecture --->
            HD-Audio  --->
                -*- Allow dynamic codec reconfiguration
                [*] Support initialization patch loading for HD-audio
                <*> Build Realtek HD-audio codec support
                <*> Build HDMI/DisplayPort HD-audio codec support
                -*- Enable generic HD-audio codec parser
          (2048) Pre-allocated buffer size for HD-audio driver
```
### Touchpad

The laptop's touchpad will probably be an Elan 1200 model, which may not be detected by system tools, including lspci.

To enable it, use the following kernel configuration settings:

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
### USB

### SATA, PCI

### Webcam

### Bluetooth

### Keyboard

This model does not come with RGB keyboard lighting, only red backlight. Kernel 5.3 and later support changing the keyboard backlight brightness through the standard hotkeys, without any extra configuration.
