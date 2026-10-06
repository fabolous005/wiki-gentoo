<!-- source: https://wiki.gentoo.org/wiki/Dell_Inspiron_3501 | group: Gentoo Wiki (Main) | wiki-title: Dell Inspiron 3501 -->
---
title: Dell Inspiron 3501
url: https://wiki.gentoo.org/wiki/Dell_Inspiron_3501
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-06-16"
fingerprint: "3e0f891dd1a639cc"
license: CC BY-SA 4.0
---

# Dell Inspiron 3501

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article focus on getting all default hardware working with this model

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Firmware | Kernel version | Notes | 
|---|---|---|---|---|---|---|---|
| CPU | [Intel(R) Core(TM) i3-10005G1 CPU @ 1.20GHz](https://www.intel.com/content/www/us/en/products/sku/196588/intel-core-i31005g1-processor-4m-cache-up-to-3-40-ghz/specifications.html) | Works | N/A | N/A | N/A | 5.15.69 | Different CPU options are available for this laptop. | 
| GPU | [Intel® UHD Graphics](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915) Iris Plus Graphics G1 (Ice Lake) | Works | 8086:8a56 | [i915](https://wiki.gentoo.org/wiki/Intel) | icl\_dmc icl\_guc icl\_huc | 5.15.69 | Intel Corporation Iris Plus Graphics G1 (Ice Lake) | 
| RAM | RAM Module(s) 4GB SODIMM | Works | N/A | N/A | N/A | 5.15.69 | Two DIMM slots. Max memory 16GB. | 
| Hard Disk |  | Works |  | ahci [NVMe](https://wiki.gentoo.org/wiki/NVMe) | N/A | 5.15.69 |  | 
| [Wifi](https://wiki.gentoo.org/wiki/Wifi) | Qualcomm Atheros QCA9377 802.11ac Wireless Network Adapter | Works |  | [ath10k](https://cateee.net/lkddb/web-lkddb/ATH10K.html) | [ath10k](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/ath10k/QCA9377/hw1.0) | 5.15.69 |  | 
| Sound | Realtek ALC3204 | Works | N/A | snd\_hda\_intel snd\_hda\_codec\_realtek | N/A | 5.15.69 | N/A | 
| HDMI Sound | Intel Corporation Ice Lake-LP Smart Sound Technology Audio Controller | Works |  | snd\_hda\_intel snd\_hda\_codec\_hdmi | N/A | 5.15.69 | N/A | 
| [Touchpad](https://wiki.gentoo.org#Touchpad) | DELL [0A2B:00 06CB:CDD6](https://linux-hardware.org/?id=ps/2:06cb-cdd6-dell0a2b-00-06cb-cdd6-mouse) Touchpad | Works |  | intel-lpss i2c-hid |  | 5.15.69 |  | 

## Installation

### Firmware

Due to errata in the processor, it is advised to install and keep the CPU microcode up-to-date. See intel [intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode).

#### Graphics

Systems using Skylake, Broxton, or newer [Intel graphics](https://wiki.gentoo.org/wiki/Intel) will need additional [firmware](https://wiki.gentoo.org/wiki/Linux_firmware) from the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package:

`root #``emerge --ask sys-kernel/linux-firmware`
Alternatively, the blobs can be directly downloaded from the [Linux repository](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915) and put into /lib/firmware

##### DMC firmware

**D**isplay **M**icro**c**ontroller firmware provides support for advanced graphics low-power idle states.

##### GuC/HuC firmware

**G**raphics **µC**ontroller firmware offloads functions from the host driver. **H**EVC/H.265 **µC**ontroller firmware improves hardware acceleration of media decoding.

`root #``cp icl_guc_70.1.1.bin /var/lib/i915/`
**GPU firmware**

```
Device Drivers  --->
    Generic Driver Options  --->
        Firmware loader  --->
            -*- Firmware loading facility
            (i915/icl_dmc_ver1_09.bin ) Build named firmware blobs into the kernel binary
            (/lib/firmware) Firmware blobs root directory
```
**Graphics (Linux 5.15)**

```
Device Drivers  --->
    Graphics support  --->
        <*> /dev/agpgart (AGP Support)  --->
            --- /dev/agpgart (AGP Support)
            -*-   Intel 440LX/BX/GX, I8xx and E7x05 chipset support
        <*> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)  --->
            --- Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)
            [*]   Enable legacy fbdev support for your modesetting driver
        <*> Intel 8xx/9xx/G3x/G4x/HD Graphics
        [ ]   Enable alpha quality support for new Intel hardware by default
        ()    Force probe driver for selected new Intel hardware
        [*]   Enable capturing GPU state following a hang
        [*]     Compress GPU error state
        [*]   Always enable userptr support
        [ ]   Enable Intel GVT-g graphics virtualization host support
              Frame buffer Devices  --->
                  <*> Support for frame buffer devices  --->
                    [*]   EFI-based Framebuffer Support
                    <*>   Simple framebuffer support
              Console display driver support  --->
               [*] Framebuffer Console support
```
#### Ethernet

Install the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package:

`root #``emerge --ask sys-kernel/linux-firmware`
Alternatively, the blob can be directly downloaded from the [Linux repository](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/rtl_nic/rtl8106e-1.fw) and copied to /lib/firmware/rtl\_nic.

#### Wi-Fi

Install the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package:

`root #``emerge --ask sys-kernel/linux-firmware`
Alternatively, the blobs can be directly downloaded from the [Linux repository](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/ath10k/QCA9377/hw1.0).

### Kernel

#### GPU firmware

**GPU firmware**

```
Device Drivers  --->
    Generic Driver Options  --->
        Firmware loader --->
            -*- Firmware loading facility
            (i915/kbl_dmc_ver1_04.bin) Build named firmware blobs into the kernel binary
            (/lib/firmware) Firmware blobs root directory
```
#### CPU

**CPU**

```
Processor type and features  --->
    [*] Machine Check / overheating reporting
    [*]   Intel MCE Features
    [ ]   AMD MCE Features
    Processor family (Core 2/newer Xeon)  --->
        ( ) Opteron/Athlon64/Hammer/K8
        ( ) Intel P4 / older Netburst based Xeon
        (X) Core 2/newer Xeon
        ( ) Intel Atom
        ( ) Generic-x86-64
```
#### Hard disk

```
Device Drivers --->
   <*> Serial ATA and Parallel ATA drivers  --->
      [*] ATA ACPI Support
   Select the driver for the SATA controller, e.g.:
      <*> AHCI SATA support (ahci)
         [*]   ATA SFF support (for legacy IDE and PATA) 
         [*]     ATA BMDMA support      
      <*> Intel ESB, ICH, PIIX3, PIIX4 PATA/SATA support (ata_piix)
   SCSI device support  ---> 
      <*> SCSI device support
      <*> SCSI disk support
    NVME Support  --->
        [*] NVM Express block device
        [Opt] NVMe hardware monitoring
```
#### Wi-Fi and Ethernet

**Wi-Fi**

```
[*] Networking support  --->
      -*-   Wireless  --->
          <*>   cfg80211 - wireless configuration API
          [*]     support CRDA
          <*>   Generic IEEE 802.11 Networking Stack (mac80211)
          [*]   Minstrel
          [*]   Enable LED triggers
        <*>   RF switch subsystem support  --->
Device Drivers --->
    Generic Driver Options  --->
            Firmware loader  --->
               -*- Firmware loading facility
                (ath10k/QCA9377/hw1.0/board-2.bin ath10k/QCA9377/hw1.0/firmware-5.bin   regulatory.db  regulatory.db.p7s)
   [*] Network device support  --->
      [*]   Ethernet driver support  --->  
          [*]   Realtek devices
          <*>     Realtek 8169/8168/8101/8125 ethernet support   
      Wireless LAN  --->
          [*]   Atheros/Qualcomm devices
          <*>     Atheros 802.11ac wireless cards support    
          <*>     Atheros ath10k PCI support
```
#### Sound

snd-hda-intel driver is used . To force sof driver with kernel parameter options snd-intel-dspcfg dsp\_driver=3 but didnt see noticeable difference

**Sound**

```
Device Drivers --->
   <*> Sound card support  --->
   <*>   Advanced Linux Sound Architecture  --->
   <*>   HR-timer backend support
   [*]   Sound Proc FS Support
   <*>   Sequencer support
   [*]     Use HR-timer as default sequencer timer
   [*]   Generic sound devices  --->
   [*]   PCI sound devices  --->
   HD-Audio  --->
       <*> HD Audio PCI
       <*> Build Realtek HD-audio codec support
       <opt> Build HDMI/DisplayPort HD-audio codec support
       -*- Enable generic HD-audio codec parser
```
#### Multi-function driver

**Multi-function driver**

```
Device Drivers  --->
     Multifunction device drivers  --->
        <*> Intel ICH LPC
        <*> Intel Low Power Subsystem support in ACPI mode
        <*> Intel Low Power Subsystem support in PCI mode
```
#### Power management

```
        Power management and ACPI options  --->
          [ ] Suspend to RAM and standby
          [*] ACPI (Advanced Configuration and Power Interface) Support  --->                                                                                                          
          [*] Power Management Timer Support
          [*] Platform Runtime Mechanism Support
                CPU Frequency scaling  --->
                      < >   'ondemand' cpufreq policy governor
                      < >   'conservative' cpufreq governor
                      [*]   Intel P state control
          [*] Cpuidle Driver for Intel Processors
```
#### Touchpad

kernel says DELL0A2B i2c\_designware which used hid-multitouch driver

**Touchpad**

```
Device Drivers  --->
    Input device support  --->
      <*>   Event interface
  -*- Pin controllers  --->
         [*]   Intel Ice Lake PCH pinctrl and GPIO driver
    -*- I2C support   
      I2C Hardware Bus support  --->
            [*] Intel 82801 (ICH/PCH)
            [*] Synopsys DesignWare Platform
    HID support  --->
        [*]   /dev/hidraw raw HID device support                                   
        [*]   User-space I/O driver support for HID subsystem                       
        [*]   Generic HID driver
        Special HID drivers  --->
              [*] HID Multitouch panels
        I2C HID support  --->
              [*] HID over I2C transport layer ACPI driver
```
