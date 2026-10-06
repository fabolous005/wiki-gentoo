<!-- source: https://wiki.gentoo.org/wiki/Gigabyte_X570-UD | group: Gentoo Wiki (Main) | wiki-title: Gigabyte X570-UD -->
---
title: Gigabyte X570-UD
url: https://wiki.gentoo.org/wiki/Gigabyte_X570-UD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: bf0b8816d8a6b98c
license: CC BY-SA 4.0
---

# Gigabyte X570-UD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- What to do about sensors?
- Is everything in lspci accounted for?
- Most of the CPU-specific stuff should be folded into [Ryzen](https://wiki.gentoo.org/wiki/Ryzen)

## General Information

This article details the Gigabyte X570-UD motherboard, providing Linux kernel configuration hints and outlines what to expect (including known hardware and firmware issues). The motherboard has an X570 chipset and AM4 CPU socket compatible with [Ryzen](https://wiki.gentoo.org/wiki/Ryzen) CPUs. This page is for the “rev 1.0” hardware, the only revision at the time of writing.

## Hardware Specs

| CPU Support | AM4 (znver2; supports Ryzen 7 and 9 models) | 
|---|---|
| Memory | 4× DDR 4 slots, maximum 128GB DDR4-3200, XMP up to DDR4-4000 | 
| Expansion Slots |  | 
| Internal Storage |  | 
| Connectivity |  | 

## Kernel Configuration

### CPU

This list is only a guideline, in particular note that current kernels (5.4) need to *disable* SME due to an incompatibility with the [amdgpu](https://wiki.gentoo.org/wiki/AMDGPU#AMD_Secure_Memory_Encryption) driver.

```
Processor type and features  --->
    [*] DMA memory allocation support
    [*] Symmetric multi-processing support
    [*] Support x2apic
    [*] Avoid speculative indirect branches in kernel
    [*] x86 CPU resource control support
    Processor family (AMD Zen 2)
    [*] Machine Check / overheating reporting
        [*]   AMD MCE features
    [*] CPU microcode loading support
    [*]   AMD microcode loading support
    [*] AMD Secure Memory Encryption (SME) support
    [ ]   Activate AMD Secure Memory Encryption (SME) by default
    [*] MTRR (Memory Type Range Register) support
        [*]   x86 PAT support
    [*] x86 architectural random number generator
    [*] Supervisor Mode Access Prevention
Power management and ACPI options  --->
    CPU Frequency scaling  --->
        Default CPUFreq governor (schedutil)
        <*>   ACPI Processor P-States driver
        <*>   AMD frequency sensitivity feedback powersave bias
[*] Virtualization  --->
    <M>   Kernel-based Virtual Machine (KVM) support
    <M>     KVM for AMD processors support
    [*]       AMD Secure Encrypted Virtualization (SEV) support
Device Drivers  --->
    Character devices  --->
        <*> TPM Hardware Support  --->
            [*]   TPM HW Random Number Generator support
            <*>   TPM 2.0 CRB Interface
    -*- Hardware Monitoring support  --->
        <*>   AMD Family 10h+ temperature sensor
    [*] IOMMU Hardware Support  --->
        [*]   AMD IOMMU support
        <*>     AMD IOMMU Version 2 driver
        [*]   Support for Interrupt Remapping
```
The CCP is present but non-functional due to a broken BIOS:

```
-*- Cryptographic API  --->
    [*]   Hardware crypto devices  --->
        [*]   Support for AMD Secure Processor
        <*>     Secure Processor device driver
        [*]       Cryptographic Coprocessor device
        <*>         Encryption and hashing offload support
        [*]       Platform Security Processor (PSP) device
```
### Storage Devices

The NVMe driver has support for temperature sensors as of Linux 5.5.

**01:00.0 Non-Volatile memory controller: (your device name here)**

```
Device Drivers  --->
    NVME Support  --->
        <*> NVM Express block device
        [*] NVMe hardware monitoring
```
If you have SATA disks, the usual driver works. There are six physical ports spread across four PCIe devices, presumably to make virtualisation setups simpler.

**0{6,7,b,c}:00.0 SATA controller \[0106\]: Advanced Micro Devices, Inc. \[AMD\] FCH SATA Controller \[AHCI mode\] \[1022:7901\] (rev 51)**

```
Device Drivers  --->
    <*> Serial ATA and Parallel ATA drivers (libata)  --->
        <*>   AHCI SATA support
```
### Sound

```
Device Drivers  --->
    <*> Sound Card Support  --->
        <*> Advanced Linux Sound Architecture  --->
            HD-Audio  --->
                <*> HD Audio PCI
                <*> Build Realtek HD-audio codec support
```
### Ethernet

```
Device Drivers  --->
    Networking support  --->
        [*] Network device support  --->
            [*] Ethernet driver support  --->
                [*]   Realtek devices
                <*>     Realtek 8169/8168/8101/8125 ethernet support
```
The NIC can have its firmware (rtl\_nic/rtl8168h-2.fw) built into the kernel to silence a warning, though in practice it doesn't affect functionality.

### Peripherals

The XHCI driver covers all USB functionality, even the USB 2.0 ports.

```
Device Drivers  --->
    [*] USB support  --->
        [*]   PCI based USB host interface
        <*>   xHCI HCD (USB 3.0) support
```
### Sensors

The following is correct for Linux 5.6:

- The CPU supports the k10temp driver with ⅛°C resolution. Linux 5.6 also supports Fam17h voltage, current and per-core-complex temperatures.
- ACPI Thermal Zone readings appear to be invalid and unchanging, at 16.8°C. dmesg contains warnings about “Invalid passive threshold” which might be related, but according to web search results this appears to be a BIOS bug.
- The X570-UD has an IT8688 chip on an i2c-piix4 bus. sensors-detect can see the IT8688 but [Linux **does not** have a driver for it](https://github.com/lm-sensors/lm-sensors/issues/154). The BIOS provides a configuration GUI for its various fan settings and temperature trip points.

Raw unfiltered sensor values for reference, using kernel 5.5.2 (with the k10temp patches from 5.6):

`user $``sensors -jc /dev/null | sed -n '/k10temp\|acpitz/,/^   },$/ p'````
   "k10temp-pci-00c3":{
      "Adapter": "PCI adapter",
      "Vcore":{
         "in0_input": 1.494
      },
      "Vsoc":{
         "in1_input": 1.013
      },
      "Tdie":{
         "temp1_input": 33.500
      },
      "Tctl":{
         "temp2_input": 33.500
      },
      "Tccd1":{
         "temp3_input": 33.250
      },
      "Tccd2":{
         "temp4_input": 33.250
      },
      "Icore":{
         "curr1_input": 11.000
      },
      "Isoc":{
         "curr2_input": 7.500
      }
   },
   "acpitz-acpi-0":{
      "Adapter": "ACPI interface",
      "temp1":{
         "temp1_input": 16.800,
         "temp1_crit": 20.800
      },
      "temp2":{
         "temp2_input": 16.800,
         "temp2_crit": 20.800
      }
   },
```
## Other details

### lspci output

### dmesg excerpts

The usual set of x86 hardware vulnerabilities have to be worked around:

The BIOS wants to know if we're running Linux, for some reason:

Some messages about badly-provisoned onboard PCIe devices:

Buggy ACPI:

Buggy USB 3.1 controllers:

There is a watchdog timer chip physically present, but not usable:

### Known hardware issues

- Many of AMD's AM4 CPUs come with an animated RGB LED heatsink fan, which some may find irritating. It can be disabled (persistently) using the included USB cable and [cm-rgb](https://github.com/gfduszynski/cm-rgb).
- If boot ends in a hard reset more than once in a row (e.g. due to a kernel misconfiguration), the firmware will reset its settings back to factory defaults. To lessen the impact of this, use the “Save Profile” option in the BIOS menu to keep a backup of known-working settings, and use [kexec](https://wiki.gentoo.org/wiki/Kexec) to test new kernels and avoid doing a cold reboot.
- Do *not* change Default ASPM policy from "BIOS default" in the kernel, as the power saving modes will cause various peripherals (including NVMe) to go missing during boot.
